# Analýza procesu: požadavek na změnu průmyslového řídicího systému

[English](README.md) · **Čeština**

Případová studie popisuje, jak se požadavek na změnu řídicího systému výroby (program PLC a vizualizace pro
obsluhu) dostane od nahlášení operátorem až k otestované změně v provozu. Proces je zachycen v notaci
**BPMN 2.0** ve stavu **as-is**, tedy tak, jak dnes skutečně probíhá. Vychází z mé praxe.

![Proces as-is: změna řídicího systému](diagrams/01_as_is_change_process.svg)

Zdrojový soubor pro draw.io: [`diagrams/01_as_is_change_process.drawio`](diagrams/01_as_is_change_process.drawio)

## Kontext
Jde o nepřetržitý provoz průmyslových pecí s několika výrobními linkami. Každá linka má vlastní PLC
a vizualizaci (HMI) na velínu.

Požadavky na změny přicházejí od operátorů pravidelně. Typicky jde o nesrozumitelnou chybovou hlášku, hodnotu
zobrazenou v nevhodných jednotkách nebo poruchu, kterou nelze potvrdit bez zásahu údržby. Operátoři je zapisují
do knihy požadavků na velínu. Akutní požadavky se řeší okamžitě. Ostatní, obvykle drobné úpravy, se shromažďují
a externí programátor je provede v jedné zakázce.

Programování zajišťuje externí dodavatel řídicího systému Simatic. Analytickou část změn mám na starosti od roku
2022, souběžně s prací elektrotechnika.

Studie je anonymizovaná a neobsahuje názvy firmy, linek, strojů ani osob.

## Role
| Role | Odpovědnost |
|------|-------------|
| Operátor | nahlašuje požadavek, v běžném provozu potvrzuje funkčnost změny |
| Analytik (moje role) | vyjasnění požadavku, analýza historie alarmů a poruch, specifikace, test na místě (UAT), zaškolení operátorů, dokumentace |
| Mistr elektro | schválení požadavku, spolurozhodnutí o změně programu, potvrzení výsledku, rozhodnutí o návratu na původní verzi |
| Externí programátor | implementace a nahrání změny. Jako samostatná organizace má v diagramu vlastní pool |

## Hranice procesu
Proces začíná problémem na lince a končí jedním ze tří výsledků:
- požadavek je zamítnut,
- problém je vyřešen bez změny programu,
- změna je předána do provozu.

Fakturace dodavatele do rozsahu nepatří, jde o samostatný proces.

## Klíčová rozhodnutí
1. **Schváleno?** Mistr posoudí, zda má požadavek smysl řešit.
2. **Je potřeba změna programu?** Rozhoduje analytik společně s mistrem na základě analýzy historie alarmů
   a poruch. Část problémů se vyřeší bez zásahu do programu.
3. **Funguje?** Změna se ověřuje přímo na místě (UAT). Pokud nefunguje, analytik předá programátorovi popis chyby
   a změna se vrací k přepracování, dokud test neprojde.

## Řízení rizik
Změny se nahrávají výhradně při odstávce, nikdy za výroby. Pece jsou v režimu stand-by, technologie stojí a běží
pouze chladicí dmychadla, která chrání hořáky před poškozením vysokou teplotou.

Dodavatel uchovává zálohu programu a v případě potřeby dokáže vzdáleně obnovit původní verzi (rollback).
O návratu rozhoduje mistr.

Specifikace a běžná komunikace s dodavatelem probíhá e-mailem. Akutní problémy se řeší telefonicky
a vzdáleným připojením.

## Můj podíl a příklady změn
U každé změny vyjasňuji s operátorem, co skutečně potřebuje, připravuji podklady pro programátora, testuji
výsledek na místě a po schválení zaškolím obsluhu a doplním dokumentaci. Několik změn, které tímto procesem
prošly:

- **Otáčky dmychadla v procentech.** Operátor zadával otáčky v ot/min, se kterými v praxi nepracuje. Po změně
  zadává procenta a přepočet provádí program.
- **Srozumitelná chybová hláška.** Hláška zobrazovala pouze kód pohonu. Programátorovi jsem předal přesné datum
  a čas poruchy. Podle nich dohledal v historii příčinu (přetížení frekvenčního měniče) a hláška ji nyní uvádí.
- **Kvitace poruch z velínu.** Vybrané poruchy frekvenčních měničů lze potvrdit přímo z velínu. Dříve musel
  elektrikář dojít na rozvodnu, což podle mé zkušenosti trvalo zhruba 10 až 60 minut. Nyní to operátor zvládne
  přibližně za 20 sekund.

## Modelovací rozhodnutí
- Externí programátor je samostatný pool, nikoli dráha. Jde o jinou organizaci, jejíž vnitřní kroky nejsou
  známé ani řízené z naší strany. S procesem ho propojují pouze toky zpráv: specifikace, nahrání změny
  a popis chyby.
- Tok procesu (plná šipka) nepřechází mezi pooly. Komunikace mezi organizacemi je vyjádřena tokem zpráv
  (čárkovaná šipka).
- Společné rozhodnutí analytika a mistra je kvůli přehlednosti zakresleno jako brána v dráze analytika
  s poznámkou.

## Omezení modelu a návrhy pro to-be
- Rollback ani plánování odstávky nejsou v diagramu zakresleny. Verze to-be by je doplnila jako samostatné kroky
  včetně rozhodnutí mistra, zda se vrací původní verze, nebo se chyba opraví.
- Diagram sleduje jeden požadavek a nerozlišuje akutní a dávkové změny. Verze to-be by přidala bránu „Akutní?“
  a krok sběru drobných změn do dávky.
- Kniha požadavků je vedena papírově. Digitální evidence (datum, žadatel, priorita, rozhodnutí, stav) by umožnila
  sledovat počet změn a dobu jejich vyřízení.
- Akceptační kritéria se domlouvají ústně až při testu. Pokud by byla součástí specifikace, test by byl
  opakovatelný a výsledek jednoznačný.

## Soubory
- `diagrams/` diagram BPMN (zdroj pro draw.io, SVG a PNG)
- `docs/process_description.md` popis jednotlivých kroků s rolí, vstupem, výstupem a pravidlem

Autor: David Tejkl · [LinkedIn](https://www.linkedin.com/in/david-tejkl) · [další projekty na GitHubu](https://github.com/DavidTejkl)
