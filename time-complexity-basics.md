## Constraints i Big O vremenska složenost

U računarskom inženjeringu, efikasnost koda se ne meri subjektivnim osećajem, već matematičkim modelom pod nazivom **Big O vremenska i prostorna složenost**. 

Na platformama kao što je LeetCode, tvoj kod se izvršava na udaljenom serveru gde važe stroga pravila za maksimalno vreme izvršavanja (obično **1 do 2 sekunde** po test primeru).

### 1. Šta je $N$ i gde se nalazi?
Promenljiva **$N$** predstavlja **veličinu ulaznih podataka** koje tvoja funkcija treba da obradi.
*   Ako je ulaz niz brojeva (`int[] nums`), $N$ je dužina tog niza (`nums.length`).
*   Ako je ulaz tekst (`String s`), $N$ je broj karaktera u tom tekstu (`s.length()`).
*   Ako je ulaz graf ili matrica, $N$ može predstavljati broj čvorova ili ćelija.

Na LeetCode interfejsu, ova ograničenja se nalaze na dnu leve kolone (kartica **Description**), odmah ispod primera, pod naslovom **Constraints**.

### 2. Zlatno pravilo: $10^8$ operacija po sekundi
Moderne serverske procesorske jedinice mogu da izvrše približno **$10^8$ (100 miliona) bazičnih operacija u sekundi** za potrebe izvršavanja koda na online sistemima.

To znači: ako tvoj algoritam zahteva više od $10^8$ operacija za zadato $N$, server će prekinuti izvršavanje i vratiti grešku **Time Limit Exceeded (TLE)**.

---

### 3. Big O Refresher sa Java primerima koda

Sledeća tabela i primeri pokazuju kako različite Big O složenosti troše procesorsko vreme u zavisnosti od rasta ulaza $N$.

| Big O Oznaka | Tip Složenosti | Broj operacija za $N = 10^5$ | Dozvoljeno na LeetCode za $N = 10^5$? |
| :--- | :--- | :--- | :--- |
| **$O(1)$** | Konstantno vreme | 1 (fiksno, bez obzira na $N$) | **Da** (trenutno) |
| **$O(\log N)$** | Logaritamsko vreme | $\approx 17$ operacija | **Da** (izuzetno brzo) |
| **$O(N)$** | Linearno vreme | $10^5$ (100.000) operacija | **Da** (veoma brzo) |
| **$O(N \log N)$** | Linearitamsko vreme | $\approx 1.7 \times 10^6$ operacija | **Da** (standard za sortiranje) |
| **$O(N^2)$** | Kvadratno vreme | $10^{10}$ (10 milijardi) operacija | **NE (Time Limit Exceeded)** |
| **$O(2^N)$** | Eksponencijalno vreme | $2^{100000}$ (beskonačno) | **NE (Srušiće server)** |

#### Java primeri koda za svaku složenost:

*   **$O(1)$ — Konstantno vreme (Constant Time)**
    Operacija se izvršava u jednom koraku, bez obzira na veličinu niza.
    ```java
    // Pristup elementu preko indeksa je uvek O(1)
    int getFirstElement(int[] nums) {
        return nums[0]; 
    }
    ```

*   **$O(\log N)$ — Logaritamsko vreme (Logarithmic Time)**
    Kod svakog koraka, prostor pretrage se polovi. Tipičan primer je binarna pretraga na sortiranom nizu.
    ```java
    int binarySearch(int[] sortedNums, int target) {
        int left = 0, right = sortedNums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (sortedNums[mid] == target) return mid;
            if (sortedNums[mid] < target) left = mid + 1;
            else right = mid - 1;
        }
        return -1;
    }
    ```

*   **$O(N)$ — Linearno vreme (Linear Time)**
    Broj koraka raste proporcionalno sa rastom ulaza $N$. Jednostruka petlja.
    ```java
    boolean containsNumber(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) return true; // Prolazi kroz ceo niz jednom
        }
        return false;
    }
    ```

*   **$O(N \log N)$ — Linearitamsko vreme**
    Karakteristično za efikasne algoritme sortiranja (`Quick Sort`, `Merge Sort`). Svako sortiranje niza u Javi koristi ovu složenost.
    ```java
    void sortArray(int[] nums) {
        java.util.Arrays.sort(nums); // Izvršava se u O(N log N) vremena
    }
    ```

*   **$O(N^2)$ — Kvadratno vreme (Quadratic Time)**
    Broj operacija raste sa kvadratom ulaza. Dve ugnježdene petlje. 
    ```java
    // Brute-force pretraga parova (svaki sa svakim)
    void printAllPairs(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < nums.length; j++) {
                System.out.println(nums[i] + ", " + nums[j]);
            }
        }
    }
    ```
