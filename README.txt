Bayesian Logic (BLOG) inference engine version 0.3

Copyright (c) 2007, 2008, Massachusetts Institute of Technology
Copyright (c) 2005, 2006, Regents of the University of California
All rights reserved.  This software is distributed under the license 
included in LICENSE.txt.

Lead author: Brian Milch, bmilch@gmail.com
Supervisors: Prof. Stuart Russell (Berkeley), Prof. Leslie Kaelbling (MIT)
Contributors: Martí Bolivar, Brendan Clark, Rodrigo de Salvo Braz, 
    Michael Haimes, Keith Henderson, Kristian Kersting, Andrey Kolobov, 
    Bhaskara Marthi, Daniel L. Ong, David Sontag, Luke S. Zettlemoyer


Online Information
------------------
For documentation and the latest version of the BLOG inference engine,
please see:

  http://people.csail.mit.edu/milch/blog


License
-------
The BLOG inference engine is distributed under the BSD license, as 
given in LICENSE.txt.  

This distribution also includes several third-party packages, which are 
distributed under their own licenses:
 * a modified version of the CUP v0.10k parser generator
 * a modified version of the JLex 1.2.6 lexical analyzer generator
 * the JAMA 1.0.1 matrix package
 * the JUnit 4.5 unit testing framework (source code available 
   from junit.org)


Installation
------------
The following installation instructions assume that you're using a
machine with the Java SDK (version 1.5 or newer), the "make" utility,
and the ability to run a shell script.

Decompressing the ZIP archive that you downloaded will yield a
directory called "blog-0.3".  You can put this directory wherever you
like.  To compile the source code, go into this directory and type
"make".  If "make" finishes with no errors, the inference engine is 
ready to run.

Please see CHANGELOG.txt for changes since the previous version.  


Getting Started
---------------
To get you started with BLOG, we'll show you how to reproduce the
experiment from our IJCAI-05 paper.  The top-level BLOG directory
contains a shell script called "runblog", which invokes java with the
proper classpath.  There is also a subdirectory called "examples" that
contains several example BLOG models.  To run inference on the
urn-and-balls scenario from our paper, give the command:

./runblog examples/balls/poisson-prior-noisy.mblog 
    examples/balls/all-same.eblog examples/balls/num-balls.qblog

The program will do inference and print out the posterior distribution
over the number of balls in the urn, given 10 draws that all appear to
be the same color.  By default, the program does 10000 samples of
likelihood weighting.  You can compare its output to the correct
posterior distribution, which is given in a comment at the end of
examples/balls/poisson-prior-noisy.mblog.

To find out how to do more with the BLOG inference engine, please see
the user manual at:

  http://people.csail.mit.edu/milch/blog/manual


What Can It Do?
---------------
The BLOG inference engine can parse any model written in the BLOG
language that we introduced in our SRL-04 and IJCAI-05 papers.  It
includes a full set of built-in types (integers, strings, real
numbers, vectors, matrices) and can use arbitrary conditional
probability distributions (CPDs) in the form of Java classes that
implement a certain interface.  Models can include arbitrary
first-order formulas.

As noted in our papers, some BLOG models do not actually define unique
probability distributions, because they contain cycles or infinite
receding chains.  The current version of the inference engine does not
make any effort to detect whether a model is well-defined or not.  On
some ill-defined models, the inference algorithms will end up in
infinite loops.

This version of the inference engine includes three general-purpose
inference algorithms: rejection sampling (as in our IJCAI-05 paper),
likelihood weighting (as in our AISTATS-05 paper), and a
Metropolis-Hastings algorithm where the proposal distribution just
samples values for variables given their parents.  These algorithms
are very slow, but they can still yield interesting results on toy
problems.  The inference engine also allows modelers to plug in their
own Metropolis-Hastings proposal distributions: the proposal
distribution can propose arbitrary changes to the current world, and
the engine will compute the acceptance probability.  We include a
hand-crafted split-merge proposal distribution for the urn-and-balls
scenario as an example.

The inference engine also includes two exact inference algorithms that
work on BLOG models with known objects (that is, with no number
statements).  One of these is the variable elimination algorithm; the
other is first-order variable elimination with counting formulas
(C-FOVE) described at AAAI 2008.

We plan to include parameter estimation capabilities -- specifically 
Monte Carlo EM -- in a future version of the engine.  However, the current
version has no learning code.


How to Help
-----------
The main reason we're releasing the BLOG inference engine code is so
that other people can use it, evaluate the strengths and weaknesses of
the BLOG language, and develop new inference and learning algorithms.
It would also be great to have help improving the inference engine's
interface and building utilities to work with it.  If you have
feedback, bug reports, ideas for improvement, or new code, please send
email to Brian Milch at bmilch@gmail.com.
