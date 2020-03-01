
- taskproxy.spawned has been removed from data_messages.proto, need to regen the
protobuf py module (instructions in the .proto file).

- have made suicide triggers back compat, but would it be better to remove those
elements from the graph?  (depends on remaining legit uses, e.g. Tim W's).

- obsolete sequential tasks, or make sure they are handled correctly?

---------

## Multiple Prerequisites

(a single prerequisite may be conditional)

`(A|B) => D`, `C => D`

- '& C' prevents auto-reflow of `D` due to late execution of `B`
  - but recording that `D` spawn already won't hurt
- partially satisfied `D` could be removed if `C` finished and (`A` or `B`) finished?

-----------

- Should state reset be disallowed?  Instead of lying about the past, force
downstream tasks to carry on as you want regardless of the past.

State reset was needed in SoS to laboriously set up conditions for a reflow, or
otherwise to allow a waiting task downstream to run. In SoD there is no
upstream finished task to reset. Can just as well operate on the downstream
task instead.

Need to allow individual prerequisite satisfaction?

-------

Try number increments for auto retries
Submit number increments for auto retries and forced retrigger

If we force trigger a task that ran in the past, should its submit number
increment or go back to 1 (with an incremented flow number?)?
(if not, job logs get overwritten)
But if submit number, what happens downstream of a retrigger? Do we always have
to check that a task didn't run already?
Can we just move job logs out of the way instead?

Note that SoS submit num goes back to 1 if the task had to be inserted!!!

----

`cylc/flow/tests/(network/, test_data_store.py)` uses `insert_tasks()` (removed)

