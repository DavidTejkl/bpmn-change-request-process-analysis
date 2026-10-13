# User stories: kvitace poruch frekvenčních měničů z velínu

[English](user_stories.md) · **Čeština**

Požadavky ze [specifikace](specification_fm_reset_cz.md) zapsané z pohledu uživatelů. Priority podle metody MoSCoW
jsou nastavené zpětně podle toho, co bylo pro změnu nezbytné: Must (musí), Should (měl by), Could (mohl by),
Won't (tentokrát ne).

| ID | User story | Priorita | Vazba na specifikaci | Stav |
|----|------------|----------|----------------------|------|
| US-01 | Jako operátor chci kvitovat přetížení, nadproud a poruchu komunikace dmychadla z velínu, abych nečekal na elektrikáře a výroba nebyla omezená. | Must | FR-02 až FR-04, AC-01 až AC-03 | dodáno |
| US-02 | Jako elektrikář chci, aby závažné poruchy, například výpadek fáze, z velínu kvitovat nešly, abych je prověřil na místě a motor neshořel. | Must | FR-05, NFR-01, AC-04 | dodáno |
| US-03 | Jako operátor chci podle barvy pohonu poznat, zda kvitace proběhla, abych věděl, kdy volat elektrikáře. | Must | FR-06, AC-01 až AC-04 | dodáno |
| US-04 | Jako elektrikář chci dál kvitovat poruchy přímo na měniči v rozvodně, abych mohl vyřešit i závažnou poruchu na místě. | Must | NFR-02, AC-05 | dodáno |
| US-05 | Jako mistr elektro chci vidět počet kvitací pohonu za směnu, abych poznal opakující se poruchu. | Could | specifikace, část 11 | návrh, neimplementováno |
| W-01 | Kvitace závažných poruch z velínu. | Won't | BR-02 | záměrně vyřazeno z bezpečnostních důvodů |

Změna byla malá a jednoúčelová, proto jsou téměř všechny stories Must. Položky Could a Won't ukazují, kde vede
hranice rozsahu: US-05 je návrh na další zlepšení, W-01 je vědomě vyloučená funkce.
