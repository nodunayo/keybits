### Designing: The Value Of Time

Design should be done incrementally, on a daily basis, as part of a project.

"Far from 'design nothing', the XP strategy is 'design always'."

A powerful heuristic to think about is 'Once and Only Once':

Data, structure, or logic should only exist in one place. When we see duplication, we uncover the need for a design investment. You want to avoid duplication so that conceptual changes don't mean that you have to make code changes in several places. If your code has the property of 'Once and Only Once' then changes remain inexpensive.

Part of incremental design is working out when to do design improvements.

Design should be deferred until it can be made in light of experience and the decisions can be used immediately so that useful feedback can be gleaned from the changes.

#### Simplicity

The following are four criteria to evaluate the simplicity of a design:

1. Is it appropriate for the intended audience? The people who need to work with it have to understand it.
1. Is it communicative? Every idea that needs to be communicated should be represented in the system.
1. Is it factored? Duplication of logic and structure make code hard to understand and change.
1. Is it minimal? The system should have the fewest elements possible.

