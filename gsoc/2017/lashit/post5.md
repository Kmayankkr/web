# Adapting TED-A* to AGGLPlanner

In **TED-A*** we've *`operator chunks`* consisting of several operators and each chunk has it's own *`run time`*. **TED-A*** consists of many *cycles*, in each *cycle* **A*** search is performed. Each cycle takes different amount of *`operator chunks`* into consideration. New *`operator chunks`* are introduced every cycle. In a cycle, **A*** is performed using an *`operator chunk`* for time equal to its *`run time`*. 


Let's say in a *cycle* we've:

`operator_chunk1`, `operator_chunk2`, .., `operator_chunkn`

having corresponding run-times:

`run_time1`, `run_time2`, .., `run_timen`

In **TED-A*** algorithm we'll perform **A*** search using an `operator_chunki` for it's `run_timei`. We'll do it for all *`operator chunks`* under consideration in the current cycle.

Three important things to note are: 
i) In **TED-A***, we assume all operator chunks to be disjoint.
ii) Number of *`operator chunk`* under consideration will be superset of it's preceeding cycles.
iii) When no longer *`operator chunk`* can be considered, a cycle will **saturate**. Every cycle after that will contain all possible *`operator chunk`* and each chunk will be run for it's corresponding *`run time`*. 

When introducing **TED-A*** to the current implementation of **AGGLPlanner**, we tweak the proposed **TED-A*** algorithm a bit for the ease of implementation. Now for every *cycle* we'll have only one *`operator chunk`* (which is union of all operator chunks for a *cycle* in **TED-A***). Also now, instead of *`operator chunk`*, every *cycle* will have a *`time chunk`*.

The only difference is the time allocation pattern to each *`operator chunk`*.  


* * *
Lashit Jain