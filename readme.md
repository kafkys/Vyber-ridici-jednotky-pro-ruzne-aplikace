[Co dodělat ]: #
[pojmy ]: #

# Výběr řídící jednotky pro různé aplikace

$${\color{#FFA500}E9 \space \color{#4682B4}A1 }$$

## Cíle

- **Kategorizovat a porovnat** architektury řídicích systémů (MCU, MPU, embedded systémy, PLC, iPC, programovatelná relé) podle výkonu, paměti, determinismu a spolehlivosti.
- **Analyzovat provozní prostředí a vnější vlivy** (krytí IP, teplotní rozsah, EMC rušení, vibrace) a stanovit požadavky na mechanickou a elektrickou odolnost hardware.
- **Sestavit I/O bilanci** a navrhnout optimální řídicí jednotku z reálných katalogů výrobců pro konkrétní průmyslovou či IoT aplikaci včetně projektové rezervy.
- **Vypracovat vícekriteriální rozhodovací matici** a obhájit zvolenou platformu z technického a ekonomického hlediska (pořizovací cena, náročnost vývoje, údržba a spolehlivost).
- **Provést kritický technický audit (troubleshooting)** nevhodného návrhu řízení, identifikovat bezpečnostní a provozní rizika a navrhnout certifikované řešení v souladu s průmyslovými standardy.

## Ověření cílů

Výběr řídící jednotky pro různé aplikace

1. Příklady řídících jednotek
2. Jejich základní vlastnosti z hlediska výpočetního výkonu a velikosti paměťového prostoru
3. A z hlediska odolnosti
4. Příklady použití v praxi (kde se používají MCU, a kde ř. j. s MPU)


%% 1. Správné vysvětlení pojmů, architektur a zkratek z oblasti řídicích systémů. %%
%% 2. Schopnost posoudit vliv prostředí na výběr hardwaru a dešifrovat IP kód. %%
%% 3. Vypracování rozhodovací matice pro volbu vhodné platformy (MCU vs. PLC vs. iPC). %%
%% 4. Návrh konkrétní konfigurace řídicí jednotky na základě zadané I/O bilance a provozních podmínek. %%
%% 5. Kritická technická oponentura (audit) nevhodně navrženého řešení. %%

---

## Úlohy


### 1. Základní pojmy a architektury řídicích jednotek

*Časová dotace: 10–15 minut | Úvodní orientační úloha*

Doplňte do níže uvedené tabulky význam zkratek, základní princip a typický příklad reálného nasazení nebo zástupce:

| Zkratka / Pojem          | Co zkratka znamená (česky / anglicky) | Základní charakteristika (architektura, kde běží program)                                            | Typický zástupce (konkrétní rodina / model) | Příklad reálného nasazení                |
| :----------------------- | :------------------------------------ | :--------------------------------------------------------------------------------------------------- | :------------------------------------------ | :--------------------------------------- |
| **MCU**                  |                                       | Integrovaný čip (CPU + RAM + Flash na jednom substrátu), deterministický běh bez OS nebo RTOS        | např. ESP32, PIC16LF1xxx, RP2040            |                                          |
| **MPU**                  |                                       | Samostatný procesor vyžadující externí RAM a úložiště, zpravidla běží plnohodnotný OS (Linux)        |                                             |                                          |
| **Embedded**             |                                       |                                                                                                      | Embedded PLC, Embedded PC                   | Bílá technika, bankomaty, regulace kotlů |
| **PLC**                  |                                       | Průmyslový automat pro cyklické deterministické řízení procesů, vysoká odolnost, modulární/kompaktní |                                             |                                          |
| **iPC**                  |                                       |                                                                                                      |                                             |                                          |
| **Programovatelné relé** |                                       | Zjednodušené kompaktní PLC pro méně náročné úlohy, nahrazuje časovací relé a stykačové kombinace     |                                             |                                          |

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **SoC (System on Chip):** Integrovaný obvod sdružující všechny klíčové elektronické obvody a komponenty celého počítače či elektronického systému na jediném křemíkovém čipu. 
> 	 Systém na čipu. *Wikipedie: Otevřená encyklopedie* [online]. San Francisco (CA): Wikimedia Foundation, 2024, 2024-06-07 [cit. 2026-09-17]. Dostupné z: https://cs.wikipedia.org/wiki/Syst%C3%A9m_na_%C4%8Dipu
> - **DSP (Digital Signal Processor):** Specializovaný mikroprocesor architektury Harvard optimalizovaný pro matematické výpočty v reálném čase (rychlá Fourierova transformace FFT, filtrace šumu, digitální vektorové řízení střídavých motorů). 
> 	Digitální signálový procesor. In: _Wikipedia: otevřená encyklopedie_ [online]. St. Petersburg (Florida): Wikimedia Foundation, 2006, poslední editace 28. 2. 2026 [cit. 2026-09-14]. Dostupné z: [Digitální signálový procesor – Wikipedie](https://cs.wikipedia.org/wiki/Digit%C3%A1ln%C3%AD_sign%C3%A1lov%C3%BD_procesor)
> - **FPGA (Field-Programmable Gate Array):** Programovatelné logické hradlové pole, jehož vnitřní struktura logických bloků a propojení je konfigurovatelná až u zákazníka. Umožňuje masivní paralelní zpracování s hardwarovou latencí v řádu nanosekund. 
> 	Programovatelné hradlové pole. *Wikipedie: Otevřená encyklopedie* [online]. San Francisco (CA): Wikimedia Foundation, 2024, 2024-01-10 [cit. 2026-09-17]. Dostupné z: https://cs.wikipedia.org/wiki/Programovateln%C3%A9_hradlov%C3%A9_pole


<details>
<summary> :bulb: Tip k doplnění tabulky: </summary>
<p>Zaměřte se na čas náběhu a architekturu: U MCU je kód ve vnitřní paměti Flash procesoru a vykonává se okamžitě po přivedení napájení (řádově milisekundy). U systémů s MPU a iPC musí BIOS/bootloader nejprve zavést jádro operačního systému (OS Linux, Windows) z disku/eMMC/SD karty do operační paměti RAM, což trvá desítky sekund.</p>
</details>

:star2: **Bonusová otázka k úloze 1:**
Proč se u kritických aplikací v letectví (např. systém řízení letu Fly-by-Wire) nebo v jaderné energetice stále upřednostňují jednoduché deterministické mikrořadiče s několika desítkami kilobajtů paměti nebo obvody FPGA před moderními vícejádrovými gigahertzovými procesory s gigabajty RAM?

*Vaše odpověď:*
`...`

---

### 2. Parametry, paměti a provozní odolnost (IP krytí)

*Časová dotace: max. 15 minut | Mírně náročnější úloha propojující parametry a praxi*

1. **Typy pamětí v řídicích jednotkách:**
   - Doplňte porovnání pamětí z hlediska stálosti dat a rychlosti:
     - **RAM:** 
	     - Je volatilní (energeticky závislá)? `[Ano / Ne]`
	     - Rychlost zápisu: `...` 
	     - K čemu se využívá v PLC/MCU: `...`
     - **Flash (ROM):** 
	     - Je volatilní? `[Ano / Ne]`
	     - K čemu se využívá v PLC/MCU: `...`
     - **EEPROM / NVRAM:** 
	     - Je volatilní? `[Ano / Ne]`
	     - K čemu se využívá v PLC/MCU: `...`
   - *Otázka z praxe:* Kam se v průmyslovém PLC ukládají aktuální provozní proměnné (např. čítače vyrobených kusů nebo motohodiny), aby se při nečekaném výpadku napájení neztratily (tzv. remanentní / retain data)?
     - Odpověď: `...`

2. **Reálný čas a determinismus (Hard vs. Soft Real-Time):**
   - Proč pro reakci na nouzové zastavení lisu (požadavek reakce do 5 ms) použijeme PLC či mikrokontrolér s RTOS, a nikoliv běžné Raspberry Pi s operačním systémem Raspberry Pi OS (standardní Linux)?
     - Odpověď: `...`

3. **Odolnost vůči vlivům prostředí a dešifrování kódu IP:**
   - Dešifrujte kód **IP68**:
     - První číslice (6): `...`
     - Druhá číslice (8): `...`
   - Jaké minimální krytí IP musí mít rozváděč umístěný ve venkovním nekrytém prostředí, kde na něj přímo dopadá déšť a fouká polétavý prach?
     - Označte správnou volbu: `[ ] IP20` | `[ ] IP44` | `[ ] IP65` | `[ ] IP00`
     - Zdůvodnění: `...`

4. **Konstrukční rozdíly kancelářského PC vs. průmyslového iPC:**
   - Vyberte a doplňte hlavní odlišnosti:
     - *Chlazení:* 
	     - Kancelářské PC: `...` 
	     - vs. iPC: `...`
     - *Napájecí napětí a filtrace:* 
	     - Kancelářské PC: `...` 
	     - vs. iPC: `...`
     - *Odolnost proti otřesům a vibracím:* `...`
     - *Způsob montáže:* 
	     - Kancelářské PC: na stůl/pod stůl 
	     - vs. iPC: `...`

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **Determinismus (Real-Time):** Vlastnost systému, která zaručuje, že odezva na vstupní událost proběhne vždy v přesně definovaném a předvídatelném čase (deadline). V *Hard Real-Time* systémech znamená nedodržení časového limitu fatální havárii celého procesu. 
> 	Operační systém reálného času. *Wikipedie: Otevřená encyklopedie* [online]. San Francisco (CA): Wikimedia Foundation, 2024, 2024-05-12 [cit. 2026-09-17]. Dostupné z: https://cs.wikipedia.org/wiki/Opera%C4%8Dn%C3%AD_syst%C3%A9m_re%C3%A1ln%C3%A9ho_%C4%8Dasu
> - **Krytí IP (Ingress Protection):** Mezinárodní standard dle normy **ČSN EN 60529** určující stupeň ochrany krytem před vniknutím pevných cizích těles včetně prachu (1. číslice 0–6) a vniknutím vody (2. číslice 0–9K).
> 	ČESKÝ NORMALIZAČNÍ INSTITUT. *ČSN EN 60529 (33 0330) Stupně ochrany krytem (krytí - IP kód)*. Praha: Český normalizační institut, 1993. Třídící znak 330330.
> - **Remanentní paměť (Retain):** Paměťový prostor v PLC, jehož obsah zůstává zachován i po přerušení napájecího napětí (využívá zálohovací baterii, superkondenzátor nebo zápis do FRAM/MRAM/EEPROM).

<details>
<summary> :bulb: Tip k otázce determinismu: </summary>
<p>Běžný Linux je <b>preemptivní víceúlohový systém</b>, který se snaží spravedlivě rozdělit čas procesoru mezi stovky procesů. Může se stát, že kvůli obsluze disku, správě paměti nebo síťovému provozu se proces řízení pozdrží na desítky milisekund. PLC naproti tomu vykonává cyklus v pevném taktu bez zpoždění vyvolaného aplikacemi na pozadí.</p>
</details>

:star2: **Bonusová otázka k úloze 2:**
Co označuje doplňkové písmeno **K** v kódu krytí **IP69K** a v jakém průmyslovém odvětví je toto krytí bezpodmínečně vyžadováno?

*Vaše odpověď:*
`...`

---

### 3. Rozhodovací matice platforem (MCU vs. PLC vs. iPC) 

*Časová dotace: 20–25 minut | :star: Klasifikovaná inženýrská úloha na známky*

Jste v pozici nezávislého konzultanta automatizace. Tři různí zákazníci požadují navrhnout optimální kategorii řízení.

#### Příklad aplikace (vzorové řešení):
- **Vzorová aplikace 0 – Automatická vjezdová závora na parkoviště:** Jednoduchý jednoúčelový systém s indukční detekční smyčkou vozidla, bezpečnostní optozávorou, koncovými spínači polohy ramene, motorem závory (vpřed/vzad) a výstražným semaforem (červená/zelená). Požadavek na jednoduchou správu správcem objektu a spolehlivý chod v rozváděči u vjezdu.

#### Popis zadaných aplikací pro studenty:
1. **Aplikace A – Chytrý pokojový termostat (IoT):** Bateriově napájený přístroj měřící teplotu a vlhkost v místnosti, zobrazující údaje na e-ink displeji a odesílající data přes protokol ZigBee/Wi-Fi do domácí brány. Plánovaná sériová výroba: 10 000 kusů ročně.
2. **Aplikace B – Automatická balicí linka:** Průmyslová linka ve výrobní hale. Obsahuje 28 optických snímačů, 14 pneumatických válců, 3 dopravníkové pásy s asynchronními motory a bezpečnostní světelnou závoru. Vyžaduje se nepřetržitý provoz 24/7 a snadná údržba podnikovým elektrikářem.
3. **Aplikace C – Kontrolní stanice optické jakosti svarů:** Pracoviště se 2 vysokorychlostními průmyslovými GigE kamerami snímajícími svary na karoserii automobilu. Snímky v rozlišení 4K jsou analyzovány neuronovou sítí v reálném čase, vady jsou označeny a ukládány do podnikové relační databáze (SQL / MES).

#### Váš úkol:
Vyplňte rozhodovací matici. Jako vzor poslouží vyplněný sloupec pro **Vzorovou aplikaci 0**. Přiřaďte každé aplikaci nejvhodnější platformu (**MCU / Embedded SoC**, **Kompaktní/modulární PLC**, **Průmyslové PC – iPC**) a doplňte multikriteriální posouzení:


| Kritérium hodnocení | Vzorová aplikace 0 (Vjezdová závora - VZOR) | Aplikace A (Pokojový termostat) | Aplikace B (Balicí linka) | Aplikace C (Kamerová kontrola svarů) |
| :--- | :--- | :--- | :--- | :--- |
| **Doporučená platforma (MCU / PLC / iPC)** | Programovatelné relé / kompaktní PLC (např. Siemens LOGO!, Eaton easyE4) | MCU / Embedded SoC | Kompaktní / modulární PLC | Průmyslové PC (iPC) |
| **Pořizovací cena HW na 1 kus** | Střední (cca 3 500 – 6 000 Kč) | Nízká (< 500 Kč) | Střední (cca 5 – 30 tis. Kč) | Vysoká (> 50 tis. Kč) |
| **Primární programovací jazyk** | FBD / LAD (grafické funkční bloky nebo liniové schéma dle IEC 61131-3) | C / C++ / MicroPython | IEC 61131-3 ST / LAD (strukturovaný text / liniové schéma) | Python / C# / C++ pod OS |
| **Klíčový technický argument pro volbu** | Montáž přímo na DIN lištu v rozváděči, integrovaný displej pro nastavení časovačů přímo na místě, robustní reléové výstupy pro motor a semafor, napájení 24 V DC / 230 V AC bez nutnosti vývoje vlastního plošného spoje. | Minimální spotřeba energie (možnost bateriového napájení), extrémně nízké výrobní náklady při masové produkci a malé rozměry pro integraci na vlastní PCB. | Vysoký determinismus (reálný čas) pro synchronizaci motorů, vysoká odolnost proti průmyslovému rušení, snadná modularita a rozšiřitelnost o I/O karty. | Extrémní výpočetní a grafický výkon pro zpracování obrazu ve vysokém rozlišení v reálném čase, podpora AI/OpenCV knihoven a velké úložiště pro data. |
| **Hlavní riziko při volbě špatné platformy** | **MCU:** Nutnost vývoje vlastní desky, nízká odolnost vůči venkovnímu rušení a obtížný servis údržbou.<br><br>**iPC:** Zbytečně extrémní cena (> 30 tis. Kč), dlouhý start po výpadku napájení a vysoká spotřeba. | **PLC:** Vysoká cena, obrovské rozměry a nemožnost integrace do estetického interiérového těla termostatu.<br><br>**iPC:** Extrémní cena, vysoká spotřeba (nelze napájet z baterií) a nutnost chlazení. | **MCU:** Riziko selhání v zarušeném prostředí (EMC), chybějící průmyslové krytí a složitý servis bez diagnostiky.<br><br>**iPC:** Náchylnost ke stabilitě OS (pády systému) a zbytečně vysoká cena pro standardní řízení pohonů. | **MCU:** Naprostý nedostatek paměti RAM a výpočetního výkonu pro analýzu obrazových matic.<br><br>**PLC:** Standardní PLC neumí zpracovat video stream z kamer a chybí mu grafický výkon pro detekci vad. |

> **Kritéria hodnocení úlohy 3 (bodování a známka):**
> - :star: **Správnost technického přiřazení platforem (30 %):** Stoprocentně logické a obhajitelné přiřazení všech 3 technologií.
> - :star: **Inženýrská a ekonomická argumentace (40 %):** Zohlednění ekonomiky sériovosti (kusová vs. masová výroba), spotřeby energie, náročnosti vývoje a schopností servisního personálu.
> - :star: **Analýza rizik nevhodné platformy (30 %):** Věcné zdůvodnění, proč je v daném případě jiná platforma neefektivní, příliš drahá nebo neschopná úlohu odbavit.

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **Norma ČSN EN 61131-3:** Mezinárodní standard pro programovací jazyky PLC automatů. Definuje dva textové jazyky (ST – strukturovaný text, IL – seznam instrukcí) a tři grafické jazyky (LD – příčkový diagram / kontaktní schéma, FBD – funkční blokové schéma, SFC – sekvenční funkční schéma).
> 	ČESKÝ NORMALIZAČNÍ INSTITUT. *ČSN EN 61131-3 ed. 3 (18 0080) Programovatelné řídicí jednotky - Část 3: Programovací jazyky*. Praha: Úřad pro technickou normalizaci, metrologii a státní zkušebnictví, 2014. Třídící znak 180080.
> - **GigE Vision:** Komunikační standard rozhraní pro průmyslové kamery využívající gigabitový Ethernet, umožňující přenos nekomprimovaného videa vysokou rychlostí na velké vzdálenosti.

<details>
<summary> :bulb: Tip pro Aplikaci A vs. B vs. C: </summary>
<p>U aplikace A rozhoduje kusová cena a odběr proudu z baterie (PLC ani iPC z baterie nerozběhnete). U aplikace B potřebujete vyměnitelný modul na DIN lištu s diagnostickými LED, který přeprogramuje běžný údržbář v jazyce LAD. U aplikace C potřebujete obrovský výpočetní výkon pro AI a ovladače pro průmyslové kamery, což MCU ani běžné PLC nezvládne.</p>
</details>

:star2: **Bonusová otázka k úloze 3:**
Co je to tzv. **SoftPLC** a jak umožňuje průmyslovému PC (iPC) kombinovat výhody operačního systému Windows/Linux a deterministického řízení reálného času v jediném fyzickém počítači?

*Vaše odpověď:*
`...`

---

### 4. Návrh a konfigurace řídicí jednotky pro čerpací stanici

*Časová dotace: 25–30 minut | :star: Klasifikovaná inženýrská úloha na známky*

Jste v roli projektanta automatizace. Zákazník poptává zhotovení řízení pro obecní přečerpávací stanici odpadních vod.

#### Zadání technologického procesu a periferií:
- **Snímače a vstupy:**
  - 3× plovákový hladinový spínač (havarijní spodní hladina proti chodu nasucho, zapínací hladina, havarijní přepad) – bezpotenciálový kontakt spínající 24 V DC.
  - 1× hydrostatická ponorná sonda výšky hladiny v jímce – výstupní signál 4–20 mA.
  - 1× termistorové ochranné relé přehřátí motoru čerpadla – poruchový kontakt 24 V DC.
- **Akční členy a výstupy:**
  - 2× stykač pro spouštění motorů hlavního a záložního čerpadla – spínání cívky stykače 230 V AC / 0,5 A.
  - 1× opticko-akustický výstražný maják – napájení 24 V DC / 0,3 A.
  - 1× řízení otáček frekvenčního měniče hlavního čerpadla – analogový signál 0–10 V.
- **Komunikace a přenos dat:**
  - Odesílání údajů o hladině a poruchách na dispečink vodáren (Ethernet / Modbus TCP nebo GSM/LTE modul).
- **Provozní podmínky:**
  - Venkovní nekrytý terén, rozváděč vystavený dešti, prachu a teplotám v rozmezí **-20 °C až +45 °C**.

#### Váš úkol:

1. **Sestavte tabulku I/O bilance** a spočtěte celkový počet signálů. Připočtěte rezervu min. 20 % pro budoucí rozšíření:

# Technický návrh: Řízení obecní přečerpávací stanice odpadních vod

---

## 1. Tabulka I/O bilance s rezervou

*Při výpočtu je základní požadavek aplikace zaokrouhlen směrem nahoru na celé jednotky (signály) po připočtení **20% rezervy**.*

# Technický návrh: Řízení obecní přečerpávací stanice odpadních vod

---

## 1. Tabulka I/O bilance s rezervou

*Při výpočtu je základní požadavek aplikace zaokrouhlen směrem nahoru na celé jednotky (signály) po připočtení **20% rezervy**.*

| Typ signálu | Požadavek aplikace (kusy) | Popis signálů v aplikaci | Počet po započtení rezervy (+20 %) |
| :--- | :---: | :--- | :---: |
| **Digitální vstup (DI)** | 4 | 3× plovákový spínač (suchoběh, zapínací hladina, přepad), 1× poruchový kontakt ochrany přehřátí motoru. | **5** *(4 × 1,2 = 4,8)* |
| **Digitální výstup (DO) – reléový** | 2 | 2× cívka stykače pro spouštění motorů hlavního a záložního čerpadla (230 V AC / 0,5 A). | **3** *(2 × 1,2 = 2,4)* |
| **Digitální výstup (DO) – tranzistorový** | 1 | 1× opticko-akustický výstražný maják (24 V DC / 0,3 A). | **2** *(1 × 1,2 = 1,2)* |
| **Analogový vstup (AI)** | 1 | 1× hydrostatická ponorná sonda výšky hladiny v jímce (4–20 mA). | **2** *(1 × 1,2 = 1,2)* |
| **Analogový výstup (AO)** | 1 | 1× řízení otáček frekvenčního měniče hlavního čerpadla (0–10 V). | **2** *(1 × 1,2 = 1,2)* |
| **CELKEM** | **9** | | **14** |

---

## 2. Výběr konkrétního hardwaru z katalogu výrobce

Pro zajištění vysoké spolehlivosti a splnění průmyslových standardů byla zvolena modulární platforma **Siemens SIMATIC S7-1200**.

* **Výrobce a přesný model CPU:** Siemens SIMATIC S7-1200, CPU 1214C DC/DC/DC
* **Objednací kód (Part Number):** [6ES7214-1AE40-0XB0](https://siemens.com)
* **Rozšiřující moduly (pro splnění I/O a rezervy):**
  * **1× Signálová deska analogového výstupu:** SB 1232, 1 AO (0–10 V / 4–20 mA) – instaluje se přímo do čelního slotu CPU, šetří místo na DIN liště. Objednací kód: [6ES7232-4HA30-0XB0](https://siemens.com).
  * *Poznámka k integraci:* Integrované CPU 1214C obsahuje 14 DI, 10 DO (tranzistorových) and 2 AI (0–10 V). Ponorná sonda (4–20 mA) se připojí na vestavěný AI přes přesný bočníkový odpor 500 Ω (převod na 2–10 V v programu), což eliminuje nutnost drahého AI rozšiřujícího modulu. Pro reléové výstupy využijeme tranzistorové DO k buzení externích vazebních relé (viz bod 3).
* **Napájecí napětí zvolené jednotky:** 24 V DC (přípustný rozsah 20,4 až 28,8 V DC)
* **Jak je vyřešeno odesílání dat na dispečink:** CPU disponuje integrovaným portem RJ45 s podporou **Profinet / Modbus TCP**. Do rozváděče bude osazen průmyslový LTE router (např. *Teltonika RUT241*), který bude s PLC komunikovat přes Modbus TCP a data bezpečně šifrovaným VPN tunelem (IPsec/OpenVPN) přenášet na dispečink vodáren, případně odesílat SMS alarmy.
* **Odkaz na technický list (datasheet):** [Siemens S7-1200 CPU 1214C Datasheet](https://siemens.com)
* **Odkazy na další použité zdroje:** [Siemens Industry Mall](https://siemens.com) / [Teltonika Networks](https://teltonika-networks.com)

---

## 3. Technické ověření z datasheetu

* **Zvládá zvolená jednotka garantovaný provoz při -20 °C? Doložte údaj z datasheetu:**
  > **Ano.** Podle oficiálního technického listu výrobce Siemens je okolní provozní teplota pro rodinu S7-1200 (pro model 6ES7214-1AE40-0XB0) při horizontální montáži garantována v rozsahu **-20 °C až +60 °C**.
* **Jakým způsobem spínáte cívku stykače 230 V AC (reléový výstup jednotky přímo, nebo přes pomocné mezilehlé relé)? Zdůvodněte:**
  > Cívky spínáme **přes pomocná mezilehlé (vazební) relé** (např. *Finder řady 38* s paticí na DIN lištu, šířka 6.2 mm). 
  > 
  > **Zdůvodnění:** Cívka výkonového stykače (0,5 A) vykazuje při rozepnutí vysokou indukční špičku. Přímé spínání interními relé v PLC by rapidně snížilo životnost kontaktů a v případě jejich spečení by byla nutná výměna celého drahého procesoru. Použití úzkých vazebních relé galvanicky odděluje citlivou elektroniku PLC od silové části 230 V AC, dramaticky usnadňuje servis (výměna relé v patici trvá 10 sekund a stojí cca 200 Kč) a umožňuje plně využít spolehlivější tranzistorové DC výstupy přímo na základní desce PLC.

---

## 4. Krytí a teplotní management rozváděče

* **Zvolené krytí rozváděče:** 
  **IP66** v provedení z nerezové oceli nebo UV stabilního sklolaminátu (např. průmyslové skříně *Rittal řady AX*). Vzhledem k umístění na nekrytém venkovním terénu musí skříň stoprocentně odolat intenzivně stříkající vodě (přívalový déšť, bouřky) a jemnému prachu ze všech směrů. Rozváděč bude navíc vybaven vrchní ochrannou stříškou proti dešti a přímému slunci.
* **Teplotní management skříně:**
  * **Provoz v mrazech (-20 °C):** Do spodní části rozváděče bude instalováno **odporové topné těleso s integrovaným termostatem** (např. *STEGO* o výkonu 50W–100W) nastaveným na spínání při poklesu pod +5 °C. To zajistí, že teplota uvnitř neklesne k mezním hodnotám PLC a zároveň eliminuje kondenzaci vzdušné vlhkosti, která by mohla způsobit zkrat.
  * **Provoz v letních vedrech (+45 °C):** Jelikož je skříň na přímém slunci, vnitřní teplota by bez chlazení snadno překročila kritických +60 °C. Rozváděč bude mít **dvojitou stěnu (pasivní stínění)**, reflexní světle šedý lak (RAL 7035) a bude osazen **ventilačními mřížkami s nuceným oběhem (ventilátor + filtr)** ovládanými termostatem nastaveným na +35 °C. Výdechové mřížky budou osazeny venkovními kryty proti dešti (tzv. *Schrankshub* se zachováním krytí IP55/IP56).


> **Kritéria hodnocení úlohy 4 (bodování a známka):**
> - :star: **Správnost I/O bilance a dimenzování (30 %):** Správný součet všech signálů, korektní rozlišení reléových vs. tranzistorových výstupů a správné započtení rezervy min. 20 %.
> - :star: **Reálnost výběru a kompatibilita HW (40 %):** Zvolený přístroj skutečně existuje na trhu, konfigurace plně pokrývá všechny vstupy/výstupy (včetně analogů 4–20 mA a 0–10 V) a komunikaci.
> - :star: **Posouzení provozních podmínek a instalace (30 %):** Správná volba krytí rozváděče (min. IP65), vyřešení vytápění/ventilace pro mráz a spolehlivé galvanické oddělení výkonových akčních členů.

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **Proudová smyčka 4–20 mA:** Průmyslový standard pro přenos analogových signálů ze senzorů. Výhodou oproti napěťovému signálu 0–10 V je vysoká odolnost proti elektromagnetickému rušení, nezávislost na odporu dlouhého vedení a detekce přetržení vodiče (pokud je proud roven 0 mA, jde o poruchu vedení – tzv. živá nula / live zero).
> 	Proudová smyčka. *Wikipedie: Otevřená encyklopedie* [online]. San Francisco (CA): Wikimedia Foundation, 2023, 2023-04-18 [cit. 2026-09-17]. Dostupné z: https://cs.wikipedia.org/wiki/Proudov%C3%A1_smy%C4%8Dka
> - **Galvanické oddělení:** Elektrické oddělení dvou elektrických obvodů (např. pomocí optočlenů nebo relé), které zabraňuje přenosu rušení, rozdílům zemních potenciálů a chrání citlivé vstupy řídicí jednotky před zničením přepětím.
> - **Bezpotenciálový kontakt** (označovaný také jako **dry contact**) je elektrický kontakt, který sám o sobě nemá žádné vlastní napětí ani neposkytuje žádný proud. Funguje čistě jako mechanický nebo elektronický spínač (jako klasický vypínač na zdi), který pouze spojí nebo rozpojí dva vodiče v externím obvodu.

<details>
<summary> :bulb: Tip pro výběr modulů: </summary>
<p>Pozor na analogové vstupy: Základní kompaktní jednotky (např. LOGO! nebo S7-1200) mívají integrované analogové vstupy pouze pro napětí 0–10 V. Vstupní signál 4–20 mA ze sondy vyžaduje buď speciální rozšiřující modul pro proudové signály, nebo zařazení přesného paralelního odporu 500 Ω (převod 4–20 mA na 2–10 V).</p>
</details>

:star2: **Bonusová otázka k úloze 4:**
Proč se u čerpadel v čistírnách odpadních vod a jímkách striktně upřednostňuje měření hladiny pomocí proudového signálu 4–20 mA před napěťovým signálem 0–10 V a proč se do jímky nepoužívá ultrazvukový senzor, pokud v ní vzniká hustá pěna?

*Vaše odpověď:*
`...`

---

### 5. Technický audit a oponentura nevhodného návrhu

*Časová dotace: 20–25 minut | :star: Klasifikovaná inženýrská úloha na známky*

Jako vedoucí inženýr jste převzal projekt po nezkušeném brigádníkovi, který navrhl řízení automatizovaného tvářecího a lisovacího stroje v prašné kovářské dílně následovně:
- **Řídicí deska:** Běžná vývojová deska **Arduino Uno (Rev3)** s mikrokontrolérem ATmega328P.
- **Pouzdro a umístění:** Plastová krabička vytištěná na 3D tiskárně z materiálu **PLA**, přišroubovaná přímo na těleso vibrujícího hydraulického lisu.
- **Napájení:** 5V USB nabíječka na mobilní telefon zapojená do prodlužovacího kabelu 230 V.
- **Spínání zátěže:** 4kanálový hobby reléový modul z čínského e-shopu propojený s Arduinem tenkými nepájenými vodiči (DuPont propojky). Modul přímo spíná 400V ventily hydrauliky.
- **Bezpečnost (Safety):** Nouzové stop tlačítko (E-Stop) je zapojeno přímo do digitálního pinu D2 Arduina jako softwarové přerušení (interrupt), které v kódu nastaví výstupy na `LOW`.

#### Váš úkol:

1. **Zpracujte písemný audit rizik (minimálně 4 fatální technická selhání):**
   Vyplňte protokol o zjištěných vadách a popište konkrétní fyzikální mechanismus, jak daná chyba způsobí havárii stroje či ohrožení lidského života:

# 1. Písemný audit rizik (Protokol o zjištěných vadách)

| Oblast auditu | Zjištěná vada v amatérském návrhu | Fyzikální mechanismus selhání (proč to selže) | Následek pro stroj nebo obsluhu |
| :--- | :--- | :--- | :--- |
| **Elektromagnetická kompatibilita (EMC)** | Absence galv. oddělení, odrušovacích diod/RC členů u indukční zátěže, chybějící stínění. | Napěťové špičky z indukční zátěže hydraulických ventilů způsobí restart MCU nebo poškození tranzistorů. | Neočekávané chování lisu, ztráta kontroly nad polohou pístu, riziko nekontrolovaného sepnutí lisu a těžkého úrazu obsluhy. |
| **Mechanická a teplotní odolnost** | PLA plast a montáž na těleso lisu | PLA plast má nízkou teplotu měknutí (cca 60 °C). Vibrace a teplo z tělesa lisu způsobí strukturální degradaci, deformaci krytu a uvolnění komponentů. | Zkrat na kostru stroje při uvolnění desky elektroniky. Fatální selhání řízení za běhu stroje, riziko požáru nebo zásahu proudem. |
| **Konektivita a propojení vodičů** | DuPont propojovací kabely bez aretace | Kontaktní odpor se vlivem vibrací lisu mění. Dochází k mikrovýpadkům spojení nebo k úplnému vytřesení kabelu z pinů. | Ztráta signálu ze senzorů (např. koncové spínače). Stroji chybí zpětná vazba, pokračuje v pohybu za mechanické limity a zničí se. |
| **Funkční bezpečnost (Safety)** | Nouzový stop řešený softwarově v čipu | Zaseknutí programu (freeze MCU), chyba v kódu nebo poškození čipu špičkou způsobí ignorování stisku tlačítka E-Stop. | Nemožnost zastavit stroj v případě nouze. Fatální či smrtelné zranění obsluhy (přimáčknutí, amputace končetin). |

---

# 2. Návrh profesionálního nápravného řešení

* **Náhrada řídicí jednotky:** **Siemens LOGO! 24RCE** (případně Eaton easyE4). Jedná se o certifikované průmyslové programovatelné relé s robustním krytím, montáží na DIN lištu, vysokou odolností proti vibracím, teplotám a elektromagnetickému rušení.
* **Náhrada napájecího zdroje:** **Siemens SITOP PSU100C 24 V / 1,3 A** (případně Mean Well na DIN lištu). Stabilizovaný průmyslový spínaný zdroj určený na DIN lištu s integrovanou ochranou proti přetížení, zkratu a přepětí, zajišťující čisté napájení pro logické obvody.
* **Způsob zapojení bezpečnostního okruhu (Safety):** 
  Podle platných norem (**ČSN EN ISO 13849-1**) musí být tlačítko Emergency Stop (E-Stop) zapojeno výhradně **hardwarově**, a to prostřednictvím certifikovaného **bezpečnostního relé** (např. *Sick, Pilz, Schneider Preventa*), které při aktivaci fyzicky a bezpečně odpojí napájení akčních členů (stykačů motorů a ventilů). 
  
  **Zdůvodnění:** Nesmí se v žádném případě spoléhat pouze na software mikrokontroléru. Software není deterministicky bezpečný prvek – může selhat z důvodu zacyklení, chyb v registru, poškození paměti nebo hardwarového poškození samotného jádra MCU. Bezpečnostní funkce musí fungovat nezávisle na řídicím systému.

> **Kritéria hodnocení úlohy 5 (bodování a známka):**
> - :star: **Odborná úroveň identifikace závad (35 %):** Přesná technická terminologie (např. elektromagnetická indukce, absence odrušovacích varistorů, skelný přechod PLA plastu při 60 °C, studené spoje a vyklepání konektorů vibracemi).
> - :star: **Pochopení norem funkční bezpečnosti Safety (35 %):** Znalost základního principu bezpečnosti strojních zařízení – nouzové zastavení musí být řešeno hardwarově přes certifikované bezpečnostní relé s nuceně vedenými kontakty, nikoliv pouhým softwarovým vstupem MCU.
> - :star: **Kvalita a realizovatelnost nápravného řešení (30 %):** Návrh odpovídá robustní průmyslové praxi s montáží do oceloplechového rozváděče na DIN lištu.

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **Funkční bezpečnost (Safety) vs. Kybernetická bezpečnost (Security):** *Safety* (dle ČSN EN ISO 13849-1) zajišťuje, že strojní zařízení nezpůsobí úraz člověku ani při vnitřní poruše řídicího systému (využívá redundantní obvody, bezpečnostní relé, optické závory, kategorii spolehlivosti PL a až PL e / SIL 3). *Security* řeší ochranu dat a systému před úmyslným napadením zvenčí (hackeři, malware).
> - **EMC (Elektromagnetická kompatibilita):** Schopnost zařízení spolehlivě pracovat v prostředí s elektromagnetickým rušením (odolnost / imunita) a současně nezpůsobovat nepřípustné rušení jiným zařízením (emise).
> 	Elektromagnetická kompatibilita. *Wikipedie: Otevřená encyklopedie* [online]. San Francisco (CA): Wikimedia Foundation, 2023, 2023-11-20 [cit. 2026-09-17]. Dostupné z: https://cs.wikipedia.org/wiki/Elektromagnetick%C3%A1_kompatibilita

<details>
<summary> :bulb: Tip k bezpečnostnímu okruhu (Safety): </summary>
<p>Základní pravidlo bezpečnosti: <strong>Software může selhat, zacyklit se nebo zamrznout.</strong> Bezpečnostní okruh nouzového zastavení (červený hřib) musí být vždy dvoukanálový, zapojený do hardwarového bezpečnostního relé (např. Pilz, Schneider Preventa, Siemens SIRIUS), které odpojí silové napájení stykačů ventilů přímo na hardwarové úrovni nezávisle na procesoru!</p>
</details>

:star2: **Bonusová otázka k úloze 5:**
Proč hobby reléové moduly s optočleny určené pro Arduino v průmyslovém rozváděči často shoří nebo způsobí trvalé sepnutí zátěže (tzv. přivaření kontaktů), i když jmenovitý proud relé je 10 A a cívka stykače odebírá jen 0,5 A?

*Vaše odpověď:*
`...`

---

### 6. Rozšiřující inženýrská výzva: TCO a životní cyklus v automatizaci

*Časová dotace: 15–20 minut | :star2: Bonusová výzva pro pokročilé studenty*

V průmyslové automatizaci nákupní cena řídicí jednotky (CAPEX) často tvoří méně než 15 % celkových nákladů na životní cyklus zařízení (OPEX / TCO).

Představte si, že management firmy rozhoduje mezi dvěma variantami řízení pro sérii 50 kusů výrobních linek s plánovanou životností 15 let:
- **Varianta 1 (Nízkonákladová na pořízení):** Využití levných embedded mikrokontrolérových desek s vlastním zákaznickým návrhem plošného spoje (cena HW: 2 500 Kč / kus, vývoj firmwaru v C/C++ od externího programátora bez dokumentace).
- **Varianta 2 (Průmyslový standard):** Využití modulárního PLC renomovaného výrobce (Siemens / Rockwell / Schneider) s cenou 22 000 Kč / kus, programováno v normovaném jazyce LAD/ST dle IEC 61131-3.

#### Váš úkol:
1. Srovnejte obě varianty v níže uvedené tabulce a uveďte předpokládaná skrytá rizika a náklady v horizontu 10–15 let:

# 6. Rozšiřující inženýrská výzva: TCO a životní cyklus v automatizaci

---

## 1. Srovnání variant v horizontu 10–15 let

| Aspekt životního cyklu | Varianta 1 (Custom Embedded MCU) | Varianta 2 (Průmyslové PLC) |
| :--- | :--- | :--- |
| **Dostupnost náhradních dílů za 10 let** | **Extrémně kritická.** Elektronické komponenty podléhají rychlému morálnímu zastarání (End-of-Life). Při výpadku jednoho čipu je nutný kompletní redesign celého plošného spoje (PCB) a nová certifikace. | **Vysoká garance.** Renomovaní výrobci (Siemens, Rockwell) garantují dostupnost identických náhradních dílů po dobu 10 let od ukončení výroby a následnou zpětnou kompatibilitu nástupců. |
| **Servisovatelnost podnikovým elektrikářem** | **Nemožná.** Běžný údržbář nemá vybavení ani znalosti pro diagnostiku embedded desek na úrovni mikročipů. Bez chybějící dokumentace a zdrojového kódu je systém pro údržbu „černou skříňkou“. | **Standardní.** Běžný provozní elektrikář je vyškolen na práci s PLC. Dokáže vyměnit vadný modul na DIN liště, připojit se k jednotce, přečíst chybovou diagnostiku a nahrát zálohu programu. |
| **Doba odstávky linky při poruše CPU** | **Dny až týdny.** Pokud nejsou skladem specifické osazené desky, linka stojí. Oprava vyžaduje zásah externího specialisty, zdlouhavé hledání chyb v hardwaru nebo kompletní přepis firmwaru. Výpadky generují obrovské ztráty. | **Minuty až hodiny.** Údržba vyjme vadné PLC z DIN lišty, nacvakne nový kus ze skladu, nahraje ze serveru zálohovaný program (nebo přehraje SD kartu) a linka okamžitě pokračuje v produkci. |
| **Cena vývojových nástrojů a licencí IDE** | **Nízká / Zdarma.** Vývojová prostředí pro MCU (např. STM32CubeIDE, VS Code, Arduino IDE) jsou většinou open-source bez licenčních poplatků. | **Vysoká.** Profesionální inženýrské softwary (např. TIA Portal, Studio 5000) vyžadují nákup drahých vývojových licencí a pravidelné poplatky za aktualizace (Software Update Service). |
| **Závěrečné doporučení (kterou variantu vybrat a proč)** | **Nedoporučuje se pro sériovou výrobu.** Nízké pořizovací náklady (CAPEX) jsou vykoupeny extrémním rizikem obrovských provozních nákladů (OPEX) při sebemenší poruše a závislostí na jednom externím vývojáři. | **Jednoznačná volba pro průmysl.** Vyšší počáteční investice se mnohonásobně vrátí v minimální době odstávek, snadné údržbě, dlouhodobé stabilitě a plné zastupitelnosti servisních techniků. |
