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

The parent/2 facts are checked in the order they appear in the knowledge base. The first two facts with tom as the first argument are parent(tom, liz) and parent(tom, bob), so X is first assigned liz and then bob.

## P02

The query checks whether parent(tom, bob) exists in the knowledge base. Since the exact fact is already defined, Prolog immediately finds a match without needing to check any other possibilities.

## P03

The query searches for a parent whose child is tom. Looking through the parent/2 facts, none has tom as its second argument, so there is no matching fact.

## P04

Prolog first matches X with f(Y, a). When Y is unified with b, that value is also applied to the term assigned to X. This gives X = f(b, a) and Y = b.

## P05

Both terms have the same functor, f, and the same number of arguments. Prolog compares their corresponding arguments, matching X with a and Y with b. This is possible because the corresponding arguments are compatible.

## P06

The query attempts to unify the atoms a and b. Since atoms are constants and these two have different values, Prolog cannot make them match.

## P07

The grandparent/2 rule first looks for a parent and then checks whether that parent has children. Starting with Tom, Prolog finds Bob as his child and then checks Bob's children, which are Ann and Pat. It does not consider Liz because there are no facts showing that Liz has children.

## P08

The ancestor/2 predicate places its base clause before its recursive clause. Prolog first looks for direct children and then explores their descendants through recursive calls. Because the base clause is always checked first, direct descendants are encountered before more distant ones.

## P09

The ancestor_r/2 predicate uses the same rules as ancestor/2, but the recursive clause comes first. Prolog explores deeper relationships before checking the direct parent relationship. This changes the order in which the descendants are found, even though both predicates can find the same descendants.

## P10

The ancestor/2 predicate checks its base clause first, followed by its recursive clause. When searching for the ancestors of Jim, Prolog first checks his direct parent, Pat, and then follows the parent relationships to Bob and Tom.

## P11

The reach/2 predicate uses left recursion because it calls itself before checking an edge. The recursive call happens without first moving to another node in the graph. As a result, Prolog keeps repeating the same recursive call instead of reaching the base clause.

## P12

Unlike reach/2, the reach_r/2 predicate checks an edge before making a recursive call. This allows Prolog to move from one node to another, following the path from a to b, then c, and eventually d. However, the graph contains a cycle (a to b to c to a), so Prolog can continue searching indefinitely when it backtracks for more answers.

## P13

The len/2 predicate counts the elements of a list using recursion. Its base clause handles an empty list by assigning a length of zero, while the recursive clause removes one element and adds one to the length of the remaining list. For [a,b,c], the count is three.

## P14

The join/3 predicate combines two lists into one. Prolog first tries the base clause where the first list is empty and the second list contains the entire result. It then tries the recursive clause that takes the elements from the beginning of the result and placing them into the first list. This allows [1,2] to be divided in different ways.

## P15

The arithmetic comparison X > 2 is evaluated before elem/2. At this point, X has not been assigned a value because the predicate that generates list elements has not been called yet. Prolog cannot perform an arithmetic comparison with an unbound variable, resulting in an instantiation error.

## P16

The elem/2 predicate generates the elements of the list from left to right. Each value is then checked against the condition X > 2. The first two elements, 1 and 2, do not satisfy the condition, but the last element, 3, does.

