# Matice RACI: změna řídicího systému (to-be)

[English](raci.md) · **Čeština**

Kdo v procesu [to-be](../README_cz.md#proces-to-be) jednotlivé činnosti dělá a kdo za ně odpovídá.

- **R** (Responsible) činnost provádí,
- **A** (Accountable) za činnost odpovídá a má poslední slovo, u každé činnosti je to právě jedna role,
- **C** (Consulted) je konzultován před rozhodnutím nebo během práce,
- **I** (Informed) je informován o výsledku.

R/A znamená, že činnost provádí stejná role, která za ni odpovídá.

| Činnost | Operátor | Analytik | Mistr elektro | Externí programátor | Vedoucí provozu |
|---------|:--------:|:--------:|:-------------:|:-------------------:|:---------------:|
| Zapsat požadavek do evidence | R/A | I | I | | |
| Vyjasnit požadavek s operátorem | C | R/A | | | |
| Schválit požadavek | I | C | R/A | | |
| Analyzovat data (alarmy, historie poruch) | | R/A | C | | |
| Rozhodnout, zda je potřeba změna programu | | R | A | | |
| Rozhodnout, zda je změna akutní | C | I | R/A | | |
| Zpracovat specifikaci vč. akceptačních kritérií | C | R/A | C | | |
| Vyjádřit se ke specifikaci | | I | I | R/A | |
| Rozhodnout, zda lze změnu realizovat | | R | A | C | |
| Dohodnout termín nahrání | | I | R/A | C | C |
| Naplánovat odstávku | I | | C | I | R/A |
| Implementovat a nahrát změnu | | I | I | R/A | |
| Otestovat změnu na místě (UAT) | R | R/A | C | | |
| Rozhodnout o návratu na původní verzi | | C | R/A | I | |
| Potvrdit správnost a funkčnost | I | C | R/A | | |
| Zaškolit operátory a aktualizovat dokumentaci | I | R/A | I | | |

## Pravidla, ze kterých matice vychází
- O elektrických zařízeních má vždy poslední slovo mistr elektro. Proto je A i u rozhodnutí, která připravuje
  a navrhuje analytik.
- Test na místě vede analytik, kvitaci a obsluhu na velínu vždy zkouší operátor.
- Vedoucí provozu se o konkrétní změně dozvídá od mistra při domluvě termínu nahrání.
- Externí programátor odpovídá za úpravu programu a její nahrání. O jeho vnitřních krocích matice nic neříká,
  stejně jako diagram, kde má programátor samostatný pool.
