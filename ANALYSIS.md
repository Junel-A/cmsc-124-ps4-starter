# Analysis

We traced P07, grandparent(tom, X). Line numbers below count from the == trace P07 == header as line 1.

```text
== trace P07 ==
Call: grandparent(tom,X)
  Call: parent(tom,_)
  Exit: parent(tom,liz)
  Call: parent(liz,X)
  Fail: parent(liz,X)
  Redo: parent(tom,liz)
  Exit: parent(tom,bob)
  Call: parent(bob,X)
  Exit: parent(bob,ann)
Exit: grandparent(tom,ann)
answer 1: X=ann
Redo: grandparent(tom,ann)
  Redo: parent(bob,ann)
  Exit: parent(bob,pat)
Exit: grandparent(tom,pat)
answer 2: X=pat
Redo: grandparent(tom,pat)
  Redo: parent(bob,pat)
  Fail: parent(bob,X)
  Redo: parent(tom,bob)
  Fail: parent(tom,_)
Fail: grandparent(tom,X)
== 2 answers ==
```

## Runtime Stack: Push and Pop

In Activity 15, a procedure call pushes a new frame onto the runtime stack, while returning pops that frame. In the Prolog trace, lines 2, 3, 5 and 9 contain Call events that represent goals beginning execution and adding active frames. For example, line 2 calls grandparent(tom,X), while line 3 calls its subgoal, parent(tom,_). Successful Exit events show goals completing such as line 11 where grandparent(tom,ann) succeeds. Fail events, such as line 6 show goals that cannot succeed and whose execution is unwound. These events illustrate the push and pop operations discussed in Activity 15.

## Redo and Preserved Information

A Redo line means Prolog came back to a goal that already exited and asked it for another answer. A returned frame in Activity 15 was gone, so nothing remembered how to try again. A Prolog goal still holds a choice point and it also records which fact or clause was used, that others are untried, and which bindings were made since, so they can be undone. Line 7, Redo parent(tom,liz), shows this. Prolog undoes P=liz, and line 8 exits with parent(tom,bob). Line 13 does the same one level up. After answer 1, grandparent is redone, goes back into parent(bob,_) on line 14, and finds pat on line 15.

## Recursive Clause Ordering

The recursive clauses of reach/2 and ancestor/2 differ in what they consume before making a recursive call. In reach/2, the recursive call happens before checking edge(Z,Y). Because of this, Prolog can repeatedly call reach/2 without first following an edge. This can potentially cause infinite recursion when the graph contains a cycle. In contrast, ancestor/2 checks parent(X,Z) before recursively calling ancestor(Z,Y). Each recursive step follows an existing parent relationship moving through the finite family tree toward the base case. Therefore, swapping the clauses of reach/2 can affect whether Prolog finds an answer or gets stuck in recursion. Swapping the clauses of ancestor/2 changes the order in which answers are found but both clauses can still be explored.