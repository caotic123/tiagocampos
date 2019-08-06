---
layout: post
title: 'How to Hindley Milner Type System and C++ made me think that sometimes we just need to "free out"'
comments: true
description: ""
keywords: "Type System, Inference of types, Haskell, Ocaml"
---

Let’s go talk about functional programming and type system to say yes to free out…. you don’t believe yet? Let’s drown…
When you need a function that print alls elements from a list you do something like that:

```
fun double (x : List<Int>) = do (print (concat x))
```

But if i need a list of doubles or string or any type that is printable. Yes you can duplicate each function and create a specialized type function. Of course, if your are C programmer you will love create many of function to do this or “free out” and use a void* type, however, we will assume that you is a lazy programmer and know about generics types. Let’s avoid concepts about types because it’s not my job here.

A lazy and proud programmer will to consider this solution below:

```haskell
concat :: [a] -> a
concat xs = ...
f :: forall a. [a] -> IO ()
f x = do (print (concat x))
```

You are most lazy programmer of world? Ok… so you will take off type notation right? No problems Hindley Milner inference will to do job for you… Bye notations…

```
concat xs = ...
f x = do (print (concat x))
```

Seems magic, but is just some few rules (if you have interest http://steshaw.org/hm/hindley-milner.pdf)
Hindley Milner system is a derivation of System F that is base of HM languagens like Ml and Haskell.
So… Every code i made in Sytem F system it’s possible use in HM system to do the job for me? Almost…., nothing is perfect everything is almost good or almost bad…
Let’s consider that

```
d f x = (f f) x
--- apply (d (\x-> x) Anything)
```

No problems? i think that maybe not :(, because it is impossible of to be encoding in HM type system. The type notation by Hm will to be ∀α b. (a -> a) -> b. Notice the quantification over types (a and b), sound wrong for you? Well, it is… a /= b right? so to do (f f) generates for me type a then apply ((f f) x) will give b, but wait… don’t exist any type whose (for every preposition where a and b are of same set). But why it’s happens? Simply… HM have only quantifications in outside of type notation so it’s means all of parametric polymorphism types belong only one quantification.
But how to solve this? Easy uses System F type system that allow quantification inside type notation and use inference rules…,

```haskell
k :: (forall a. a -> a) -> (forall a. a -> a)
k f x = (f f) x
-- but if maybe a == b so to do (f f) give me a and ((f f) x) will give me b and (for every prepostion of type a) == (for every prepostion of type b) so this is possible
```

Problem solved? Almost because this is Rank-N type funciton…., the big problem is that inference in System F is undecidable.
So how to writes this without type notations?

```c++
auto k = [=](auto f){return ([=](auto x) {return f(f)(x);});};
std::cout<< k(([=](auto x){return x;}))(2) << std::endl;
```

Yes… works because auto specifer is just a template deduction of a simply typed lambda function… No it’s don’t have a big type theory behind of this and i am not sure if is mathematically proven, but works!.
So therefore, i has been learn how to “free out” with Hindley Milner Type System and C++.
Don’t agree? Text me:
camposferreiratiago@gmail.com


 Text avaliable in : 
 https://medium.com/@tiagocampos_16501/why-c-and-type-system-just-me-to-think-that-sometimes-we-need-just-fuck-up-c39ea741f4a4
