# Understand the output log from CPLEX solver

> Ref: <https://www.ibm.com/support/knowledgecenter/SSSA5P_12.6.3/ilog.odms.cplex.help/CPLEX/UsrMan/topics/discr_optim/mip/para/52_node_log.html>

### Solver Output

```
MIP search method: dynamic search.
Parallel mode: none, using 1 thread.
Root relaxation solution time = 0.00 sec (0.00 ticks)

         Nodes                                 Cuts/   
   Node  Left     Objective  IInf  Best Integer     Best Bound    ItCnt    Gap
*     0+    0                            0.0000     3261.8212        8     ---
*     0+    0                         3148.0000     3261.8212        8    3.62%
      0     0     3254.5370     7     3148.0000       Cuts: 5       14    3.38%
      0     0     3246.0185     7     3148.0000       Cuts: 3       24    3.11%
*     0+    0                         3158.0000     3246.0185       24    2.79%
      0     0     3245.3465     9     3158.0000       Cuts: 5       27    2.77%
      0     0     3243.4477     9     3158.0000       Cuts: 5       32    2.71%
      0     0     3242.9809    10     3158.0000     Covers: 3       36    2.69%
      0     0     3242.8397    11     3158.0000     Covers: 1       37    2.69%
      0     0     3242.7428    11     3158.0000       Cuts: 3       39    2.68%
      0     2     3242.7428    11     3158.0000     3242.7428       39    2.68%
     10    11     3199.1875     2     3158.0000     3215.1261       73    1.81%
*    20+   11                         3168.0000     3215.1261       89    1.49%
     20    13     3179.0028     5     3168.0000     3215.1261       89    1.49%
     30    15     3179.9091     3     3168.0000     3197.5227      113    0.93%
*    39     3      integral     0     3186.0000     3197.3990      126    0.36%
     40     3     3193.7500     1     3186.0000     3197.3990      128    0.36% 

Cover cuts applied:  9
Zero-half cuts applied:  2
Gomory fractional cuts applied:  1

Solution pool: 5 solutions saved. 
MIP-Integer optimal solution:  Objective =   3.1860000000e+03
Solution time =    0.01 sec.  (0.00 ticks)  Iterations = 131   Nodes = 44
```

### Column Meaning

- **(1st)** : * - integer-feasible solution (new incumbent) is found
- **Node** : current node number
- **Nodes Left** : number fo nodes left to explore
- **Objective** : 
    - Objective value at the node; or
    - Reason to fathom the node, because
        - Subproblme at the node is infeasible; or
        - Value of the objective function at the node is worse that the cutoff value for *branch & cut*; or
        - LP relaxation of the node supplies an integer solution
    - (Note: blank means CPLEX found a new incumbent by primal heuristics -- a plus(+) appears after node number)
        - e.g., Node : 0, 20
- **IInf** : number of integer-**in**feasible variables and special ordered sets
- **Best Integer** : the objective value of the best integer solution found so far (blank if no solution found)
- **Cuts/Best Bound** : the best objective function vallue achievable
    - *Cuts* : means various cuts generated
    - *\<particular name of a cut\>* (e.g., Covers) : only that kind of cut was generated
- **ItCnt** : cumulative iteration count when solving the subproblems
- **Gap** : relative gap value
    - hyphen (-) : gap > 999.99%
    - blank : no solution so far
    - gap := (best integer - best node) * objsen / (abs(best ineger) )