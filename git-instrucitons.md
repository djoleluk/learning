# 🛠️ Git Osnove - Podsetnik

Kratak pregled najčešćih Git komandi za svakodnevni rad na projektu.

### 1. Povezivanje novog projekta na Git
# Inicijalizacija lokalnog repozitorijuma
git init

# Povezivanje sa udaljenim serverom (npr. GitHub/GitLab)
git remote add origin URL_TVOG_PROJEKTA

# Branch to main 
git branch -M main

# Prvo slanje koda na glavni (main) branch
git add .
git commit -m "Initial commit"
git push -u origin main

### 2. Provera grana (Branches)
# Lista lokalnih grana
git branch

# Lista svih grana (i lokalnih i na serveru)
git branch -a

### 3. Kreiranje i prelazak na novu granu
# Kreiranje nove grane i automatski prelazak na nju
git checkout -b ime-nove-grane

### 4. Povratak na staru granu (npr. main)
# Prelazak na postojeću granu
git checkout main

### 5. Spajanje grana (Merge)
# Prvo pređi na granu u koju želiš da ubaciš izmene (npr. main)
git checkout main

# Spoji drugu granu sa trenutnom
git merge ime-grane-koju-spajaš

### 6. Brisanje grana
# Brisanje lokalne grane (nakon što je spojena)
git branch -d ime-grane

# Forsirano brisanje lokalne grane (čak i ako nije spojena)
git branch -D ime-grane

# Brisanje grane na udaljenom serveru
git push origin --delete ime-grane

### 7. Sinhronizacija i čišćenje
# Osvežavanje informacija sa servera (povlačenje info o novim granama)
git fetch

# Povlačenje nove grane sa servera koja ne postoji lokalno
git checkout ime-grane

# Čišćenje lokalnih referenci na grane koje su obrisane na serveru (Pruning)
git fetch -p

# Provera statusa udaljenih grana i poređenje sa lokalnim
git remote show origin

### 8. Kernel Update (Upstream Workflow)
Ovo koristimo da bismo povukli popravke iz `kraton-node-template` (Kernela) u specifične aplikacije (Distribucije).

# 1. Povezivanje sa Kernelom (radi se samo jednom po projektu)
git remote add upstream git@github.com:djoleluk/kraton-node-template.git

# 2. Provera novih izmena u Kernelu
git fetch upstream

# 3. Upoređivanje (vidiš šta će se promeniti)
git diff HEAD..upstream/main --stat

# 4. "Ulivanje" Kernela u tvoju trenutnu granu
git merge upstream/main

# 5. Slanje ažurirane distribucije na njen GitHub
git push origin [IME_GRANE]
