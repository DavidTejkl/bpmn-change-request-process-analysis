# Specifikace požadavku: kvitace poruch frekvenčních měničů z velínu

[English](specification_fm_reset.md) · **Čeština**

| Položka | Hodnota |
|---------|---------|
| Požadavek | Kvitace vybraných poruch frekvenčních měničů (FM) dmychadel z velínu |
| Žadatel | operátoři velínu |
| Analytik | David Tejkl |
| Schvaluje | mistr elektro |
| Realizace | externí programátor řídicího systému |
| Nasazení | červen 2025 |
| Stav | předáno do provozu |

Dokument je zpětně zpracovaná specifikace skutečné změny. Původní podklad pro programátora tvořil seznam pohonů
v Excelu a popis v e-mailu, akceptační kritéria se domlouvala ústně až při testu. Tato verze ukazuje stejný
požadavek ve formě, kterou navrhuje proces to-be: s očíslovanými požadavky a akceptačními kritérii.

## 1. Problém
Každá pec má 10 dmychadel poháněných motory s frekvenčními měniči:
- 5 dmychadel chladicího vzduchu, která vhánějí vzduch do hořáků, ve kterých právě neprobíhá cyklus pálení,
- 5 dmychadel spalovacího vzduchu, která vhánějí vzduch do pálicího pole. Podle potřeby jich běží jen část
  a operátor jim nastavuje výkon podle teploty a množství paliva.

Když měnič spadl do poruchy, například při přetížení nebo krátkém kolísání napětí za bouřky, šlo poruchu potvrdit
(kvitovat) jen přímo na měniči v rozvodně. Operátor musel zavolat elektrikáře a ten musel do rozvodny dojít. Podle
mé zkušenosti to trvalo zhruba 10 až 60 minut. Po tu dobu dmychadlo stálo a výroba byla omezená.

## 2. Cíl
Operátor kvituje vybrané poruchy měniče přímo z velínu bez příchodu elektrikáře. Cílová doba je v řádu sekund
(po nasazení přibližně 20 sekund). Závažné poruchy dál prověřuje elektrikář na místě.

## 3. Rozsah
**V rozsahu**
- 20 pohonů: 2 pece, na každé 5 dmychadel chladicího a 5 dmychadel spalovacího vzduchu,
- okno pohonu ve vizualizaci na velínu (HMI) a tlačítko pro kvitaci,
- kvitace poruch uvedených v pravidle BR-01.

**Mimo rozsah**
- změna parametrů měničů,
- kvitace závažných poruch (pravidlo BR-02),
- změna zadávání výkonu dmychadel z otáček na procenta (samostatný požadavek).

## 4. Zúčastněné strany
| Role | Zapojení |
|------|----------|
| Operátor | žadatel, uživatel funkce, zkoušel kvitaci z velínu při testu |
| Analytik | vyjasnění požadavku, specifikace, příprava a vedení testu |
| Mistr elektro | výběr poruch, schválení změny po testu |
| Odborník na frekvenční měniče (interní údržba) | výběr poruch, simulace poruch v parametrech měniče při testu |
| Externí programátor | úprava programu a vizualizace, nahrání změny |

## 5. Funkční požadavky
| ID | Požadavek |
|----|-----------|
| FR-01 | Po kliknutí na symbol pohonu ve vizualizaci se otevře okno daného pohonu. |
| FR-02 | Okno pohonu obsahuje tlačítko „KVIT PORUCHY FM“. |
| FR-03 | Stisk tlačítka odešle povel ke kvitaci poruchy do měniče pohonu, jehož okno je otevřené. |
| FR-04 | Kvitovat z velínu lze poruchy uvedené v pravidle BR-01. |
| FR-05 | Poruchy uvedené v pravidle BR-02 z velínu kvitovat nelze. Pohon zůstane v poruše. |
| FR-06 | Pohon v poruše je ve vizualizaci zobrazen červeně. Po úspěšné kvitaci se vrátí do normálního zobrazení. Pokud porucha trvá, zůstává červený. |
| FR-07 | Funkce je dostupná pro všech 20 pohonů v rozsahu. |

## 6. Nefunkční požadavky
| ID | Požadavek |
|----|-----------|
| NFR-01 | Bezpečnost: závažné poruchy nesmí být možné kvitovat vzdáleně, aby je elektrikář prověřil na místě a motor se nepoškodil. |
| NFR-02 | Kvitace přímo na měniči v rozvodně zůstává možná. |
| NFR-03 | Okno pohonu a tlačítko odpovídají stávajícímu vzhledu vizualizace. |
| NFR-04 | Změna se nahrává pouze při odstávce. |

## 7. Obchodní pravidla
| ID | Pravidlo |
|----|----------|
| BR-01 | Z velínu lze kvitovat: přetížení, nadproud (například při krátkém kolísání nebo výpadku napětí za bouřky) a poruchu komunikace. |
| BR-02 | Z velínu nelze kvitovat závažné poruchy, například výpadek fáze motoru. Ty prověřuje elektrikář na místě, aby motor neshořel. |
| BR-03 | Seznam poruch v BR-01 a BR-02 určili analytik, mistr elektro a odborník na frekvenční měniče. |
| BR-04 | Změnu schvaluje mistr elektro po úspěšném testu na místě. |

## 8. Akceptační kritéria
| ID | Výchozí stav | Akce | Očekávaný výsledek | Ověřuje |
|----|--------------|------|--------------------|---------|
| AC-01 | Pohon je v poruše přetížení. | Operátor otevře okno pohonu a stiskne „KVIT PORUCHY FM“. | Porucha je kvitována, pohon není zobrazen červeně. | FR-01 až FR-04, FR-06 |
| AC-02 | Pohon je v poruše nadproud. | Operátor stiskne „KVIT PORUCHY FM“. | Porucha je kvitována, pohon není zobrazen červeně. | FR-04, FR-06 |
| AC-03 | Pohon je v poruše komunikace. | Operátor stiskne „KVIT PORUCHY FM“. | Porucha je kvitována, pohon není zobrazen červeně. | FR-04, FR-06 |
| AC-04 | Pohon je v poruše výpadek fáze. | Operátor stiskne „KVIT PORUCHY FM“. | Porucha zůstává, pohon je dál zobrazen červeně. | FR-05, NFR-01 |
| AC-05 | Pohon je v kterékoli poruše. | Elektrikář kvituje poruchu přímo na měniči v rozvodně. | Kvitace funguje jako před změnou. | NFR-02 |
| AC-06 | Změna je nahrána. | Kontrola okna u každého z 20 pohonů. | Každé okno má tlačítko „KVIT PORUCHY FM“. | FR-02, FR-07 |

## 9. Akceptační test (UAT)
| Položka | Hodnota |
|---------|---------|
| Termín | první pec při plánované odstávce kvůli jiným opravám, test probíhal souběžně s nimi (zhruba 4 až 5 hodin). Druhá pec stejným postupem při další odstávce. |
| Účastníci | analytik a mistr elektro u měničů v rozvodně, operátor na velínu, odborník na frekvenční měniče |
| Rozsah | všech 20 pohonů |
| Nalezené chyby | žádné |
| Výsledek | mistr elektro změnu schválil (BR-04) |

| Test | Kritérium | Postup | Skutečný výsledek | Stav |
|------|-----------|--------|-------------------|------|
| UAT-01 | AC-01 | Odborník na měniče nasimuloval přetížení v parametrech měniče. Operátor poruchu kvitoval z velínu. | Porucha kvitována, pohon není červený. | splněno |
| UAT-02 | AC-02 | Nadproud nebylo možné nasimulovat. Ověřen až při skutečné poruše v provozu po nasazení. | Porucha kvitována z velínu. | splněno, ověřeno v provozu |
| UAT-03 | AC-03 | Porucha komunikace vyvolána odpojením komunikačního portu. Operátor poruchu kvitoval z velínu. | Porucha kvitována, pohon není červený. | splněno |
| UAT-04 | AC-04 | Výpadek fáze vyvolán vyjmutím jedné ze tří pojistek pohonu. Operátor zkusil kvitovat z velínu. | Kvitace neproběhla, pohon zůstal červený. | splněno |
| UAT-05 | AC-05 | Po každé vyvolané poruše kvitace přímo na měniči v rozvodně. | Kvitace funguje. U výpadku fáze kvitace na měniči proběhne, ale měnič před dalším startem zkontroluje fáze a při chybějící fázi poruchu ihned vyhlásí znovu. | splněno |
| UAT-06 | AC-06 | Kontrola oken všech 20 pohonů. | Každé okno má tlačítko „KVIT PORUCHY FM“. | splněno |

## 10. Matice sledovatelnosti
| Požadavek | Kritérium | Test | Stav |
|-----------|-----------|------|------|
| FR-01 | AC-01 | UAT-01 | splněno |
| FR-02 | AC-01, AC-06 | UAT-01, UAT-06 | splněno |
| FR-03 | AC-01 | UAT-01 | splněno |
| FR-04 (BR-01) | AC-01, AC-02, AC-03 | UAT-01, UAT-02, UAT-03 | splněno |
| FR-05 (BR-02) | AC-04 | UAT-04 | splněno |
| FR-06 | AC-01, AC-02, AC-03 | UAT-01, UAT-02, UAT-03 | splněno |
| FR-07 | AC-06 | UAT-06 | splněno |
| NFR-01 | AC-04 | UAT-04 | splněno |
| NFR-02 | AC-05 | UAT-05 | splněno |
| NFR-03 | žádné | žádný | bez testu |
| NFR-04 | žádné | žádný | procesní pravidlo, testem se neověřuje |

Matice ukazuje, že požadavek NFR-03 (vzhled okna) neměl vlastní kritérium ani test. V další specifikaci by pro
něj stačilo kritérium „okno pohonu odpovídá vzhledu ostatních oken vizualizace“ ověřené při UAT-06.

## 11. Rizika a návrhy
- Kvitace nemá potvrzovací dialog ani omezení počtu pokusů. Pokud se stejná porucha opakuje, operátor ji může
  opakovaně kvitovat a skutečná příčina zůstane skrytá. Návrh: sledovat počet
  kvitací pohonu za směnu v historii alarmů a při opakování přivolat elektrikáře.
- Seznam kvitovatelných poruch je potřeba znovu posoudit při výměně typu měniče, protože kódy poruch se mohou lišit.
