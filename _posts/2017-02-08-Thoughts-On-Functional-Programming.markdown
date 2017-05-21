---
layout: post
title:  "Functional Programming and Complexity"
date:   2017-02-08 06:12:43 -0800
categories:
---
![Kowloon City](/images/Kowloon-Cross-section.png)

One of my key goals in programming is simplicity. I try to take an Occam's Razor style approach to solving any technical problem. From the outside looking in, functional programming appears to be taking the opposite approach. Higher order functions? Stateless functions? Higher kinded types? (I won't even dare to use any 'M' or 'F' words here). Point is, from the outside, it doesn't look very simple at all. Putting aside all of the commonly touted benefits of FP, why would we want to program functionally if we primarily aim for the simplest solution to any problem?

At first glance, procedural programming does seem much simpler than FP.
Just mutate state everywhere as needed and get it done. Easy, fast.
So, blog post over right? Well, The simplicity problem is not as straightforward as it first seems.
I'm going to stop talking about FP code specifically for a moment and just talk about any type of code.

I think there is an easy argument to make that the amount of time you spend writing and planning the codebase is typically inversely proportional to the amount of time it takes to understand and use the system later. That is, if you slap a bunch of spaghetti code out as fast as you possibly can, then it will take someone else much longer to grok the codebase. It will take this poor soul much longer to use the code and extend it do something else as well. This system is probably not very modular so small changes will most likely propagate throughout the entire system making the actual act of writing the changes, post understanding, that much more time consuming. Obviously, this initalTime:futureTime ratio doesn't always hold and this very quickly leads to diminishing (or actually harmful) returns (aka: 'over engineering').

If you admit that the previous ratio is true then simplicity becomes a tradeoff.
We are sacrificing current implementation simplicity for future implementation simplicity.
We are spending more time writing the code initially to reap future efficiency gains.

From this perspective, FP can be seen as striving for much simpler code in the long run. Most will not claim that writing clean, stateless functions or datastructures is simpler than
procedural equivalents. The very architecture of the Von Neumann computer is emulated by procedural programming. However, many will claim that dealing with such systems later on is much simpler. For example, With stateless functions you can simply substitute until an expression is simplified without having to worry about random exceptions or global state being touched incorrectly by another thread. With strong statically defined types, the compiler can help you in many ways. By making the initial implementation slightly more complex, the final product became simpler to use and reason about.

Also, as you get more used to writing and thinking functionally, many do claim that the initial implementations become simpler as well. But I'm trying to make the case for using FP now! Not after you took a crash course in Category Theory and have been using FP for 3-5 years. When writing procedurally, we make this exact same simplicity v. complexity tradeoff. In the procedural world it is usually solved with OOP or 'Design Patterns'. When used correctly many have seen the benefits of both approaches. FP is just another attempt to create simple code by leveraging higher level abstractions and concepts. So, I think you can still solve the problem simply by using Functional Programming, you just have to look at the big picture.
