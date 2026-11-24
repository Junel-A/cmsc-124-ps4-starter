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
Facts are tried top to bottom; parent(tom, liz) is the first with tom first, so X=liz. The first ; backtracks to parent(tom, bob), so X=bob. No more facts, so count 2.
d.

## P02
No variables, so success reads true. It matches the fact parent(tom, bob) once, so no second answer and count 1.

## P03
No parent fact has tom as its second argument, so nothing unifies. The answer is false and the count is 0.

## P04
= is unification. X unifies with f(Y, a), then Y = b fills in the Y inside it, so X=f(b,a),Y=b, in order of first appearance. One way to unify, so count 1.

## P05
Unify argument by argument: X with a, b with b, Y with b, giving X=a,Y=b. Only one way, so count 1.

## P06
a and b are different atoms, so no binding can make them identical. The answer is false and the count is 0..

## P07
Body goals run left to right. P=liz first, but liz has no children, so it backtracks to P=bob, whose children are ann then pat. So X=ann, then X=pat, count 2.

## P08
The base clause is first, so direct children come first: liz, bob. Then the recursive clause goes through bob, giving ann, pat, then sue (via ann) and jim (via pat). Order liz, bob, ann, pat, sue, jim, count 6.

## P09
The recursive clause is first, so it goes down before reading a base fact. The deepest comes out first: sue (tom-bob-ann), then jim, then ann, pat, then liz, bob. Same six people as P08 in a different order, count 6.

## P10
The base clause gives X=pat first. The recursive clause then walks parent facts in order: tom (via bob, pat) before bob (via pat). So pat, tom, bob, count 3.

## P11
The first clause calls reach(a, Z) before reading any edge, which calls the same clause again. No fact is consulted, so nothing fails or answers. All three fields are unbounded.

## P12
The edge is read before recursing: a, b, c, then edge(c, d) gives true. Backtracking from c takes edge(c, a), round the cycle, and finds true again. It never runs out, so the second is true and the count is unbounded.

## P13
len has a clause for [] and one for [H|T], and only one matches. N is M + 1 adds one per element, so N=3, one answer, count 1.

## P14
With only the result known, join enumerates every split. Clause 1 gives X=[],Y=[1,2], clause 2 gives X=[1],Y=[2], then X=[1,2],Y=[]. Three splits, count 3.

## P15
Goals run left to right, so X > 2 is asked while X is unbound. > needs a number, so it throws an instantiation error. All three fields are error.

## P16
elem runs first and binds X to 1, 2, 3. X > 2 fails for 1 and 2 and backtracks into elem, then passes for 3. So X=3, no second answer, count 1.

