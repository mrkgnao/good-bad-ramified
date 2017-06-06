---
title: Constructing the field of p-adic numbers
author: Lewis Carroll
description: "This is a short summary of what I recently learned about \\(p\\)-adic numbers: elements of a very interesting kind of field that one can obtain from \\(\\Bbb Q\\), which have fascinating properties and are very useful in number theory. Throughout this piece, \\(p\\) is a prime."
tags: [number-theory]
date: 2016-12-23
---

## Meta: historical notes

On a previous edition of this blog, in another life[^adele], I wrote a post on \\(p\\)-adic numbers (in July 2015) and [posted it](https://www.reddit.com/r/math/comments/3c6f52/i_learned_a_little_about_padics_recently_and/) to /r/math on Reddit. (That blog has long ceased to exist.) In September, a Redditor apparently found that old post (I have no idea how), and messaged me about whether he could find the post anywhere. I said it'd be a "few weeks", but, well, whatever.

I found a copy on the Internet Archive and copied the text of it (since there was no way to recover the original Markdown, to the best of my knowledge) and, after a lot of regex-replacing in Spacemacs, managed to fix it up (as I promised in the [first post on this blog](/in-which-we-talk-a-lot/)) enough that I can post it again.

So ... the "recently" above isn't too recent!

While \\(p\\)-adics are still not something I'd say I'm very comfortable with, the fact that I can now construct them in multiple ways and can think of the constructions at higher levels of abstraction (e.g. "quotient field of a limit") has improved my ability to think about concrete examples. It's fascinating to go through this and see how my thinking has evolved in the short span of a bit over a year. For this reason, I've left it mostly unchanged, besides fixing a few typos here and there, and adding two "interludes" (on metric completions and fraction fields, respectively) that were missing in the original.

*End meta.*

## Representing a number as a sequence of remainders

Say you wish to find a solution to the equation \\(x ^ 2+1=0\\) in \\(\\Z\\). (Of course, there aren’t any, but just bear with me.)

It often happens that considering equations modulo some number (not necessarily a prime) tells us a lot about them. This happens because of the following fact: if an equation has integer solutions, these solutions will also be valid if we consider both sides mod any number, so if the equation has no solutions modulo a certain number \\(n\\) (i.e. in \\(\\Z/n\\Z\\)), it has no solutions in \\(\\Z\\).

For instance, the following is an easy exercise (hint: consider both sides mod \\(4\\)).

<div class="bd-callout bd-callout-info">
<h4>Exercise</h4>
<p>
 Show that \\(x ^ 2+y^2=435\\) has no solutions for integer \\(x\\) and \\(y\\).
</p>
</div>

Now, as we consider things modulo bigger and bigger numbers, we sort of "widen" the range of numbers we’re looking at. This inspires the following construction: instead of considering numbers modulo a fixed prime, we can look at their remainders mod every power of the prime. So, for instance, \\(25978\\) is

* \\(1 \\Mod 3\\)
* \\(4 \\Mod {3 ^ 2}\\)
* \\(4 \\Mod {3 ^ 3}\\)
* \\(58 \\Mod {3 ^ 4}\\)
* \\(220 \\Mod {3 ^ 5}\\)
* \\(463 \\Mod {3 ^ 6}\\)
* \\(1921 \\Mod {3 ^ 7}\\)
* \\(6295 \\Mod {3 ^ 8}\\)
* \\(6295 \\Mod {3 ^ 9}\\)
* \\(25978 \\Mod {3 ^ {10}}\\)
* \\(\\cdots\\)
* \\(25978 \\Mod {3 ^ {561}}\\) (you get the picture)

After \\(3 ^ {10}\\), the powers of \\(3\\) are greater than 25978, so the remainders are just \\(25978\\) itself, and the series "bottoms out". It is easy to see that this must be the case for every positive integer.

This suggests that we represent an integer \\(n\\) by a sequence of numbers

\\[(a _ 0,a _ 1,a _ 2,a _ 3,\\ldots)\\]

where \\(a _ k\\) is the remainder of \\(n\\) modulo \\(p ^ {k+1}\\). (Equivalently, \\(a _ {i+1} \\Mod {p^{i+1}}=a _ i\\).) For instance, \\(25978\\) can be represented by the infinite tuple \\((1,4,4,58,\\ldots)\\) and so on.

<div class="bd-callout bd-callout-info">
<h4>Exercise</h4>
<p>
 Check the "equivalently" bit.
</p>
</div>

(A little work shows that \\(2\\) is a solution to \\(x ^ 2+1=0\\) in \\(\\Z/5\\Z\\). Passing to \\(\\Z/25\\Z\\), we see that \\(7\\) is a solution in this ring. Continuing this, we can say that the infinite sequence \\((2,7,57,182,\\ldots)\\) is a "solution" to \\(x^2+1=0\\) "in the \\(5\\)-adics".)

## Introducing \\(\\Z _ p\\)

However, what if the remainders didn’t bottom out? What if there were something – obviously not an integer, as we’ve seen – that kept on giving different remainders modulo higher and higher powers of a prime? In much the same way as before, such an object can easily be represented as

\\[(a _ 0,a _ 1,a _ 2,a _ 3,\\ldots)\\]

that is, an infinite sequence of remainders, with the relation

\\[a _ {i+1} \\Mod {p ^ {i+1}}=a _ i.\\]

This kind of thing is called a \\(p\\)-adic integer. (Of course, technically, so are garden variety integers, but don’t nitpick.)

If you’re fancy, you can now define the ring[^lim] of \\(p\\)-adics as follows:

\\[\\Z _ p=\\{(a _ 0,a _ 1,a _ 2,\\ldots)∈\\prod\\Z/p ^ k\\Z:a _ {i+1} \\Mod {p^{i+1}} = a _ i\\} \\]

<div class="bd-callout bd-callout-info">
<h4>Exercise</h4>
<p>
 Check that termwise addition and multiplication make \\(\\Z _ p\\) into a (commutative) ring.
</p>
</div>

Next, we arrive at \\(\\Z _ p\\) from the analytical point of view.

## A few definitions

Quickly, a metric space is a set A together with a metric \\(d:A×A→R\\), such that:

* *positive definiteness*: \\(d(x,y)=0 \\iff x=y\\)
* *symmetry*: \\(d(x,y)=d(y,x)\\)
* the *triangle inequality*: \\(d(x,z)\\leq d(x,y)+d(y,z)\\)

all hold. The commonest examples include \\(\\Bbb R ^ n\\) (where we use the norm of the difference between two vectors) and rings like \\(\\Z\\), \\(\\Q\\), and \\(\\C\\) (where the metrics are the standard "absolute values" of the difference between two elements).

### An interesting example

Consider the space of functions

\\[\\mathscr C([0,1],\\R)=\\{f:[0,1]→\\R:f \\text{  is continuous}\\}\\]

where the metric is

\\[d(f,g)=\\sup _ {x\\in[0,1]}|f(x)−g(x)|\\]

<div class="bd-callout bd-callout-info">
<h4>Exercise</h4>
<p>
 Prove that \\(\\mathscr C([0,1],\\R)\\) is a metric space.
</p>
</div>

## The \\(p\\)-adic metric

Define

\\[p ^ n\\lVert x\\]

for \\(p\\) a prime if \\(p ^ n\\) is the highest power of \\(p\\) dividing \\(x\\).
We define the symbol

\\[\\nu _ p(x)=n\\]

where \\(p ^ n \\lVert x\\). This is called a \\(p\\)-adic valuation.

Now define \\(d _ p:\\Bbb{Z\\times Z\\to R}\\) by

\\[d _ p (x,y)=p ^ {−\\nu _ p(x−y)}\\]

which essentially says that two numbers are ‘close’ in some sense if their difference is divisible by a large power of \\(p\\).

<div class="bd-callout bd-callout-info">
<h4>Notational convention</h4>
<p>
 We write \\(\\|x,y\\| _ p\\) instead of \\(d _ p(x,y)\\).
</p>
</div>

It is an interesting fact that this distance function, called the \\(p\\)-adic metric, is a valid metric on \\(\\Z\\).

<div class="bd-callout bd-callout-info">
<h4>Exercise</h4>
<p>
 Prove that \\(\\|\cdot,\\cdot\\| _ p\\) is a metric.
</p>
</div>

Like every metric, the \\(p\\)-adic metric confers a topology on its underlying set – in this case, \\(\\Z\\). 

### Interlude: Completions

For every metric on a space \\(S\\), there is a notion of "completeness". A space can either be complete, or incomplete, with respect to a given metric.

Consider a sequence \\(a_i \\in S\\) where successive elements come arbitrarily close together, i.e. for any \\(\\epsilon\\), there is some \\(M\\) such that

\\[ i,j>M \\implies |a _ i - a _ j| < \\epsilon \\]

This kind of sequence is called a [*Cauchy* sequence](https://en.wikipedia.org/wiki/Cauchy_sequence).

The question is, do all Cauchy sequences in \\(S\\) *converge*? That is, does there exist some \\(L\\) such that the \\(a _ i\\) come arbitrarily close to \\(L\\)? 

In the language familiar from calculus or analysis, does every Cauchy sequence in \\(S\\) have a limit?  
Well, not necessarily: the standard example is \\(\\Q\\), with the sequence

\\\[1,1.4,1.41,1.414,1.4142,\\ldots\\\]

which does not converge since (modulo some analytical sophistication) \\(\\sqrt 2\\notin\\Q\\).

We say that \\(\\Q\\) is **incomplete** with respect to the standard metric. 

On the other hand, \\(\\R\\) is complete: in fact, the reason why \\(\\R\\) is so important
in mathematics is largely that it is the *completion* of \\(\\Q\\). 

You may be able to guess what that means: there is a procedure, called **(metric) completion**, which starts with a possibly-incomplete metric space
and produces a complete one. This satisfies some "obvious rules": for instance, completing an already-complete space does nothing (up to unique isomorphism, as always).
The construction can be summarized as follows: take the set of all Cauchy sequences in \\(S\\), denoted \\({\\rm Cau}(S)\\). Define a metric on this space (yes!) as follows:

\\[d(a, b) = \\lim _ {n\\to\\infty} d(a _ n, b _ n)\\]

This is, as you can check, a Cauchy sequence of real numbers, and hence the limit is well-defined. We can now define an equivalence relation on \\({\\rm {Cau}}(S)\\) by saying

\\[a\\sim b \\; \\text{ if } \\; d(a,b) = 0\\]

(which we think of as sequences that are "eventually the same") and define \\(S'\\), the completion of \\(S\\), as the set of equivalence classes. 

The completion of \\(\\Z\\) with respect to this metric is – you guessed it – \\(\\Z _ p\\)! 

## Constructing \\(\\Q _ p\\) from \\(\\Z _ p\\)

Now we can construct the *fraction field* \\({\\rm {Frac }}\\Z _ p\\).

### Interlude: Fraction fields[^ffwiki]

An integral domain is a ring without *zerodivisors* -- that is, rings where \\(ab = 0\\) implies that at least one of \\(a\\) or \\(b\\) is zero.

Given an integral domain \\(R\\), take the set \\(R'\\) of "formal fractions" \\(\\frac ab\\) where \\(a,b\\in\\Bbb R\\) and \\(b\\neq 0\\). We define two such fractions 
\\(a/b\\) and \\(c/d\\) to be equivalent if 

\\[ ad = bc \\]

(which is basically the idea that \\(2/3 = 4/6 \\in\\Q\\)). The set of equivalence classes is called \\({\\rm {Frac}}(R)\\), the *fraction field* of \\(R\\).

For example, \\(\\Q\\) is the fraction field of \\(\\Z\\).

We have the result

\\[{\\rm Frac}(\\Z _ p) = \\Bbb Q _ p,\\]

the field of \\(p\\)-adic rationals. These are the other completions of \\(\\Q\\) (besides \\(\\R\\), which we've already met).

## \\(\\Z _ p\\) is compact

(Aka "Fascinating fact I can’t prove yet #443" in the original.)

<div class="bd-callout bd-callout-info">
<h4>Miracle</h4>
<p>
 It turns out that \\(\\Z _ p\\) can be given a topology. 
</p>
</div>

Since 

\\[\\Z _ p\\subset\\prod\\Z/p ^ k\\Z,\\]

you can give the \\(\\Z/p ^ k\\Z\\)s the discrete topology, give the product the product topology, and finally give \\()\\Z _ p\\) the subspace topology. Apparently, this space is compact, by something called [Tychonoff’s theorem](https://en.wikipedia.org/wiki/Tychonoff's_theorem).

## Conclusion

That’s mostly it. I’ll leave it to you to ruminate over these things. I don’t know nearly enough to be able to say anything intelligent[^zp] about \\(\\Z _ p\\) yet, so I’m just writing down whatever I know to ensure that I know that much correctly. Please tell me if I’ve made any mistakes.

This stuff is fascinating – apparently it’s useful in advanced number theory. I can’t wait to learn more!

---

That concludes the original post.

## Acknowledgements

Thanks to the awesome Balarka Sen and @anon over at Math.SE chat for teaching me this stuff. I knew very little ring theory at the time, and generally had much more trouble following mathematical arguments than I do now.

Thanks to [/u/chebushka](rcc) for finding multiple errors in this post, and [/u/samloveshummus](rc1), [/u/aristotle2600](rc2), and [/u/Torpluss](rc3) for catching typos in the old version.

[rc1]: https://www.reddit.com/r/math/comments/3c6f52/i_learned_a_little_about_padics_recently_and/csspc43/
[rc2]: https://www.reddit.com/r/math/comments/3c6f52/i_learned_a_little_about_padics_recently_and/cssqvhm/
[rc3]: https://www.reddit.com/r/math/comments/3c6f52/i_learned_a_little_about_padics_recently_and/cssqw1i/
[rcc]: https://www.reddit.com/r/math/comments/5jw4ws/constructing_the_field_of_padic_numbers/dbji5ia/


[^zp]: This is still true, for a slightly different value of "intelligent".
[^adele]: Adeles were far-off mysteries to me then; things I hadn't even heard of.
[^ffwiki]: The [Wikipedia page](https://en.wikipedia.org/wiki/Field_of_fractions#Construction) treats this well.
[^gen]: The \\(s\\) and \\(t\\) ensure that everything works out even if \\(R\\) contains zerodivisors.
[^lim]: This is [sneakily](https://en.wikipedia.org/wiki/P-adic_number#Algebraic_approach) an "inverse limit", [whatever that means](https://en.wikipedia.org/wiki/Inverse_limit).