---
layout: post
title: Lua in Dart
tags: [dart, lua, interpreter]
---
In the unlikely event that anybody needs a parser and rudimentary runtime system for the [Lua programming language](http://www.lua.org/), I wrote one for fun ;-)

I know, this is something that should really have tests, but I hacked this together mostly on a train with no internet connection and now use this as a cheap excuse to not have had the unittest package available...

I'm not sure whether I correctly remembered the semantic of Lua and whether this is on par with the latest Lua version. I wrote the parser ~6 years ago to play around with Scala (when it was new and shiny) and its pattern matching. I checked a few edge cases and it seems to be working ok.

My Lua parser requires semicolons and the scanner doesn't support long strings or even comments. So it's only a subset of Lua. On errors, both parser and interpreter aren't very helpful to find them. I haven't tried methods and other meta-stuff yet and I know, that the generic `for` statement isn't working. But if somebody thinks this is cool, I might create a real project and spend some more time on it.

---

{% gist 5494574 %}