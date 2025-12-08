# Számítógépek adattárolása – Elméleti anyag

## 1. Fizikai adattárolás alapfogalmai

### **1.1. Szektor (Sector)**
- A legkisebb fizikai tárolási egység egy HDD/SSD-n.
- Általában **512 bájt** vagy **4096 bájt (4K)** méretű.
- Az operációs rendszer ezen egységeken keresztül ír és olvas.

### **1.2. Klaszter (Cluster / Allocation Unit)**
- A fájlrendszer által használt legkisebb foglalási egység.
- Egy klaszter több szektorból áll (pl. 4K, 8K, 32K).
- Egy fájl legalább 1 klasztert foglal, akkor is, ha kisebb.

### **1.3. Blokk (Block)**
- Linux rendszereknél a klaszter megfelelője.

---

## 2. Partíciók és partíciós táblák

### **2.1. Mi az a partíció?**
- A fizikai lemez logikailag felosztott része.
- Azt határozza meg, hogy melyik terület milyen célra használható.
- Példák:
  - rendszerpartíció (Windows: C:)
  - EFI rendszerpartíció (UEFI boot)
  - Linux: /, /home, swap
  - adattároló partíciók

---

## 3. Partíciós tábla típusok

### **3.1. MBR (Master Boot Record)**
- Régebbi, klasszikus partíciós tábla.
- Maximális lemezméret: **2 TB**
- Maximális partíciók száma: **4 elsődleges**
  - Ha ennél több kell: 3 elsődleges + 1 kiterjesztett → abban logikai partíciók
- Boot szektor: az első 512 bájt tartalmazza a bootloadert.

**Előnye:** kompatibilis  
**Hátránya:** korlátozott, elavult

---

### **3.2. GPT (GUID Partition Table)**
- Modern partíciós tábla, UEFI rendszerekhez.
- Maximális lemezméret: **9.4 Zettabájt** (gyakorlatilag korlátlan)
- Partíciók száma: **128 alapból**
- Biztonságosabb (CRC ellenőrzés, tartalék másolat a lemez végén)

**Kötelező:**  
- UEFI boot Windows 10/11 esetén  
- 2 TB feletti lemezeknél


---

## 4. Fájlrendszerek

### **Mi az a fájlrendszer?**
Az a struktúra, amely meghatározza:
- hogyan tárolódnak a fájlok,
- hogyan épül fel a könyvtárstruktúra,
- hogyan kezeli a jogosultságokat,
- hogyan történik a hibajavítás.

---

### **Leggyakoribb fájlrendszerek**

### **4.1. NTFS (Windows)**
- Modern Windows alapértelmezett fájlrendszere.
- Támogatja:
  - fájljogosultságok,
  - journal (naplózás → kevésbé sérül meg),
  - tömörítés,
  - titkosítás (EFS).

**Hátrány:** Linux írni tudja, de nem natív; macOS csak olvasni tud.

---

### **4.2. FAT32**
- Régi, nagyon kompatibilis.
- Maximum file méret: **4 GB**
- Maximum partíció: **2 TB**

**USB-khez jó**, de modern rendszerekhez nem.

---

### **4.3. exFAT**
- FAT32 modern utódja.
- Nincs 4 GB fájlkorlát.
- Platformfüggetlen (Windows, Linux, macOS kezeli).

**Legjobb pendrive-ra és SD-kártyára.**

---

### **4.4. EXT4 (Linux)**
- Legelterjedtebb Linux fájlrendszer.
- Gyors, stabil, journal-os.
- Maximum fájlméret: **16 TB**
- Maximum partíció: **1 EB**

---

## 5. Töredezettség (Fragmentation)

### **5.1. Mi a töredezettség?**
Töredezettségről akkor beszélünk, amikor:
- egy fájl nem egyben, hanem sok darabban tárolódik a lemezen.

### **5.2. Miért baj ez?**
- HDD-knél: a fejnek ide-oda kell ugrálnia → **lassulás**
- SSD-knél: a hozzáférési idő azonos → **nem okoz lassulást**

### **5.3. Defragmentálás**
- HDD: erősen ajánlott
- SSD: **tilos**, helyette „TRIM” művelet kell (automatikus)

---

## 6. SSD és HDD működési különbségek

### **6.1. HDD**
- forgó lemezek + író/olvasó fej
- lassabb  
- érzékeny rezgésre  
- olcsóbb  
- töredezettség lassít

### **6.2. SSD**
- NAND flash memória  
- sokkal gyorsabb  
- nem érzékeny rezgésre  
- nincs töredezettség probléma  
- TRIM szükséges

---

## 7. Bootfolyamat kapcsolata a partíciókkal

### **7.1. BIOS + MBR rendszer**
- BIOS betölti az MBR bootloadert
- Az betölti az operációs rendszer betöltőjét

### **7.2. UEFI + GPT rendszer**
- UEFI a **EFI System Partition-t** (ESP) keresi
- Itt található:
  - Windows Boot Manager
  - GRUB
  - egyéb bootloaderek

---

## 8. Gyakorlati példák partíciókra

### **Windows UEFI telepítése esetén:**
- EFI System Partition (100–300 MB)
- MSR (Microsoft Reserved, 16 MB)
- C: NTFS (a rendszer)
- Opcionálisan: adatpartíció

### **Linux telepítése esetén:**
- EFI (ha UEFI)
- root (/)  
- home (/home)  
- swap (vagy swapfile)

---

# Rövid összefoglaló
- **Szektor:** legkisebb fizikai egység  
- **Klaszter/blokk:** fájlrendszer foglalási egysége  
- **Partíció:** logikai lemezdarab  
- **MBR:** régi, max 2 TB  
- **GPT:** modern, UEFI, nagy lemezekhez  
- **Fájlrendszer:** szabályrendszer a fájlok tárolására  
- **Töredezettség:** HDD-n lassít, SSD-n nem számít  
- **Fő fájlrendszerek:** NTFS, exFAT, EXT4, APFS  

