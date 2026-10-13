# RACI matrix: control system change (to-be)

**English** · [Čeština](raci_cz.md)

Who does each activity of the [to-be process](../README.md#to-be-process) and who is accountable for it.

- **R** (Responsible) does the activity,
- **A** (Accountable) is answerable for the activity and has the final say, exactly one role per activity,
- **C** (Consulted) is consulted before a decision or during the work,
- **I** (Informed) is informed about the result.

R/A means the activity is done by the same role that is accountable for it.

| Activity | Operator | Analyst | Electrical foreman | External programmer | Operations manager |
|----------|:--------:|:-------:|:------------------:|:-------------------:|:------------------:|
| Record the request in the log | R/A | I | I | | |
| Clarify the request with the operator | C | R/A | | | |
| Approve the request | I | C | R/A | | |
| Analyse data (alarms, fault history) | | R/A | C | | |
| Decide whether a program change is needed | | R | A | | |
| Decide whether the change is urgent | C | I | R/A | | |
| Prepare the specification incl. acceptance criteria | C | R/A | C | | |
| Reply to the specification | | I | I | R/A | |
| Decide whether the change can be done | | R | A | C | |
| Agree the upload date | | I | R/A | C | C |
| Plan the shutdown | I | | C | I | R/A |
| Implement and upload the change | | I | I | R/A | |
| Test the change on site (UAT) | R | R/A | C | | |
| Decide on restoring the previous version | | C | R/A | I | |
| Confirm correctness and function | I | C | R/A | | |
| Train operators and update documentation | I | R/A | I | | |

## Rules behind the matrix
- The electrical foreman always has the final say on electrical equipment. That is why the foreman is A even for
  decisions the analyst prepares and proposes.
- The analyst leads the on-site test, the operator always tries the reset and the operation in the control room.
- The operations manager learns about a specific change from the foreman when the upload date is agreed.
- The external programmer is accountable for changing and uploading the program. The matrix says nothing about the
  programmer's internal steps, in the same way as the diagram, where the programmer has a separate pool.
