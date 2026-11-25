# Analysis

We traced P07, grandparent(tom, X). Line numbers below count from the == trace P07 == header as line 1.

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

## Runtime Stack: Push and Pop

TODO Apong

## Redo and Preserved Information

A Redo line means Prolog came back to a goal that already exited and asked it for another answer. A returned frame in Activity 15 was gone, so nothing remembered how to try again. A Prolog goal still holds a choice point and itt also records which fact or clause was used, that others are untried, and which bindings were made since, so they can be undone. Line 7, Redo parent(tom,liz), shows this. Prolog undoes P=liz, and line 8 exits with parent(tom,bob). Line 13 does the same one level up. After answer 1, grandparent is redone, goes back into parent(bob,_) on line 14, and finds pat on line 15.

## Recursive Clause Ordering

TODO Apong