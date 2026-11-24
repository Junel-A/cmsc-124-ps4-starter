<!--no-pdf-->
# Why Each Prediction Reads the Way It Does

Write one entry per case, before you run anything. Name the rule behind each
predicted field. One to three sentences is enough. The grader checks the
structure of this file only. It does not judge how good the reasoning is, and
it sets no word minimum.

For every case you predicted wrongly, add a short correction after you run
it: what you predicted, what the language printed, and which rule you
misread. Corrections belong in the correction commits, not the prediction
commit.

Commit this file together with `predictions.tsv`, before any case runs:

```bash
git add predictions.tsv REASONING.md
```

The example below uses a query that is not one of the 16 cases, so it gives
no answer away. `parent(pat, X)` reads `parent(pat, jim)` because the `parent`
facts are tried top to bottom and only that one has `pat` as its first
argument:

```text
## parent(pat, X)

The goal scans the parent facts in source order. Only parent(pat, jim) has
pat first, so X binds to jim. No second fact matches, so the first ; finds
nothing and the search ends after one answer.
```

## P01

The parent/2 facts are checked in the order they appear in the knowledge base. The first two facts with tom as the first argument are parent(tom, liz) and parent(tom, bob), so X is first assigned liz and then bob. No other facts match, therefore giving a total of two answers.

## P02

The query checks whether parent(tom, bob) exists in the knowledge base. Since the exact fact is already defined, Prolog immediately finds a match and returns true. There is only one matching fact so the count is 1.

## P03

The query searches for a parent whose child is tom. Looking through the parent/2 facts, none has tom as its second argument so no fact matches. The result is false with a count of 0.

## P04

Prolog first unifies X with f(Y, a). When Y is unified with b, that value is also applied to the term assigned to X. This gives X=f(b,a) and Y=b with only one possible unification.

## P05

Both terms have the same functor, f, and the same number of arguments. Prolog compares their corresponding arguments, matching X with a and Y with b. Since the corresponding arguments are compatible, unification succeeds once.

## P06

The query attempts to unify the atoms a and b. Since atoms are constants and these two have different values, Prolog cannot make them match. The result is false with a count of 0.

## P07

The grandparent/2 rule first looks for a parent and then checks whether that parent has children. Prolog initially considers Liz, but there are no facts showing that Liz has children. It then backtracks to Bob, whose children are Ann and Pat. It produces X=ann followed by X=pat for a total of two answers.

## P08

The ancestor/2 predicate places its base clause before its recursive clause. Prolog first finds direct descendants, Liz and Bob, and then explores their descendants through recursive calls. The resulting order is Liz, Bob, Ann, Pat, Sue and Jim giving six answers.

## P09

The ancestor_r/2 predicate uses the same base and recursive rules as ancestor/2, but the recursive clause comes first. Prolog explores deeper relationships before checking direct parent relationships, producing Sue, Jim, Ann, Pat, Liz and Bob. Although the order differs from P08, the predicate finds the same six descendants.

## P10

The ancestor/2 predicate checks its base clause first, followed by its recursive clause. When searching for the ancestors of Jim, Prolog first finds Pat through the base clause. It then applies the recursive clause by finding Tom through Bob and finally Bob through Pat. The answer order is Pat, Tom and Bob giving three answers.

## P11

The reach/2 predicate uses left recursion because it calls itself before checking an edge. The recursive call happens without first moving to another node in the graph. As a result, Prolog repeatedly makes the same recursive call without reaching the edge-checking clause. This causes unbounded recursion.

## P12

Unlike reach/2, the reach_r/2 predicate checks an edge before making a recursive call. This allows Prolog to follow the path from A to B, then C, and eventually D, producing a successful result. When backtracking for additional answers, it can follow the cycle from C back to A and eventually reach D again. It produces another true while continuing indefinitely.

## P13

The len/2 predicate counts the elements of a list using recursion. Its base clause handles an empty list by assigning a length of zero, while the recursive clause removes one element and adds one to the length of the remaining list. For [a,b,c], the resulting length is 3 with one answer.

## P14

The join/3 predicate combines two lists into one. When only the resulting list is known, Prolog first tries the base clause, where the first list is empty and the second contains the entire result. It then tries the recursive clause to generate the other possible splits: X=[1], Y=[2], followed by X=[1,2], Y=[]. This produces three answers in total.

## P15

Goals are evaluated from left to right, so the arithmetic comparison X > 2 is evaluated before elem/2. At this point, X has not been assigned a value because the predicate that generates list elements has not yet been called. Prolog cannot perform an arithmetic comparison with an unbound variable. This results in an instantiation error.

## P16

The elem/2 predicate generates the elements of the list from left to right. Each value is then checked against the condition X > 2. The first two elements, 1 and 2, do not satisfy the condition, but the last element, 3, does. Therefore, the only answer is X=3 with a count of 1.

