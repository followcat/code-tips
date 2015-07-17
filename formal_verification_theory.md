# The theoretical knowledge about formal verification 

Work has a certain scientific nature this week.
I markdown some key word first:
```
                    Omnipotence Paradox
                            |
                    Halting Problem
                            |
                        Hoare Logic
                            |
                      Turing complete
                            |
                    --------------------
                    |                  |
                   ATP               Un ATP
      (Automated theorem proving)  (UnAutomated theorem proving)
                    |                   |
    Satisfiability modulo theories      |
                    |                   |
                   Z3            Calculus of Inductive Constructions
            (And so on...)              |
                    |                Gallina
                  PySMT                 |
                    |-------------------|
                            |
                Prove four-colour conjecture
                    (And so on...)
```

Another tools:
Bap: http://bap.ece.cmu.edu/
Input some binary and output BIL/BIR/ASM.

LLVM: http://llvm.org/
Input source code and output IR.

S2E: http://s2e.epfl.ch/
A virtual machine augmented with symbolic execution and modular path analyzers.

## How to use these tools
In fact, I don't have all the ideas connected.
But from this paper: http://www.jos.org.cn/html/2014/11/4609.htm#outline_anchor_11

Show that can translate C code to SMT model, then use EventAutomata
to check that model.

The most important thing is the C code translate to static single assignmen at step3.
We can get static single assignmen by using BAP/LLVM from binary/sourcecode too!
