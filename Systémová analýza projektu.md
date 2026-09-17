
# Automat garáž ovládanie Peter Macúš
- **Názov projektu**: Automat garáž ovládanie
- **Meno riešiteľa**: Peter Macúš

---

## Dôvod a okolnosti zavedenia riešenia
Stránka je navrhnutá na zjednodušenie ovládania automatických garáží poháňaných ESP32. Na stránke sa používateľ dokáže jednoducho orientovať vďaka zrozumiteľnému UI s jednoduchým ovládaním.  

---

## Slovné zadanie, popis projektu od zákazníka
Cieľom tohto projektu je vytvoriť prehľadnú a intuitívnu stránku pre ovládanie autonómnej garáže určenej na zjednodušenie parkovania. Stránka bude jednoduchá na pochopenie pre všetkých užívateľov. Bude komunikovať s ESP32, ktoré ovláda chod celej garáže a zároveň bude zobrazovať aktuálny stav a obsadenosť garáže. Pred príjazdom do garáže zároveň bude zároveň ukázaný odhadovaný čas príchodu.

---

## Seznam modulů projektu a jejich významných atributů
1. **Modul zobrazenia stavu a obsadenosti**
   - Atribúty: živý prehľad voľných a obsadených pozícii, vizualizácia stavu garáže (voľno/plno)(funkčná/v údržbe) 
   - Unikátna identifikácia objektov: ID parkovacieho slotu, ID garáže

2. **Užívateľské rozhranie (UI)**
   - Atribúty: Interaktívne tlačidlá na otvorenie/privolanie parkovacej pozície, responzívny dizajn
   - Unikátna identifikácia objektov: ID užívateľa

3. **Dátový analytický modul**
   - Atribúty: obojstranná synchronizácia dát s firebase v reálnom čase, odosielanie požiadaviek zo stránky, prijímanie zmien zo senzorov
   - Unikátna identifikácia objektov: API kľúč

---

## Systémové požiadavky FURPS
1. **Funkčnosť (Functionality - F)**
   - Zobrazenie reálneho stavu obsadenosti
   - Zobrazenie stavu garáže
   - Odosielanie príkazov pre privolanie parkovacieho miesta

2. **Vhodnosť k použitiu (Usability - U)**
   - Užívateľsky prívetivý rozhranie
   - Intuitívne ovládanie pre profesionálov aj laikov
   - Prístupné pre každého s prístupom k internetu a internetovému prehliadaču

3. **Spoľahlivosť (Reliability - R)**
   - Spoľahlivé pripojenie medzi firebase a ESP32
   - Upozornenie v prípade chyby 

4. **Výkon (Performance - P)**
   - Stránka vyžaduje minimálny výkon na zariadení 

5. **Schopnosť údržby (Supportability - S)**
   - V prípade chýb by bola stránka upravená a opravená 

---

## Kritické situácie
1. **Systémové**
   - Výpadok napájania: Garáž nie je schopná pracovať bez prísunu elektrickej energie.
   - Výpadok internetového pripojenia: Stránka stráca spojenie s ESP32 a databázou firebase

2. **Aplikačné**
   - Chyba komunikácie medzi webom a databázou: Zlyhanie zápisu/čítania stavov

---

## Tri situácie definujúce hranice systému
1. **Ideálny scenár**
   - Užívateľ zvolí akciu na webe, príkaz sa okamžite zapíše do Firebase, garáž vykoná pohyb a web zobrazí aktualizovaný stav v reálnom čase.

2. **Hranične riešiteľný scenár**
   - Pripojenie je pomalé alebo nestabilné. Aplikácia počká na potvrdenie z databázy, zobrazí sa indikátor načítavania a po nadviazaní spojenia dáta zosynchronizuje.

3. **Nevyriešiteľný scenár**
   - Úplný výpadok internetu na strane používateľa alebo zlyhanie Firebase cloudu. Aplikácia nevie odoslať príkaz a zobrazí chybovú správu s možnosťou opätovného načítania.

---

## Kontext prostredia
Stránka funguje len po prihlásení sa a úspešnej verifikácii.

---

## Charakteristika aktérov a prostredia
- **Aktéri**: Osoba parkujúca auto, ESP32
- **Prostredie**: internetový prehliadač, garáž

---

## Use Case diagram
- **Minimálne 5 modulov a 2 aktéry**
- Doporučené maximum: 5 modulov s využitím `include` a `extend` vzťahov.
- <img width="842" height="462" alt="Untitled Diagram drawio (3)" src="https://github.com/user-attachments/assets/f719f494-b497-4ff5-8f89-c82c75189ff0" />



---

## Scenáre - konkrétna implementácia Use Case

**1. Odoslanie požiadavky na zaparkovanie / vyparkovanie vozidla**  
   - **Názov**: Odoslanie požiadavky na zaparkovanie / vyparkovanie  
   - **Kontext**: Používateľ chce prostredníctvom webovej aplikácie zaparkovať vozidlo do voľného slotu alebo privolať zaparkované vozidlo z garáže.
   - **Level zanoření Use Case**: Hlavný scénar  
   - **Aktéri**: Používateľ, ESP32
   - **Stakeholdeři a zájmové osoby**: Majiteľ garáže, vodiči využívajúci garážový systém
   - **Vstupné podmienky**: Používateľ je prihlásený vo webovej aplikácii a systém je pripojený k internetu.
   - **Výstupné podmienky**:Príkaz je zapísaný do Firebase databázy a ESP32 vykoná mechanický pohyb garáže.
   - **Minimálny výstup**: Zobrazenie stavu spracovania požiadavky na webe.
   - **Ideálny výstup**: Úspešný pohyb garáže do požadovanej polohy a okamžitá aktualizácia stavu v aplikácii.

**Hlavný scénár**:  
1. Používateľ na webovej stránke zvolí akciu.
2. Webová aplikácia zapíše požiadavku do Firebase databázy
3. ESP32 načíta zmenu z databázy a vykoná mechanický pohyb bubnu/výťahu.
4. Po dokončení pohybu ESP32 aktualizuje stav v databáze a web zobrazí novú polohu.

**Rozšírenie**:  
- Ak zlyhá pripojenie na internet: Aplikácia nezapíše príkaz do Firebase a zobrazí používateľovi chybovú správu o nedostupnosti sieťového pripojenia.
- Ak ESP32 neodpovedá: Aplikácia po uplinutí časového limitu vyvolá chybové upozornenie.
---

## Sekvenčný diagram

<img width="652" height="659" alt="Untitled Diagram drawio (6)" src="https://github.com/user-attachments/assets/4d02d7f8-1e71-40ff-b7c3-91df469bf918" />


---

## Triedny diagram
- Zobraziť triedy ako `Vehicle`, `ECUDiagnosticTool`, `OBD2_Codes` a ich vzťahy.

---

## Aktivitný diagram — *bonus*

> Nie je povinný. Za dobre spracovaný diagram sú **plusové body**.

Vezmite **jeden zložitejší scenár** z kapitoly *Scenáre* (ideálne taký, kde je
vetvenie alebo viac krokov za sebou) a rozkreslite jeho tok ako **diagram aktivít**:

- počiatočný uzol → akcie → **rozhodovací uzol** s podmienkami `[…]` → koncový uzol
- ak v scenári niečo prebieha súbežne, použite **fork / join**
- ak je pri akcii jasné, kto ju vykonáva (mechanik vs systém), rozdeľte akcie do **plaveckých dráh**

Notácia a hotový príklad: [Úvod do softvérového inžinierstva → Diagram aktivít](/citacka.html?s=oop&doc=uvod-do-si#diagram-aktivit)

---

## BPMN diagram — *bonus*

> Nie je povinný. Za dobre spracovaný diagram sú **plusové body**.

BPMN nie je súčasťou UML — je to štandard na modelovanie **biznis procesu**, do
ktorého systém zapadá. Ukážte **jeden proces** okolo vášho systému (napr. „príjem
vozidla do servisu a diagnostika") a zamerajte sa na:

- **bazén a dráhy** — kto je účastník (zákazník, mechanik, systém)
- **typy úloh** — čo robí človek cez systém (*user task*) vs čo systém automaticky (*service task*)
- **brány** — kde sa proces vetví (`×` exkluzívna brána)
- **štartovú a koncové udalosti**

Notácia, typy úloh a hotový príklad: [Úvod do softvérového inžinierstva → BPMN](/citacka.html?s=oop&doc=uvod-do-si#bpmn-procesny-pohlad)

---

## Wireframe kľúčových obrazoviek — *bonus*

> Nie je povinný. Za dobre spracovaný wireframe sú **plusové body**.

Načrtnite **2–3 kľúčové obrazovky** vášho systému — nízkofidelitný wireframe
(rozloženie prvkov, žiadne farby ani finálny dizajn). Každú obrazovku viažte na
konkrétny use case (napr. formulár novej žiadanky = UC „vytvoriť žiadanku",
zoznam so stavmi = UC „sledovať stav").

Toto je zároveň **návrh aplikácie, ktorú budete postupne implementovať** na
hodinách programovania — oplatí sa navrhnúť niečo, čo naozaj chcete mať hotové.

Úrovne (wireframe → mockup → prototyp) a hotový príklad:
[Úvod do softvérového inžinierstva → Wireframe a mockup](/citacka.html?s=oop&doc=uvod-do-si#wireframe-a-mockup)

---


# Rozšírenie FURPS analýzy pre projekt diagnostického softvéru pre automobily

## 1. **S.M.A.R.T. Ciele (Specific, Measurable, Achievable, Relevant, Time-bound)**
Táto metodika pomáha definovať jasné a merateľné ciele, ktoré by mal systém splniť. Použitie tejto analýzy môže byť veľmi užitočné na určenie konkrétnych cieľov pre implementáciu systému:
- **Specific (Špecifické)**: Čo presne má systém robiť? (napr. čítanie diagnostických kódov)
- **Measurable (Merateľné)**: Ako budeme hodnotiť úspech? (napr. doba odozvy systému pri diagnostike)
- **Achievable (Dosiahnuteľné)**: Je tento cieľ realistický s dostupnými zdrojmi?
- **Relevant (Relevantné)**: Má tento cieľ skutočne hodnotu pre používateľov systému?
- **Time-bound (Časovo ohraničené)**: Kedy by mal byť cieľ dosiahnutý?

---

## 2. **SWOT analýza (Strengths, Weaknesses, Opportunities, Threats)**
SWOT analýza je skvelý nástroj na hodnotenie silných a slabých stránok systému, ako aj príležitostí a hrozieb, ktoré môžu ovplyvniť jeho úspešnosť:
- **Strengths (Silné stránky)**: Aké sú hlavné výhody systému (napr. vysoká spoľahlivosť)?
- **Weaknesses (Slabé stránky)**: Kde má systém slabiny (napr. obmedzená podpora pre staršie modely vozidiel)?
- **Opportunities (Príležitosti)**: Aké príležitosti existujú pre rozšírenie systému (napr. pripojenie na mobilné aplikácie)?
- **Threats (Hrozby)**: Aké externé faktory by mohli ohroziť systém (napr. technológie konkurentov)?

---

## 3. **Risk Analysis (Analýza rizík)**
Risk analýza sa zameriava na identifikáciu a hodnotenie potenciálnych rizík spojených s vývojom a implementáciou systému:
- **Technologické riziká**: Napríklad problémy s integráciou nových modelov vozidiel alebo zmeny v OBD-II protokole.
- **Projektové riziká**: Napríklad oneskorenie v implementácii alebo nepredvídané náklady.
- **Bezpečnostné riziká**: Riziká spojené s ochranou dát a citlivých informácií.

---

## 4. **UML (Unified Modeling Language) Diagramy**
Okrem FURPS analýzy môžu študenti využiť aj rôzne UML diagramy, ako sú:
- **Triedne diagramy**: Ukazujú štruktúru systému a jeho komponenty (triedy a objekty) s atribútmi a metódami.
- **Sekvenčné diagramy**: Ukazujú časovú posloupnosť udalostí a interakcií medzi rôznymi komponentami systému.
- **Stavové diagramy**: Zobrazujú rôzne stavy systému a prechody medzi nimi na základe určitých podmienok.
- **Aktivitné diagramy**: Vizualizujú tok aktivít v systéme a rozhodovanie medzi rôznymi operáciami.

---

## 5. **Agilné metodiky (Scrum, Kanban)**
Pre projektový manažment je možné použiť agilné metodiky na riadenie vývoja systému. Tieto metodiky sú obzvlášť užitočné pri dynamických projektoch, kde sa môže meniť rozsah a požiadavky:
- **Scrum**: Metodika, ktorá sa zameriava na pravidelné iterácie a tým aj rýchlejšie nasadzovanie nových funkcií.
- **Kanban**: Vizualizuje pracovný tok a umožňuje sledovať stav jednotlivých úloh v reálnom čase.

---

## 6. **Testovacia analýza (Testovanie kvality)**
Kvalitné testovanie je neoddeliteľnou súčasťou každého systému. Testovacia analýza by mala zahŕňať:
- **Unit Testing (Jednotkové testy)**: Testovanie jednotlivých komponentov systému.
- **Integration Testing (Integračné testy)**: Testovanie interakcie medzi rôznymi časťami systému.
- **Acceptance Testing (Akceptačné testy)**: Overenie, či systém spĺňa požiadavky používateľa a obchodné ciele.

---

## 7. **Vývojový životný cyklus (SDLC - Software Development Life Cycle)**
Pre štruktúrovaný vývoj môže byť užitočné dodržiavať niektorý z modelov vývojového životného cyklu:
- **Waterfall**: Tradičný prístup s fázami ako analýza, návrh, implementácia a testovanie.
- **Agile**: Flexibilnejší prístup s častými iteráciami a zlepšovaním systému.

---

# Zhrnutie
Na obohatenie tvojej video analýzy FURPS môžeš zvážiť pridanie ďalších metodík a nástrojov ako:
- **S.M.A.R.T. Ciele** na definovanie konkrétnych a merateľných cieľov.
- **SWOT analýza** na hodnotenie silných a slabých stránok systému.
- **Risk Analysis** na identifikáciu a hodnotenie potenciálnych rizík.
- **UML diagramy** na vizualizáciu a detailnejšie pochopenie systému.
- **Agilné metodiky** na riadenie projektu a iteratívny vývoj.
- **Testovacia analýza** na zabezpečenie kvality systému.
- **SDLC modely** na riadenie vývoja.

Tieto metódy a analýzy môžu študentom pomôcť lepšie pochopiť rôzne aspekty systému a jeho vývoja, čo je veľmi užitočné pri implementácii skutočných softvérových riešení.

