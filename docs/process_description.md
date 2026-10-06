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
| 12 | Změna nahrána (Change uploaded) | Analyst | message ZMĚNA NAHRÁNA | | waits for the programmer, the upload takes place only during a shutdown |
| 13 | Otestovat změnu na místě (Test the change on site, UAT) | Analyst and operator | uploaded change | test result | checked on the equipment itself, for example on the frequency converter |
| 14 | Funguje? (Does it work?) | Analyst | test result | yes / no | if no, continue with step 15 |
| 15 | Nahlásit chybu programátorovi (Report the fault to the programmer) | Analyst | test result | message POPIS CHYBY | back to step 12 (rework loop), urgent problems by phone |
| 16 | Potvrdit správnost a funkčnost (Confirm correctness and function) | Foreman | test result | confirmation | the foreman also decides on a rollback if the change causes problems |
| 17 | Zaškolit operátory a aktualizovat dokumentaci (Train operators and update documentation) | Analyst | confirmed change | trained operators, updated documentation | end event: Změna předána do provozu (Change handed over to operation) |

## Messages between the analyst and the external programmer
| Message | From | To | Channel |
|---------|------|----|---------|
| SPECIFIKACE (specification) | Analyst | Programmer | e-mail |
| ZMĚNA NAHRÁNA (change uploaded) | Programmer | Analyst | e-mail or phone |
| POPIS CHYBY (fault description) | Analyst | Programmer | e-mail, in urgent cases phone and remote connection |

## Not drawn in the diagram
- The program backup and the remote rollback are handled by the programmer, inside the programmer's pool.
  The decision to roll back is made by the foreman.
- The shutdown itself (kilns in stand-by mode, cooling blowers running) is a precondition of the upload,
  not a step of this process.
