# Requirement specification: resetting frequency converter faults from the control room

**English** · [Čeština](specification_fm_reset_cz.md)

| Item | Value |
|------|-------|
| Request | Reset selected faults of the blower frequency converters (FC) from the control room |
| Requested by | control room operators |
| Analyst | David Tejkl |
| Approved by | electrical foreman |
| Implemented by | external control system programmer |
| Deployed | June 2025 |
| Status | handed over to operation |

This document is a retrospective specification of a real change. The original input for the programmer was a list
of drives in Excel and a description in an e-mail, and the acceptance criteria were agreed verbally only during
the test. This version shows the same request in the form proposed by the to-be process: with numbered
requirements and acceptance criteria.

## 1. Problem
Each kiln has 10 blowers driven by motors with frequency converters:
- 5 cooling air blowers, which blow air into the burners that are not in a firing cycle at the moment,
- 5 combustion air blowers, which blow air into the firing zone. Only some of them run, as needed, and the
  operator sets their output according to the temperature and the amount of fuel.

When a converter went into a fault, for example an overload or a short voltage fluctuation during a storm, the
fault could only be reset directly on the converter in the switchroom. The operator had to call an electrician,
who had to walk to the switchroom. In my experience this took roughly 10 to 60 minutes. During that time the
blower stood still and production was limited.

## 2. Goal
The operator resets selected converter faults directly from the control room without an electrician coming. The
target time is a matter of seconds (about 20 seconds after deployment). Serious faults are still checked by an
electrician on site.

## 3. Scope
**In scope**
- 20 drives: 2 kilns, each with 5 cooling air and 5 combustion air blowers,
- the drive window in the control room visualisation (HMI) and a reset button,
- resetting the faults listed in rule BR-01.

**Out of scope**
- changing converter parameters,
- resetting serious faults (rule BR-02),
- changing the blower output setting from speed to percent (a separate request).

## 4. Stakeholders
| Role | Involvement |
|------|-------------|
| Operator | requester, user of the function, tried the reset from the control room during the test |
| Analyst | clarifying the request, specification, preparing and leading the test |
| Electrical foreman | selecting the faults, approving the change after the test |
| Frequency converter specialist (internal maintenance) | selecting the faults, simulating faults in the converter parameters during the test |
| External programmer | changing the program and the visualisation, uploading the change |

## 5. Functional requirements
| ID | Requirement |
|----|-------------|
| FR-01 | Clicking the drive symbol in the visualisation opens the window of that drive. |
| FR-02 | The drive window contains the button „KVIT PORUCHY FM“ (reset FC fault). |
| FR-03 | Pressing the button sends a fault reset command to the converter of the drive whose window is open. |
| FR-04 | The faults listed in rule BR-01 can be reset from the control room. |
| FR-05 | The faults listed in rule BR-02 cannot be reset from the control room. The drive stays in fault. |
| FR-06 | A drive in fault is shown in red in the visualisation. After a successful reset it returns to the normal display. If the fault persists, it stays red. |
| FR-07 | The function is available for all 20 drives in scope. |

## 6. Non-functional requirements
| ID | Requirement |
|----|-------------|
| NFR-01 | Safety: it must not be possible to reset serious faults remotely, so that an electrician checks them on site and the motor is not damaged. |
| NFR-02 | Resetting directly on the converter in the switchroom remains possible. |
| NFR-03 | The drive window and the button match the existing look of the visualisation. |
| NFR-04 | The change is uploaded only during a shutdown. |

## 7. Business rules
| ID | Rule |
|----|------|
| BR-01 | The following can be reset from the control room: overload, overcurrent (for example after a short voltage fluctuation or outage during a storm) and communication fault. |
| BR-02 | Serious faults, for example a motor phase loss, cannot be reset from the control room. An electrician checks them on site so that the motor does not burn out. |
| BR-03 | The list of faults in BR-01 and BR-02 was set by the analyst, the electrical foreman and the frequency converter specialist. |
| BR-04 | The electrical foreman approves the change after a successful on-site test. |

## 8. Acceptance criteria
| ID | Initial state | Action | Expected result | Verifies |
|----|---------------|--------|-----------------|----------|
| AC-01 | The drive is in an overload fault. | The operator opens the drive window and presses „KVIT PORUCHY FM“. | The fault is reset, the drive is not shown in red. | FR-01 to FR-04, FR-06 |
| AC-02 | The drive is in an overcurrent fault. | The operator presses „KVIT PORUCHY FM“. | The fault is reset, the drive is not shown in red. | FR-04, FR-06 |
| AC-03 | The drive is in a communication fault. | The operator presses „KVIT PORUCHY FM“. | The fault is reset, the drive is not shown in red. | FR-04, FR-06 |
| AC-04 | The drive is in a phase loss fault. | The operator presses „KVIT PORUCHY FM“. | The fault stays, the drive is still shown in red. | FR-05, NFR-01 |
| AC-05 | The drive is in any fault. | An electrician resets the fault directly on the converter in the switchroom. | The reset works as before the change. | NFR-02 |
| AC-06 | The change is uploaded. | Check of the window of each of the 20 drives. | Every window has the button „KVIT PORUCHY FM“. | FR-02, FR-07 |

## 9. Risks and proposals
- The reset has no confirmation dialog and no limit on the number of attempts. If the same fault keeps coming
  back, the operator can reset it again and again and the real cause stays hidden. Proposal: track the number of
  resets per drive and shift in the alarm history and call an electrician when a fault repeats.
- The list of resettable faults needs to be reviewed again when the converter type is replaced, because the fault
  codes may differ.
