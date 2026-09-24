
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
- <img width="1013" height="618" alt="image" src="https://github.com/user-attachments/assets/b5cb7fe0-36e7-4443-9a8f-2ef7ca91271b" />




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

<img width="684" height="634" alt="image" src="https://github.com/user-attachments/assets/2fa88a37-6028-4cb1-8684-5805262a95dc" />




---

## Triedny diagram
<img width="892" height="712" alt="Untitled Diagram drawio (8)" src="https://github.com/user-attachments/assets/15bb0d8f-874b-49b3-b051-2144cf9cb5c7" />


---

