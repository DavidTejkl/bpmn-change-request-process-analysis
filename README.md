# Process analysis: change request for an industrial control system

**English** · [Čeština](README_cz.md)

This case study describes how a change request for a production control system (PLC program and operator
visualisation) moves from the operator's report to a tested change in operation. The process is modelled in
**BPMN 2.0** as **as-is**, meaning the way it actually runs today. It is based on my own work.

![As-is process: control system change](diagrams/01_as_is_change_process.svg)

Source file for draw.io: [`diagrams/01_as_is_change_process.drawio`](diagrams/01_as_is_change_process.drawio)

The labels in the diagram are in Czech. A translation is in the [glossary](#glossary) at the end.

## Context
The setting is a continuous operation of industrial kilns with several production lines. Each line has its own
PLC and visualisation (HMI) in the control room.

Operators request changes regularly. Typical examples are an unclear fault message, a value shown in unsuitable
units, or a fault that cannot be reset without calling maintenance. Operators write the requests into a request
book in the control room. Urgent requests are handled immediately. The others, usually minor adjustments, are
collected and the external programmer implements them in one order.

Programming is done by an external supplier of the Simatic control system. I have been responsible for the
analysis side of these changes since 2022, alongside my work as an electrician.

The case study is anonymised and contains no company, line, machine or person names.

## Roles
| Role | Responsibility |
|------|----------------|
| Operator | reports the request, confirms in daily operation that the change works |
| Analyst (my role) | clarifying the request, analysing alarm and fault history, specification, on-site test (UAT), operator training, documentation |
| Electrical foreman | approving the request, joint decision on a program change, confirming the result, deciding on a rollback |
| External programmer | implementing and uploading the change. As a separate organisation it has its own pool in the diagram |

## Process scope
The process starts with a problem on the line and ends with one of three outcomes:
- the request is rejected,
- the problem is solved without a program change,
- the change is handed over to operation.

Supplier invoicing is out of scope, it is a separate process.

## Key decisions
1. **Approved?** The foreman decides whether the request is worth solving.
2. **Is a program change needed?** The analyst decides together with the foreman, based on the analysis of alarm
   and fault history. Some problems are solved without touching the program.
3. **Does it work?** The change is verified on site (UAT). If it does not work, the analyst sends the programmer
   a fault description and the change goes back for rework until the test passes.

## Risk control
Changes are uploaded only during a shutdown, never during production. The kilns are in stand-by mode, the plant
is stopped and only the cooling blowers keep running to protect the burners from heat damage.

The supplier keeps a backup of the program and can restore the previous version remotely if needed (rollback).
The foreman decides on a rollback.

Specifications and routine communication with the supplier go by e-mail. Urgent problems are handled by phone
and remote connection.

## My contribution and examples of changes
For each change I clarify with the operator what is really needed, prepare the input for the programmer, test
the result on site and, after approval, train the operators and update the documentation. A few changes that
went through this process:

- **Blower speed in percent.** The operator used to enter the speed in rpm, a unit not used in daily work.
  Now the operator enters a percentage and the program does the conversion.
- **Clear fault message.** The message showed only the drive code. I gave the programmer the exact date and time
  of the fault. Based on that, the programmer found the cause in the history (frequency converter overload) and
  the message now states it.
- **Fault reset from the control room.** Selected frequency converter faults can now be reset directly from the
  control room. Before, an electrician had to walk to the switchroom, which in my experience took roughly 10 to
  60 minutes. Now the operator does it in about 20 seconds.

## Modelling decisions
- The external programmer is a separate pool, not a lane. It is a different organisation whose internal steps
  are neither known nor controlled by us. It is connected to the process only by message flows: specification,
  change uploaded and fault description.
- Sequence flow (solid arrow) does not cross pools. Communication between organisations is shown as message flow
  (dashed arrow).
- For readability, the joint decision of the analyst and the foreman is drawn as a gateway in the analyst lane
  with a note.

## Model limitations and to-be proposals
- Rollback and shutdown planning are not drawn in the diagram. A to-be version would add them as separate steps,
  including the foreman's decision whether to restore the previous version or fix the fault.
- The diagram follows one request and does not distinguish urgent and batched changes. A to-be version would add
  an "Urgent?" gateway and a step that collects minor changes into a batch.
- The request book is kept on paper. A digital log (date, requester, priority, decision, status) would make it
  possible to track the number of changes and how long they take.
- Acceptance criteria are agreed verbally only during the test. If they were part of the specification, the test
  would be repeatable and the result unambiguous.

## Glossary
| Czech label in the diagram | English |
|----------------------------|---------|
| Změna řídicího systému | Control system change |
| Operátor · Analytik · Mistr elektro · Externí programátor | Operator · Analyst · Electrical foreman · External programmer |
| Problém na lince | Problem on the line |
| Nahlásit požadavek na změnu | Report a change request |
| Vyjasnit požadavek s operátorem | Clarify the request with the operator |
| Projednat požadavek s analytikem · Schváleno? | Discuss the request with the analyst · Approved? |
| Informovat operátora o zamítnutí · Požadavek zamítnut | Inform the operator · Request rejected |
| Analyzovat data (alarmy, historie poruch) | Analyse data (alarms, fault history) |
| Je potřeba změna programu? (rozhodnutí s mistrem) | Is a program change needed? (decided with the foreman) |
| Vyřešit bez změny programu · Vyřešeno | Solve without a program change · Solved |
| Zpracovat specifikaci a podklady | Prepare the specification and input |
| Předat specifikaci programátorovi | Send the specification to the programmer |
| Změna nahrána | Change uploaded |
| Otestovat změnu na místě (UAT) · Funguje? | Test the change on site (UAT) · Does it work? |
| Nahlásit chybu programátorovi | Report the fault to the programmer |
| Potvrdit správnost a funkčnost | Confirm correctness and function |
| Zaškolit operátory a aktualizovat dokumentaci | Train operators and update documentation |
| Změna předána do provozu | Change handed over to operation |
| SPECIFIKACE · ZMĚNA NAHRÁNA · POPIS CHYBY | SPECIFICATION · CHANGE UPLOADED · FAULT DESCRIPTION |

## Files
- `diagrams/` BPMN diagram (draw.io source, SVG and PNG)
- `docs/process_description.md` description of each step with role, input, output and rule

Author: David Tejkl · [LinkedIn](https://www.linkedin.com/in/david-tejkl) · [more projects on GitHub](https://github.com/DavidTejkl)
