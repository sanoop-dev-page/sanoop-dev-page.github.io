
Listing out some guidelines for block level TB owners to be mindful of, while developing the testbenches. These items ensure a faster reuse of the block level testbench at subsys/ chip_top levels.

## Sequences
Sequence should generate a legal constraint by default. 
- Use layered sequences whenever required. 
- Use config db effectively to make the sequence adaptable to different levels. (Do not make assignments to configurations with in the sequence, instead grab it from config db get)
## Components
Active/Passive components' behavior in each mode needs to be carefully orchestrated. 
- All the components in the testbench needs to take care of the value of its is_active  (eg, connection of sequencer and driver needs to be guarded under is_active)
- Active components should not
  - Update any members of cfg
  - dump important message that could uncover some difficult-to-debug errors
  - collect coverage
  - send information to scoreboard
  - implement any checks
  - influence test ending mechanism
