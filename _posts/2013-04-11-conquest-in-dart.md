---
layout: post
title: Conquest in Dart
tags: [dart, conquest, game]
---
Yesterday, just for fun, I started to convert an ancient strategy game called "Conquest" to Dart. I found it on Amiga Fish disk #24 a very long time ago. It is written in C, but in a style that looks like it was ported from something else (perhaps Pascal).

## Translating the Code

I copy and pasted all 3884 lines of code into a single file called `conquest.dart` and tried to edit it with DartEditor. That failed. The editor became completely unusable. So I started with an empty file, pasting only one of the C files at a time (some 50-400 lines) and fixed the obvious syntax errors. Again, DartEditor was completely unusable until I switched to the experimental analyzer. It stopped working when I reached ~3200 lines, though. I didn't bother to find out what made it stop and I didn't find any log file or stack trace. Once, it said something about an internal error.

After a few hours, I was down from 4000+ errors and countless warnings to zero errors and about 200 warnings, most warnings because the C source uses variables passed by reference to simulate multiple return values.

It would have been nice if Dart had support for multiple return values and/or parallel assignments. So, I had to work around this by using a simple `Ref<T>`class and some rewriting.
 
Interestingly, an expression like `foo(&bar)` is not a syntax error but only a warning because of an unknown empty variable named '' before the operator. The command line tool `dart_analyzer` flags these expressions (correctly, IMHO) as errors, though.

When DartEditor didn't show any more warnings, I switched to `dart_analyzer` which unfortunately found dozens of additional problems, most caused by C's feature, that `char` and `int` are exchangeable and stuff like `char c = 'A' + i;`. Why didn't DartEditor find those problems? And even `dart_analyzer` didn't find all occurrences, I think.

## In Need of Synchronous IO

After I hacked together a simple Dart native extension to switch my terminal to raw mode (something, the application needs), and after I created a simple `printf` implementation, I could actually run Conquest on the Dart VM and old memories from the golden age of Amiga returned :-) I had to fix a few null exceptions because I forgot to initialize arrays (which work differently in C) but now it seems to mostly work. That was easier than expected.

The source is ugly as hell. Is there any Dart pretty printer available? DartEditor seems to lack this feature and I tried IntelliJ IDEA + Dart plugin, but it completely destroys the source code beyond recognition. I noticed that dart_analyzer can dump the AST which might be the next best thing, but it seems to remove the compact "=>" function.

And the source needs heavy refactoring. As mentioned above, the C code is strange, already, with less than optional data types, a consequent ignorance of the fact that C indexes are zero-based and inconsistent names with numbers inserted at random points, probably to make names unique in the first 5 characters or so. It needs love, badly...

In the unlikely case, somebody's interested in the source code, here's my gist:

{% gist 5365220 %}
