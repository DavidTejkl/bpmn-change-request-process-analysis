# User stories: resetting frequency converter faults from the control room

**English** · [Čeština](user_stories_cz.md)

The requirements of the [specification](specification_fm_reset.md) written from the users' point of view. The
MoSCoW priorities are set retrospectively by what the change could not do without: Must, Should, Could,
Won't (not this time).

| ID | User story | Priority | Link to the specification | Status |
|----|------------|----------|---------------------------|--------|
| US-01 | As an operator I want to reset an overload, overcurrent or communication fault of a blower from the control room, so that I do not wait for an electrician and production is not limited. | Must | FR-02 to FR-04, AC-01 to AC-03 | delivered |
| US-02 | As an electrician I want serious faults, for example a phase loss, to be impossible to reset from the control room, so that I check them on site and the motor does not burn out. | Must | FR-05, NFR-01, AC-04 | delivered |
| US-03 | As an operator I want to see from the colour of the drive whether the reset worked, so that I know when to call an electrician. | Must | FR-06, AC-01 to AC-04 | delivered |
| US-04 | As an electrician I want to keep resetting faults directly on the converter in the switchroom, so that I can solve a serious fault on site. | Must | NFR-02, AC-05 | delivered |
| US-05 | As the electrical foreman I want to see the number of resets per drive and shift, so that I spot a recurring fault. | Could | specification, section 11 | proposal, not implemented |
| W-01 | Resetting serious faults from the control room. | Won't | BR-02 | deliberately excluded for safety reasons |

The change was small and had a single purpose, so almost all stories are Must. The Could and Won't items show
where the scope boundary lies: US-05 is a proposal for a further improvement, W-01 is a deliberately excluded
function.
