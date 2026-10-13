# Process analysis: change request for an industrial control system

**English** · [Čeština](README_cz.md)

This case study describes how a change request for a production control system (PLC program and operator
visualisation) moves from the operator's report to a tested change in operation. The process is modelled in
**BPMN 2.0** as **as-is**, meaning the way it actually runs today. It is based on my own work. A second diagram
in the **to-be** state shows the proposed changes to the process, see [To-be process](#to-be-process).

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
| Electrical foreman | approving the request, joint decision on a program change and on the programmer's reply, confirming the result, deciding on a rollback |
| External programmer | replying to the specification, implementing and uploading the change. As a separate organisation it has its own pool in the diagram |
| Operations manager | planning shutdowns. Has its own lane in the to-be diagram |

## Process scope
The process starts with a problem on the line and ends with one of four outcomes:
- the request is rejected,
- the problem is solved without a program change,
- the change is technically not possible,
- the change is handed over to operation.

Supplier invoicing is out of scope, it is a separate process.

## Key decisions
1. **Approved?** The foreman decides whether the request is worth solving.
2. **Is a program change needed?** The analyst decides together with the foreman, based on the analysis of alarm
   and fault history. Some problems are solved without touching the program.
3. **Can it be done?** The programmer replies to the specification and the foreman receives the reply in copy.
   The analyst and the foreman then decide based on the reply. The change can be done as specified, or the
   programmer proposes another solution and the analyst confirms it, or the change is technically not possible
   and the foreman informs the operator.
4. **Does it work?** The change is verified on site (UAT). If it does not work, the analyst sends the programmer
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
  programmer's reply, proposal accepted, change uploaded and fault description.
- Sequence flow (solid arrow) does not cross pools. Communication between organisations is shown as message flow
  (dashed arrow).
- For readability, the joint decision of the analyst and the foreman is drawn as a gateway in the analyst lane
  with a note.
- So far, another solution proposed by the programmer has always been accepted. There is therefore no "Accept the
  proposal?" gateway in the as-is, the model shows how the process actually runs.
- A merging gateway sits before the "Change uploaded" event. It joins three paths: the change as specified, the
  confirmed proposal of the programmer and the fix after a reported fault.

## As-is model limitations
- Rollback and shutdown planning are not drawn in the diagram. A to-be version would add them as separate steps,
  including the foreman's decision whether to restore the previous version or fix the fault.
- The diagram follows one request and does not distinguish urgent and batched changes. A to-be version would add
  an "Urgent?" gateway and a step that collects minor changes into a batch.
- The request book is kept on paper. A digital log (date, requester, priority, decision, status) would make it
  possible to track the number of changes and how long they take.
- Acceptance criteria are agreed verbally only during the test. If they were part of the specification, the test
  would be repeatable and the result unambiguous.

The to-be process in the next section addresses all four limitations.

## To-be process
![To-be process: control system change](diagrams/02_to_be_change_process.svg)

Source file for draw.io: [`diagrams/02_to_be_change_process.drawio`](diagrams/02_to_be_change_process.drawio)

Steps that change or are new compared to the as-is are green. The number in brackets matches the row of the
table. The process has a new lane for the operations manager, because the manager sets the shutdown date. The
programmer's reply to the specification and the decision "Can it be done?" work the same way as in the as-is,
so they are not green.

| No. | As-is | To-be | Benefit | What needs to be introduced |
|-----|-------|-------|---------|-----------------------------|
| 1 | Requests are written into a paper book in the control room. | The operator records the request in a log with fixed fields: date, requester, line, description, proposed priority, decision and status. The status is updated at every decision and at closure. | It is visible how many requests come in, how long they take and how many are rejected. | A shared table or list with fixed columns and a rule on who updates the status. |
| 2 | The process does not distinguish urgent and minor changes. | The foreman decides whether a change is urgent, meaning whether it endangers operation. The operator proposes the urgency. An urgent change goes straight to the specification. Minor changes are added to a batch and sent to the programmer once there are more of them. | An urgent problem does not wait. Minor changes are done in one order and in one shutdown. | A clear criterion of what is urgent. |
| 3 | The shutdown is only a precondition of the upload, it is not part of the process. | The foreman agrees the upload date with the programmer and the operations manager. The operations manager plans the shutdown and the programmer receives the date as a message. | Every upload is tied to a specific shutdown and it is clear who sets the date. | A rule that an urgent change is uploaded in an extra shutdown and a batch in a planned one. |
| 4 | Acceptance criteria are agreed verbally only during the test. | The specification contains acceptance criteria and the on-site test follows them. | The test is repeatable and the result unambiguous. | A specification template with a section for acceptance criteria. |
| 5 | Neither the rollback nor the decision about it is visible in the process. | If the change does not work, the foreman decides whether to restore the previous version. If yes, the foreman orders a rollback from the programmer. In both cases the analyst sends the programmer a fault description. | A change that does not work does not stay in operation without a decision, and it is clear who decides. | A rule for when the previous version is restored. The programmer already keeps the program backup today. |

The request log makes it possible to track these indicators:
- number of requests per month,
- average time from recording a request to handover to operation,
- share of rejected requests,
- share of urgent changes.

Modelling decisions in the to-be:
- Where two paths join again, there is a merging gateway. This keeps the flow unambiguous.
- The request log is a data store. The dotted line shows data being written, not the process flow.
- Rules that are not process steps (for example when a batch is sent to the programmer) are in the table, not in
  the diagram. This keeps the diagram readable.

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
| Vyjádření přijato | Reply received |
| Lze realizovat? (rozhodnutí s mistrem) · JINÝ NÁVRH | Can it be done? (decided with the foreman) · OTHER PROPOSAL |
| Potvrdit návrh programátorovi | Confirm the proposal to the programmer |
| Informovat operátora o nerealizaci · Změna nerealizována | Inform the operator that the change will not be made · Change not made |
| Změna nahrána | Change uploaded |
| Otestovat změnu na místě (UAT) · Funguje? | Test the change on site (UAT) · Does it work? |
| Nahlásit chybu programátorovi | Report the fault to the programmer |
| Potvrdit správnost a funkčnost | Confirm correctness and function |
| Zaškolit operátory a aktualizovat dokumentaci | Train operators and update documentation |
| Změna předána do provozu | Change handed over to operation |
| SPECIFIKACE · ZMĚNA NAHRÁNA · POPIS CHYBY | SPECIFICATION · CHANGE UPLOADED · FAULT DESCRIPTION |
| VYJÁDŘENÍ PROGRAMÁTORA (mistr v kopii) | PROGRAMMER'S REPLY (foreman in copy) |
| SOUHLAS S NÁVRHEM (e-mail, nebo mistr telefonicky) | PROPOSAL ACCEPTED (e-mail, or the foreman by phone) |
| **To-be only** | |
| Vedoucí provozu | Operations manager |
| Zapsat požadavek do evidence · Evidence požadavků | Record the request in the log · Request log |
| Datum, žadatel, linka, popis, návrh priority, rozhodnutí, stav. Stav se aktualizuje při každém rozhodnutí a uzavření. | Date, requester, line, description, proposed priority, decision, status. The status is updated at every decision and at closure. |
| Akutní? (ohrožuje provoz?) | Urgent? (does it endanger operation?) |
| Zařadit do dávky drobných změn | Add to the batch of minor changes |
| Zpracovat specifikaci vč. akceptačních kritérií | Prepare the specification incl. acceptance criteria |
| Dohodnout termín nahrání s programátorem a vedoucím provozu | Agree the upload date with the programmer and the operations manager |
| Naplánovat odstávku | Plan the shutdown |
| Otestovat změnu podle akceptačních kritérií (UAT) | Test the change against the acceptance criteria (UAT) |
| Vrátit původní verzi? · Nařídit rollback programátorovi | Restore the previous version? · Order a rollback from the programmer |
| TERMÍN NAHRÁNÍ · ROLLBACK | UPLOAD DATE · ROLLBACK |

## Files
- `diagrams/` as-is and to-be BPMN diagrams (draw.io source, SVG and PNG)
- `docs/process_description.md` description of each as-is step with role, input, output and rule
- [`docs/specification_fm_reset.md`](docs/specification_fm_reset.md) specification of the change "Fault reset
  from the control room" with requirements, business rules, acceptance criteria, UAT record and traceability matrix
- [`docs/user_stories.md`](docs/user_stories.md) user stories for the same change with MoSCoW priorities
- [`docs/raci.md`](docs/raci.md) RACI matrix of the to-be process

Author: David Tejkl · [LinkedIn](https://www.linkedin.com/in/david-tejkl) · [more projects on GitHub](https://github.com/DavidTejkl)
