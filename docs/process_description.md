# Process description: control system change (as-is)

All steps of the BPMN diagram in order. The Czech label is the one used in the diagram, the English translation
is in brackets.

| # | Step | Role | Input | Output | Rule / note |
|---|------|------|-------|--------|-------------|
| 1 | Problém na lince (Problem on the line) | Operator | problem during operation | | start event |
| 2 | Nahlásit požadavek na změnu (Report a change request) | Operator | problem | entry in the request book | urgent requests are handled immediately, the others are collected and implemented in one order |
| 3 | Vyjasnit požadavek s operátorem (Clarify the request with the operator) | Analyst | request book entry | clarified need: what, where and why | the goal is what the operator really needs, not only what was written down |
| 4 | Projednat požadavek s analytikem (Discuss the request with the analyst) | Foreman | clarified need | | |
| 5 | Schváleno? (Approved?) | Foreman | | yes / no | if no, continue with step 6 |
| 6 | Informovat operátora o zamítnutí (Inform the operator) | Foreman | rejection | operator informed | end event: Požadavek zamítnut (Request rejected) |
| 7 | Analyzovat data (Analyse data) | Analyst | alarm and fault history | cause, or the exact date and time of the fault | |
| 8 | Je potřeba změna programu? (Is a program change needed?) | Analyst and foreman | analysis | yes / no | joint decision, if no, continue with step 9 |
| 9 | Vyřešit bez změny programu (Solve without a program change) | Analyst | analysis | problem solved | end event: Vyřešeno (Solved) |
| 10 | Zpracovat specifikaci a podklady (Prepare the specification and input) | Analyst | analysis | specification, for example a list of drives in Excel | |
| 11 | Předat specifikaci programátorovi (Send the specification to the programmer) | Analyst | specification | message SPECIFIKACE | by e-mail, minor changes are sent together in one order |
| 12 | Vyjádření přijato (Reply received) | Analyst | message VYJÁDŘENÍ PROGRAMÁTORA | | waits for the programmer's reply, the foreman receives it in copy |
| 13 | Lze realizovat? (Can it be done?) | Analyst and foreman | programmer's reply | yes / other proposal / no | joint decision, if other proposal, continue with step 14, if no, with step 15 |
| 14 | Potvrdit návrh programátorovi (Confirm the proposal to the programmer) | Analyst | programmer's proposal | message SOUHLAS S NÁVRHEM | by e-mail, or the foreman by phone. So far a proposal has always been accepted |
| 15 | Informovat operátora o nerealizaci (Inform the operator that the change will not be made) | Foreman | reply that the change is not possible | operator informed | end event: Změna nerealizována (Change not made) |
| 16 | Změna nahrána (Change uploaded) | Analyst | message ZMĚNA NAHRÁNA | | waits for the programmer, the upload takes place only during a shutdown. A merging gateway before this step joins step 13 (yes), step 14 and step 19 |
| 17 | Otestovat změnu na místě (Test the change on site, UAT) | Analyst and operator | uploaded change | test result | checked on the equipment itself, for example on the frequency converter |
| 18 | Funguje? (Does it work?) | Analyst | test result | yes / no | if no, continue with step 19 |
| 19 | Nahlásit chybu programátorovi (Report the fault to the programmer) | Analyst | test result | message POPIS CHYBY | back to step 16 (rework loop), urgent problems by phone |
| 20 | Potvrdit správnost a funkčnost (Confirm correctness and function) | Foreman | test result | confirmation | the foreman also decides on a rollback if the change causes problems |
| 21 | Zaškolit operátory a aktualizovat dokumentaci (Train operators and update documentation) | Analyst | confirmed change | trained operators, updated documentation | end event: Změna předána do provozu (Change handed over to operation) |

## Messages between the analyst and the external programmer
| Message | From | To | Channel |
|---------|------|----|---------|
| SPECIFIKACE (specification) | Analyst | Programmer | e-mail |
| VYJÁDŘENÍ PROGRAMÁTORA (programmer's reply) | Programmer | Analyst, foreman in copy | e-mail |
| SOUHLAS S NÁVRHEM (proposal accepted) | Analyst or foreman | Programmer | e-mail from the analyst, or a phone call from the foreman |
| ZMĚNA NAHRÁNA (change uploaded) | Programmer | Analyst | e-mail or phone |
| POPIS CHYBY (fault description) | Analyst | Programmer | e-mail, in urgent cases phone and remote connection |

## Not drawn in the diagram
- The program backup and the remote rollback are handled by the programmer, inside the programmer's pool.
  The decision to roll back is made by the foreman.
- The shutdown itself (kilns in stand-by mode, cooling blowers running) is a precondition of the upload,
  not a step of this process.
