## Mentalni modeli — Zanatlijski vs. Sistemski inženjerski pristup

Da bismo postigli maksimalnu efikasnost u radu, moramo jasno razlikovati dva mentalna modela rešavanja problema. Oba imaju svoju vrednost, ali u produkciji i komercijalnom radu, balans mora biti pomeren ka inženjerskom pristupu.

| Dimenzija | Zanatlijski (Majstorski) Pristup | Sistemski (Inženjerski) Pristup |
| :--- | :--- | :--- |
| **Glavni pokretač** | Kreativnost, unikatnost, dokazivanje lične sposobnosti kroz rešenje. | Optimizacija resursa (vreme, budžet, stabilnost, ROI). |
| **Metodologija** | "Napraviću ovo sam od nule (peške) jer znam da mogu." | "Da li već postoji dokazano, standardno rešenje koje mogu da integrišem?" |
| **Trošak vremena** | Visok. Mnogo vremena odlazi na improvizaciju i rešavanje nepredviđenih grešaka. | Nizak. Koriste se standardizovani blokovi i protokoli. |
| **Održavanje** | Teško. Rešenje je unikatno; ako kreator nije tu, niko drugi ne može da ga popravi. | Lako. Rešenje prati industrijske standarde i dokumentovano je. |
| **Upravljanje rizikom** | Fokus je na tome da sistem proradi "sada" u kontrolisanim uslovima. | Fokus je na tome kako sistem reaguje na kvarove i ko ga može servisirati za 5 godina. |

---

### Primer 1: Softverski razvoj (Tvoj domen)

#### Primer 1.1: Višenitni bafer za telemetrijske podatke
**Zadatak:** Potrebno je implementirati thread-safe redosled (Queue) za prijem telemetrijskih paketa sa IoT uređaja (Kraton server) u realnom vremenu, tako da više niti (threads) može istovremeno da upisuje i čita podatke bez kvarenja memorije.

*   **Zanatlijski pristup:**
    Programer odlučuje da napiše svoju verziju povezane liste (`LinkedList`) od nule, piše sopstvene metode za sinhronizaciju, zaključavanje niti (`synchronized` blokovi, `ReentrantLock`), i ručno upravlja memorijskim pokazivačima kako bi osetio "kontrolu nad kodom".
    *   *Ishod:* Izgubljeno 4 do 5 sati rada. Kod radi na prvi pogled, ali pod velikim opterećenjem u produkciji počinje da pravi povremene "deadlock" greške koje je nemoguće reprodukovati u lokalnom okruženju.
*   **Inženjerski pristup:**
    Programer prepoznaje problem konkurentnosti. Umesto pisanja koda, on analizira zahteve i donosi odluku da iskoristi već postojeću, milion puta testiranu strukturu iz standardne Java biblioteke — `ConcurrentLinkedQueue` ili `ArrayBlockingQueue`.
    *   *Ishod:* Implementacija završena za 2 minuta. Kod je stabilan, visoko optimizovan na nivou JVM-a, bezbedan za rad sa nitima i odmah spreman za produkciju. Vreme je sačuvano za projektovanje arhitekture sistema.

#### Primer 1.2: Komunikacija između mikrokontrolera i centralnog servera
**Zadatak:** Razviti protokol za slanje statusa pumpe (Hydro Node) ka centralnom Kraton serveru preko mreže.

*   **Zanatlijski pristup:**
    Izmišljanje sopstvenog binarnog protokola za pakovanje bajtova (npr. prvi bajt je start, drugi je status pumpe, treći je checksum). Pisanje prilagođenog parsera na serveru koji čita sirove TCP sokete i ručno bajt-po-bajt rekonstruiše poruku.
    *   *Ishod:* Protokol je ekstremno kompaktan, ali zahteva dane testiranja. Svaka promena u statusu (npr. dodavanje još jednog senzora) zahteva prepisivanje parsera i na mikrokontroleru i na serveru. Dokumentacija ne postoji.
*   **Inženjerski pristup:**
    Korišćenje standardnog, laganog industrijskog protokola kao što je **MQTT** sa JSON formatom poruka. Na serveru se podiže standardni broker (npr. *Mosquitto*), a podaci se parseruju kroz standardne biblioteke.
    *   *Ishod:* Sistem je postavljen i konfigurisan za pola sata. Dodavanje novih senzora zahteva samo proširenje JSON strukture bez menjanja mrežnog koda. Protokol je robustan, podržava automatsko re-konektovanje i prepoznat je od strane bilo kog IoT inženjera na svetu.

---

### Primer 2: Elektro-hardverski inženjering (Za zajednički rad sa ocem)

#### Primer 2.1: Automatsko prebacivanje napajanja (ATS - Automatic Transfer Switch)
**Zadatak:** Na farmi pilića obezbediti automatsko prebacivanje napajanja na rezervni agregat u slučaju nestanka struje, uz praćenje parametara napona na mreži kako se ventilatori ne bi ugasili.

*   **Zanatlijski (Majstorski) pristup:**
    Električar projektuje unikatno upravljačko kolo koristeći pojedinačne vremenske releje, sklopke, pomoćne kontakte i gomilu ručno povezane signalne ožičenja unutar razvodnog ormara. Svaka veza se ručno lemi ili šrafi, a logika kašnjenja se podešava fizičkim točkićima na relejima.
    *   *Ishod:* Sistem je unikatnog dizajna i predstavlja demonstraciju vrhunske manuelne veštine. Rad je trajao 12 sati. Međutim, ako se nakon godinu dana desi kvar na nekom releju dok otac nije tu, lokalni električar na farmi ne može da popravi sistem jer ne postoji šema, a logika kola je samo u glavi autora.
*   **Inženjerski pristup:**
    Projektant koristi industrijski programabilni pametni relej (npr. *Siemens LOGO!* ili namenski ATS kontroler) i standardnu, fabrički sertifikovanu sklopku sa mehaničkom blokadom. Logika prebacivanja se programira softverski u kontroleru kroz jednostavan dijagram.
    *   *Ishod:* Montaža i povezivanje gotovih komponenti traje 1.5 sat. Sistem koristi standardizovane industrijske protokole. Ako se kontroler pokvari, bilo koji licencirani električar može kupiti identičan zamenski modul u prodavnici, učitati softverski fajl i vratiti farmu u rad za 15 minuta.

#### Primer 2.2: Regulacija brzine ventilatora u hali (Upravljanje motorom)
**Zadatak:** Omogućiti promenu brzine tri trofazna ventilatora u zavisnosti od temperature u hali.

*   **Zanatlijski (Majstorski) pristup:**
    Povezivanje motora preko unikatnog transformatorskog regulatora sa više izvoda (stepenasti regulator) i ručnih grebenastih sklopki. Da bi se automatizovalo, dodaju se pomoćni kontaktori koji fizički prebacuju izvode transformatora na osnovu signala sa termostata.
    *   *Ishod:* Ormar je ogroman, pun teških komponenti koje disipiraju veliku količinu toplote. Regulacija je gruba (samo u koracima, npr. 30%, 60%, 100%). Svaka promena brzine stvara strujne udare na mreži koji skraćuju životni vek motora.
*   **Inženjerski pristup:**
    Ugradnja standardnog **Frekventnog regulatora (VFD - Variable Frequency Drive)** na DIN šinu. VFD se povezuje direktno na motor, a njegova brzina se fino reguliše linearno (od 0% do 100%) preko standardnog 0-10V analognog signala sa kontrolera ili digitalno preko Modbus RTU protokola.
    *   *Ishod:* Montaža je brza i čista. Motor ima ugrađenu bimetalnu zaštitu, zaštitu od gubitka faze i meki start (nema strujnih udara). Potrošnja struje je drastično smanjena jer ventilatori rade tačno onom brzinom koja je potrebna. Zamena neispravnog VFD-a je stvar odvrtanja nekoliko šrafova na DIN šini.

#### Primer 2.3: Prenos signala sa temperaturnih senzora na velikim distancama
**Zadatak:** Povezati temperaturne senzore iz tri različite hale (udaljenost do 100 metara od centralnog ormara) sa kontrolorom.

*   **Zanatlijski (Majstorski) pristup:**
    Razvlačenje običnih dvožilnih kablova direktno od NTC otpornika (senzora) u halama do analognih ulaza mikrokontrolera. Pisanje kompleksnog koda za kalibraciju otpornosti, filtriranje šuma i kompenzaciju pada napona na dugom kablu.
    *   *Ishod:* Kada se uključe veliki ventilatori ili motori, njihovo elektromagnetno polje indukuje šum u senzorskim kablovima. Temperatura u aplikaciji počinje da "skače" za po nekoliko stepeni gore-dole. Sistem zahteva konstantno softversko peglanje i hardverske filtere.
*   **Inženjerski pristup:**
    Korišćenje senzora sa ugrađenim transmiterom koji komuniciraju preko **RS485 industrijske magistrale** koristeći **Modbus RTU** protokol (ili prenos preko strujne petlje 4-20mA). Svi senzori se povezuju na samo jedan četvorožilni kabl (širmovani paričnjak) koji ide od hale do hale u nizu (Daisy-chain).
    *   *Ishod:* Protokol ima ugrađenu kontrolu greške (CRC). Šum ne utiče na digitalni signal. Očitavanje je tačno u decimalu na udaljenosti do 1200 metara. Nema potrebe za kalibracijom u kodu; server jednostavno čita registre senzora.

#### Primer 2.4: Kontrola nivoa tečnosti u rezervoaru (Hydro Node)
**Zadatak:** Isključiti pumpu kada nivo vode u bazenu padne ispod kritičnog nivoa, kako pumpa ne bi radila na suvo i pregorela.

*   **Zanatlijski (Majstorski) pristup:**
    Izrada unikatnog mehaničkog plovka sa magnetom i reed prekidačem (reed switch) u plastičnoj cevi, sa ručno lemljenim tranzistorskim pojačavačem koji aktivira relej sklopke pumpe.
    *   *Ishod:* Nakon nekoliko meseci u vodi, dolazi do korozije na spojevima, ili se plovak zaglavi zbog kamenca. Pumpa ostaje bez vode, sistem ne detektuje kvar i motor pumpe pregoreva. Gubitak je višestruko veći od cene bilo kog senzora.
*   **Inženjerski pristup:**
    Ugradnja standardnog **DIN-releja za kontrolu nivoa tečnosti** (npr. *Carlo Gavazzi* ili *Schrack*) koji radi na principu provodljivosti vode. Na relej se povezuju standardne sonde od nerđajućeg čelika (prohroma) otporne na koroziju i kamenac.
    *   *Ishod:* Relej ima ugrađeno kašnjenje protiv talasanja vode i radi na niskom, bezbednom naponu. Sistem je imun na spoljne uslove, montira se za 15 minuta na DIN šinu i garantuje stopostotnu zaštitu pumpe. Bilo koja komponenta je lako zamenjiva industrijskim ekvivalentom.
