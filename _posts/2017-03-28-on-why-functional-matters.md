---
title:  "On Why Functional Matters"
date:   2017-03-28
categories: Functional Haskell Clojure Elixir F#
---

## The Paper I Love

I recently presented a talk at the [Papers We Love](http://paperswelove.org/) - [Winnipeg Chapter](http://pwlwpg.ca/) meetup. The paper which was the subject of my presentation (and the subject of my affections) was [_Why Functional Programming Matters_](https://www.cs.kent.ac.uk/people/staff/dat/miranda/whyfp90.pdf), by John Hughes.
If that name sounds familiar, hopefully you are not thinking about the [prolific film writer and director](http://www.imdb.com/name/nm0000455/) from the 80's and 90's. The former is well known for such cult classics as "Ferris Bueller's Day Off"
and "The Breakfast Club". The latter is well known for his functional programming street cred.

[Professor John Hughes](http://www.cse.chalmers.se/~rjmh/), whose writing I love, is the progenitor and developer of such well known hits as _Haskell_ and [QuickCheck](http://www.cse.chalmers.se/~rjmh/QuickCheck/). His paper, also written in the 80's, emphasizes that modularity is the major
source of power that puts functional programming ahead of its procedural counterparts, most notably Object Oriented Programming languages. The modularity is exmplarized through use of two new "Functional Glues" that Hughes
defines as Higher-order functions and Lazy-evaluation.


## Excellent Programming Languages

[One of the slides](https://andrewsinclair.github.io/PWL-WhyFP/#/41) I presented, gave a terse sampling of the functional programming languages I consider worth your while as a modern day developer. I listed four languages, requiring to limit my selection for brevity.
   1. [_Haskell_](https://www.haskell.org/)
   2. [_Clojure_](https://clojure.org/)
   3. [_Elixir_](http://elixir-lang.org/)
   4. [_F#_](http://fsharp.org/)

All four of these languages share certain qualities and features and this correlation is not a coincidence. Afterall, John Hughes tells us at least two of the features we want are
Higher-order Functions and Lazy-Evaluation, which unsurprisingly each of these languages support wholeheartedly. It's also true that there are other excellent functional language features supported by all four of these choices.
For example, pattern matching, partial evaluation, immutability, and referential transparency all stand out in my mind.
There are also notable differences between these four languages. _Haskell_ and _F#_ will be the languages you choose if you want to get your hands dirty with [_category theory_](https://en.wikipedia.org/wiki/Category_theory) constructs such as [functors](https://wiki.haskell.org/Functor) and [monoids](https://wiki.haskell.org/Monoid). Conversely, _Clojure_ and _Elixir_'s type systems
are dynamic. _Elixir_ is an excellent choice for concurrency due to _Erlang_'s amazing [OTP](https://en.wikipedia.org/wiki/Open_Telecom_Platform) which it integrates with seamlessly. _Clojure_'s emphasis on "code-as-data" I find well suited for the functional paradigm.

After letting this slide sink in, I noticed a correlation that you may find interesting. The order of languages on the slide represents the priority I would recommend as being best to learn.
The surprising bit is the reverse order would be my recommended priority when deciding what languages to use in production.

Am I contradicting myself? Would I waste your time by telling you to learn one language but then turnaround and devalue it as non-practical?
Actually, everything is alright and this will all make perfect sense (bear with me!) I believe both points of view are valuable and complementary; not contradictory. Allow me to explain...


## The Explanation

What it boils down to is the following argument.
> The value of learning a language is not simply being able to use that language in production code.


Namely, the student learning a particular language can enjoy the following benefits:
```
	- understanding the core principles of the paradigm
    - knowing where the core ideas come from
```

In this list, Haskell is the de facto grandfather of the other languages. Therefore, if you want to learn the origins of functional programming, that is the place to start. Likewise, learning Haskell is learning the fundamentals of the paradigm because
the gap between language and principles is reduced. Finally, Haskell is a *pure* functional language, implying there aren't any non-functional features. Therefore, you won't lose focus by being distracted away from the functional oriented constructs.

The languages going down the list add incremental amounts of non-pure abstractions. We generally refer to these as "pragmatic" features. _F#_ is at the opposite end of the spectrum. It supports the full weight of OOP and FP, and runs on the .NET CLR.
It bears noting that, in my books, _Elixir_ and _Clojure_ are nearly tied in this respect. I really could have listed them the other way around and made the same arguments here.

## A Different Point Of View

Next up, let's look at the languages in the reverse order. I am now recommending the languages in decreasing order of. Here are the criteria I'm using to evaluate the relative ordering:
```
    - Community adoption
    - Recent growth
```

To be fair, _F#_, _Clojure_, and _Elixir_ all are actively being expanded within their resepective communities. _Haskell_ is a bit older and was established a long time ago but there just aren't that many jobs available locally in the _Haskell_ space. However, when it comes to community adoption, at least in the prairies, _F#_ is the most common choice for functional language. Many local businesses are seeing more and more adoption of _F#_. This is evidenced by the fact that our user groups and developer conferences have had more _F#_ talks in the last year than any functional language talks combined.


## Conclusion

I love functional languages and the four I've listed are all excellent. _Haskell_, _Clojure_, _Elixir_, and _F#_ are all excellent languages that are ready for you to learn and use in production (albeit with different considerations for each). If you want to go far, learn _Haskell_ first. If you want to go fast, learn _F#_ first. My favorite language is _Clojure_! Really, you can't go wrong if you choose one and stick with it.



