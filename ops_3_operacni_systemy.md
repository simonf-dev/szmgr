# Operační systémy
Vypracoval @simonf-dev
> Operační systém UNIX, fungování jádra, správa paměti a zařízení jádrem. Základní konfigurace, správa uživatelů, síťové služby. Principy vývoje a vývojové prostředí v UNIXu, práce s procesy, se soubory, vstupní/výstupní operace. Operační systém MS Windows, základní konfigurace, správa uživatelů, souborový systém NTFS, HW zařízení a ovladače, základy skriptování. Příklady z praxe pro vše výše uvedené

## Operační systém UNIX, fungování jádra, správa paměti a zařízení jádrem
- víceuživatelský, víceúlohový systém, který podporuje příkazový řádek a hierarchický souborový systém
- kernel je základní část systému, stará se o řízení hardvaru, plánování úloh, paměť, procesy atd.
- jádro je načítané při spuštění systémum start se skládá z kroků:
  - identifikují se zařízení jako CPU, disky a spouští se primární zavaděč
  - primární zavaděč je malý program na začátku boot disku
  - poté je spuštěn sekundární zavaděč, ten má načíst jádro
  - nastartuje se jádro, které vytvoří proces 0, inicializuje se periferie, virtuální paměť, CPU, konzole, sběrnice, připojí se kořenového systému souboru a potom se začne inicializovat uživatelský prostor
- jádro je monolitické, takže má většinu svých funkcionalit v jádře, další typy jsou modulární jádro a mikrojádro
- v UNIX systémech je snaha všechno reprezentovat jako soubory (zařízení, sockety, informace o procesech)


### Správa paměti
- paměť spravována pomocí virtuálních adres, ty jsou mapované na fyzické adresy
- každý proces má svou tabulku virtuálních adres, která se používá pro překlad
- paměť jádra je namapovaná do každé tabulky
- většinou víceúrovňová adresace -> první část adresy ukazuje na konkrétní adresu tabulky stránek, další část potom na konkrétní stránku a na konci máme offset ukazující na datový záznam
- **PDBR** je registr, ve kterém má proces uložený první odkaz na stránkování (stránkový adresář), **TLB** cachuje přístupy na virtuální adresu a vrací rovnou přeloženou fyzickou adresu 
- stránky můžou být odstraněny z fyzické adresy na swap místo (disk), systém si to pamatuje (present bit, nebo jinak), zachytí přístup na swapped stránku, načte ji do paměti a pokračuje v běhu
- stejně proces zachycuje přístup na neplatné virtuální adresy, nebo adresy jádra, pokud nemá uživatel přístup
- u liného načítání programu se stránka načte až ve chvíli, kdy ji potřebujeme, vhodné, pokud nechceme přistupovat ke všem datům
- v C jazyku si můžeme alokovat jednotlivé byty pomocí malloc, nebo potom celé stránky vmalloc
- speciální adresy jsou sběrnicové

### Správa zařízení
- zařízení jsou uloženy ve speciálních souborech, dané soubory mají hlavní a vedlejší číslo -> hlavní je číslo ovladače a vedlejší číslo je interní číslo pro ovladač k rozpoznání zařízení
- k zobrazení informací o speciálním souboru můžeme použít **stat**, k vytvoření takového souboru potom **mknod**
- zařízení jsou pojmenovány podle nějakých pravidel, může to být topologie, výrobce, pořadí zapojení, výrobní číslo -> každé z nich má výhody i nevýhody
- pro informace o zařízeních se používá hlavně SysFS, má adresáře podle různých typů pojmenování, poskytuje informace o zařízeních, zpřístupňuje soubory, pomocí kterých můžeme komunikovat s ovladačem
- většinou v adresáři /dev, pro zařízení v uživatelském prostoru potom /udev

  
