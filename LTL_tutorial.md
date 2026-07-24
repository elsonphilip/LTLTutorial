### Tutorial on how to use LTL 

### What is LTL?
Standard logic only looks at a single moment in time. However, Linear Temporal Logic (LTL) adds time, and describes how a system should behave across a sequence of steps over time (s0, s1, s2, and so on).

Combining LLMs with LTL solves a big problem:
- LLMs are great at understanding human language, but they are not reliable for strict math or safety rules.
- Model checkers and planners need strict mathematical rules to run, but they cannot understand human instructions.

Using an LLM as a translator helps take plain English and turn it into verified LTL formulas that a robot or software system can run safely.

### Basic Operators
On top of the standard logic operators (AND, OR, NOT, Implies), LTL uses four main temporal operators:

- X (Next): p must be true in the very next time step.
- F (Eventually / Future): p will become true at least once in the future.
- G (Globally / Always): p must stay true continuously in every future step.
- U (Until): p stays true until q becomes true (and q is guaranteed to happen).

### Common Examples

When turning human requests into LTL, most instructions fit into two types of main patterns:

1. Safety (Avoid bad states)
   - Human: "Never drive into the hazard area."
   - Formula: G(!hazard)

2. Liveness (Reach a goal state)
   - Human: "Eventually return to the docking station."
   - Formula: F(dock)

### How Translation Works:

To prevent broken or incorrect logic from reaching the system, translation has to be done in three steps:

1. **Examples First:** The LLM needs to be given a few examples of English text paired with valid LTL syntax so it learns the expected format.
2. **Syntax Check:** The generated LTL string should go through a parser (like the spot library or flloat).
3. **Auto Fix:** If the parser flags a syntax error, that error message needs to be sent back to the LLM so it can fix its output automatically.


### Things to Remember:

- LLM outputs should always be passed through a formal parser before they are executed.
- The model should be prompted to list all variables before constructing the full logic formula.
- A loop should be kept open so that syntax parsing errors feed directly back into the LLM context for fast re-prompting (a self-refinement loop).
