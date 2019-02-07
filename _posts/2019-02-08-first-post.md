---
layout: post
title: 'First Post'
date: 2019-02-08 00:01:00 +0000
tags:
  math
  science
---

I'm currently most of the way through Michael Nielsen's YouTube lecture series
["Quantum Computing for the Determined"](https://www.youtube.com/playlist?list=PL1826E60FD05B44E4).
I highly recommend it to anyone who, like me, has heard a lot of handwavy pop-science
takes on "quantum computing" but has yet to learn what it actually *is*. (Heck, I own a
copy of [Scott Aaronson's _Quantum Computing Since Democritus_](https://amzn.to/2C3z8bw) that
I'm pretty sure I read, about five years ago, but clearly none of it stuck.)

Michael Nielsen's lectures spend a lot of time working through the arithmetic of
each example, which is a bit tedious (just like math class!) but I think it's a good
idea, because repetition reinforces the "muscle memory" of working with this stuff.

Here's my own pop-sci regurgitation of what I've learned. First of all, by "computing"
here we just mean "[quantum circuits](https://en.wikipedia.org/wiki/Quantum_circuit),"
which is basically a linear-algebra-heavy analogue of the logic gates you might remember
from somewhere-before-you-started-taking-CS-courses. So if there's a quantum-computing
analogue of C++ — or C — or machine code — it's very, very far above where we are right now.
(To have machine code, you must first have a machine! And it takes thousands of classical
logic gates to make something on the order of a Z80 or Intel 4004 CPU. Even more confusingly,
the actually measurable metric is [transistor count](https://en.wikipedia.org/wiki/Transistor_count);
sometimes it takes [many transistors to implement a single primitive gate](https://www.cs.bu.edu/~best/courses/modules/Transistors2Gates/), but on the other
hand, sometimes [just a few transistors](http://www.righto.com/2013/09/understanding-z-80-processor-one-gate.html)
can implement functionality that would take many more steps to express in terms of primitive gates.)

Classical logic gates can be strung together into "circuits." The bits of string in between
the gates are called "wires," and each "wire" conceptually carries either a $$\lvert 0\rangle$$ or a $$\lvert 1\rangle$$.
For example, if the two wires coming into an OR gate are carrying $$\lvert 0\rangle$$ and $$\lvert 1\rangle$$ respectively,
then the output wire will carry a $$\lvert 1\rangle$$. (We don't allow loops, by the way. Everything flows
one direction — from left to right.)

It's very tempting to slip into thinking of the labels $$\lvert 0\rangle$$ and $$\lvert 1\rangle$$ as two points on a number line...
like if you went just a little bit further along you'd come to $$\lvert 2\rangle$$, you know? But that's not how it works.
Not in classical and not in quantum either. The labels $$\lvert 0\rangle$$ and $$\lvert 1\rangle$$ are simply
arbitrary names, unrelated to the plain old number-line numbers $$0$$ and $$1$$.

So in quantum-circuit-world, we have the exact same deal; except that now instead of our
wires carrying precisely $$\lvert 0\rangle$$ or $$\lvert 1\rangle$$ (one *bit*),
we'll have each wire carry *a pair of complex numbers* (one *qubit*).
That is, where a classical wire carries

$$
    \alpha\lvert 0\rangle + \beta\lvert 1\rangle \quad (\alpha,\beta\in\mathbb{N};\; \lVert\alpha\rVert^2 + \lVert\beta\rVert^2 = 1)
$$

a quantum wire will carry

$$
    \alpha\lvert 0\rangle + \beta\lvert 1\rangle \quad (\alpha,\beta\in\mathbb{Q};\; \lVert\alpha\rVert^2 + \lVert\beta\rVert^2 = 1)
$$

This is commonly known as a "superposition" of $$\lvert 0\rangle$$ and $$\lvert 1\rangle$$, but as far as I can tell,
there's no sense in which the wire spookily carries "both values at once"; it merely carries a linear combination of
the two labels. This combination can be completely summed up in a little column vector:

$$
    A =
    \begin{pmatrix}
      \alpha \\
      \beta
    \end{pmatrix}
    =
    \begin{pmatrix}
      A_{\lvert 0\rangle} \\
      A_{\lvert 1\rangle}
    \end{pmatrix}
$$

This is where all the labels start feeling unnecessarily confusing to me, by the way. At the very "top" level,
we might say that a given wire happens to be carrying "$$\lvert 0\rangle$$." By that we mean that it's carrying
the linear combination $$\begin{psmallmatrix}
  1 \\
  0
\end{psmallmatrix}$$... which really means $$\begin{psmallmatrix}
  1+0i \\
  0+0i
\end{psmallmatrix}$$. Notice that a quantum wire might equally well be carrying $$i\lvert 0\rangle$$ a.k.a. $$\begin{psmallmatrix}
  0+1i \\
  0+0i
\end{psmallmatrix}$$ or even $$-\lvert 0\rangle$$ a.k.a. $$\begin{psmallmatrix}
  -1+0i \\
  \hfil 0+0i
\end{psmallmatrix}$$. These are all distinct and distinguishable values.

----

Quantum gates work similarly to classical gates, just in a more "continuous" fashion. So, there's a quantum version of
the NOT gate that transforms an input $$\begin{psmallmatrix}
  \alpha \\
  \beta
\end{psmallmatrix}$$ into the output $$\begin{psmallmatrix}
  \beta \\
  \alpha
\end{psmallmatrix}$$.

The next leap is that when you have multiple parallel wires, you generally can't assume their values are
independent. You can't say "wire $$A$$ has value $$\alpha\lvert 0\rangle + \beta\lvert 1\rangle$$
and wire $$B$$ has value $$\gamma\lvert 0\rangle + \delta\lvert 1\rangle$$"; or at least if you do, that's not necessarily
the whole story. You really have to say something like "The system-of-wires $$AB$$ has value
$$
\alpha\gamma\lvert 00\rangle + \alpha\delta\lvert 01\rangle + \beta\gamma\lvert 10\rangle + \beta\delta\lvert 11\rangle
$$," which combination can be summed up completely in a little column vector:

$$
    AB =
    \begin{pmatrix}
      AB_{\lvert 00\rangle} \\
      AB_{\lvert 01\rangle} \\
      AB_{\lvert 10\rangle} \\
      AB_{\lvert 11\rangle}
    \end{pmatrix}
$$

This column vector has $$2^n$$ entries. For three wires $$ABC$$, it would have eight entries. For four wires $$ABCD$$, it
would have 16 entries. Remember, this is what's needed just to fully describe the state that *is* being carried on these
wires — just a snapshot of a cut across the circuit diagram. The state of $$n$$ classical wires is summed up
in $$n$$ integer coefficients; the state of $$n$$ quantum wires is summed up in $$2^n$$ complex coefficients.

Or, to put it another way, the state of $$n$$ classical wires *can* be expressed as a $$2^n$$-entry column vector;
but because the coefficients must be integers, it follows that only one of the entries will be $$1$$ and all the rest
will be $$0$$. So we can compress that whole vector into basically a single number between zero and $$2^n-1$$ —
the index of that single non-zero entry.

----

Quantum logic gates with multiple input wires must have the same number of output wires,
and can always be expressed as matrix multiplications. There's a quantum version of the
XOR gate, called the ["controlled NOT" gate](https://en.wikipedia.org/wiki/Controlled_NOT_gate),
that transforms a pair of input wires

$$
    \begin{pmatrix}
      \alpha \\
      \beta \\
      \gamma \\
      \delta
    \end{pmatrix}
    \mathrm{into}
    \begin{pmatrix}
      \alpha \\
      \beta \\
      \delta \\
      \gamma
    \end{pmatrix}
$$

So for example say we have two initially independent wires $$A = 0.70\!\cdot\!\lvert 0\rangle + 0.71\!\cdot\!\lvert 1\rangle$$
and $$B = 0.99\!\cdot\!\lvert 0\rangle - 0.14\!\cdot\!\lvert 1\rangle$$. I'm using coefficients with no imaginary parts for
simplicity's sake, but I threw in a minus sign just to remind you that these are arbitrary complex numbers, not "probabilities"
or anything like that. So the state of our two-wire system can be summed up by the column vector

$$
    AB =
    \begin{pmatrix}
      AB_{\lvert 00\rangle} \\
      AB_{\lvert 01\rangle} \\
      AB_{\lvert 10\rangle} \\
      AB_{\lvert 11\rangle}
    \end{pmatrix}
    =
    \begin{pmatrix}
      \hfil 0.693 \\
           -0.098 \\
      \hfil 0.703 \\
           -0.099
    \end{pmatrix}
$$

And then when we feed those two wires $$AB$$ through our "controlled NOT" gate, the gate will exchange the lower two
values in the column vector, meaning that the values of our two *output* wires can be summed up by the column vector

$$
    AB^{\,\prime} =
    \begin{pmatrix}
      AB^{\,\prime}_{\lvert 00\rangle} \\
      AB^{\,\prime}_{\lvert 01\rangle} \\
      AB^{\,\prime}_{\lvert 10\rangle} \\
      AB^{\,\prime}_{\lvert 11\rangle}
    \end{pmatrix}
    =
    \begin{pmatrix}
      AB_{\lvert 00\rangle} \\
      AB_{\lvert 01\rangle} \\
      AB_{\lvert 11\rangle} \\
      AB_{\lvert 10\rangle}
    \end{pmatrix}
    =
    \begin{pmatrix}
      \hfil 0.693 \\
           -0.098 \\
           -0.099 \\
      \hfil 0.703
    \end{pmatrix}
$$

This state is a linear composition *mainly* of $$\lvert 00\rangle$$ and $$\lvert 11\rangle$$, with only
very-small-magnitude amounts of $$\lvert 01\rangle$$ and $$\lvert 10\rangle$$ thrown in. Our two wires'
values have become *entangled* in an interesting way.

This has been "Arthur tries to explain quantum circuits based on having watched a single YouTube video series."
If you're intrigued by this stuff, I highly recommend that you go watch
Michael Nielsen's ["Quantum Computing for the Determined."](https://www.youtube.com/playlist?list=PL1826E60FD05B44E4)
