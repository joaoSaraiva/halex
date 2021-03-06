HaLeX
=====


  HaLeX: A Haskell Library to Model, 
                              Manipulate and 
                              Animate Regular Languages

        http://www.di.uminho.pt/~jas/Research/HaLeX

Copyright João Saraiva
          Department of Computer Science,
          University of Minho, 
          Braga, Portugal
          jas@di.uminho.pt


Version: 1.2.2 (August, 2016)


1- What is HaleX
----------------

HaLeX is a library of datatypes and functions implemented in Haskell
that allows us to model, manipulate and animate regular languages.

This library was developed in the context of a programming methodology
course for undergraduate students, and as a consequence, it was
defined mainly for educational purposes.


2- Features of the Library
--------------------------

The library provides the following features:

  - The definition of deterministic finite automata, non-deterministic
  finite automata, and regular expressions directly and
  straightforwardly in Haskell.

  - The definition of the acceptance functions for all those models.

  - The transformation from regular expressions into non-deterministic
  finite automata (NDFA) and from NDFA into deterministic finite
  automata (DFA).

  - The transformation from NDFA and DFA into regular expressions

  - The minimization of the number of states of deterministic finite
  automata.

  - The equivalence of regular expressions and finite automata.

  - The graphical representation of finite automata.

  - The definition of reactive finite automata.

  - The automatic animation of the acceptance function of finite automata.

       The animations are produced in an external tool: the
       high-quality graph visualization system GraphViz. Thus, to be
       able to visualize and animate regular languages, you have to
       install GraphViz tool.

       The GraphViz system is public domain and it is available at:

            http://www.research.att.com/sw/tools/graphviz/


3- The HaLeX Library
--------------------

   The library consists of the following modules:

   - RegExp.hs             -> Regular Expressions
   - Dfa.hs                -> Deterministic Finite Automata (DFA)
   - Ndfa.hs               -> Non Deterministic Finite Automata (NDFA)
   - RegExp2Fa.hs          -> Converts Regular expressions into Finite Automata
   - RegExpAsDiGraph.hs    -> Graphic Representation of Regular Expressions
   - FaAsDiGraph.hs        -> Graphic Representation of Finite Automata
                                (using GraphViz language/tools)
   - FaOperations.hs       -> Operations on Finite Automata
                                (ndfa2dfa , dfa2ndfa, unions , concats, etc)
   - FaClasses.hs          -> Type Classes to overload operations
   - Minimize.hs           -> Minimization of the number of states 
   - Equivalence.hs        -> Equivalence of Regular Expressions/Automata
   - ReactiveDfa.hs        -> Reactive Finite Automata
   - Dfa2MDfa.hs           -> Produces a Reactive Dfa from a DFA
   - RegExpParser.hs       -> Simple Parser for Concrete Regular Expressions
                                (Unix like notation)
   - Parser.hs             -> Basic Parser Combinators

   - Main.hs               -> The Main Module of halex Tool (see next section)
   - MainAnim.hs           -> The Main Fomule to run Animations

   - faAnin.lefty          -> A script written in lefty (one of GraphViz tools)
                              that animates the acceptance function


4- Using HaLeX: The halex Tool
------------------------------

The HaLeX library includes a useful tool to manipulate and vizualize
regular languages: the halex tool. This is a batch tool that can be
used in Unix pipes. It accepts as input a regular expression and it
produces Haskell or graphic representations (graphviz) based on finite
automata.

To install the halex tool, just compile the library modules using a
Haskell compiler (see file INSTALL).


4.1 The synopsis of halex is:
----------------------------

Usage: halex options [file] ...

List of options:
  -N, -n                --NDFA                       generate Non-Deterministic Finite Automaton
  -D, -d                --DFA                        generate Deterministic Finite Automaton
  -M, -m                --MinDfa                     generate Minimized Deterministic Finite Automaton
  -E, -e                --Dfa with Effects           generate Reactive Deterministic Finite Automaton
  -G, -g                --graph                      generate GraphViz input file
  -S, -s                --Sync State                 include a Synk State In the Graph Representation
  -R string, -r string  --regular expression=string  specify regular expression
  -o file               --output=file                specify output file
  -h, -?                --help                       output a brief help message


4.2 Running halex: some Examples
--------------------------------

  - Generating a Haskell-based NDFA

       halex -N -R"('+'|'-')?d*('.')?d+"


  - Producing the postscript of the graphic representation of a NDFA

       halex -N -G -R"('+'|'-')?d*('.')?d+" | dot -Tps


  - DFA with minimal number of states, visualized with dotty (one of
    GraphViz tools)

       halex -D -M -G -R"('+'|'-')?d*('.')?d+" | dotty -


  - Proving one law of the algebra of regular expressions

       halex -R"a*" -R"(a+)?"


4.2.1 Running one Animation
--------------------------

    - First, we have to configure the path in the makefile
    Make_Animation (subdirectory scripts) that produces the executable
    for the animation. Update the variable HaLeX_DIR with the location
    of the HaLeX library oin your machine.

    - The above makefile, uses the Haskell main module MainAnim.hs
    (subdirectory src) which calls the GraphViz tool lefty with the
    script that animates the finite automata (file faAnim.lefty in
    subdirectory scripts). Edit that module and update the path of
    lefty_tool constant function.


    - After that we are able to produce and run the animations. For
    example, we can produce the reactive finite automata as
    follows

          halex -E -M -R"('+'|'-')?d*('.')?d+"

      which generates the automaton (in this case, the minimized
      automaton, due to the use of the -M option) in the file
      GenMDfa.hs.

    - The Haskell module MainAnim is the main module to run the animations.
      It imports the previouly generated GenMDfa and it produces the
      animations.

      Its main function accepts as argument the sentence to be
      accepted/animated by the acceptance function and calls the lefty
      tool.

    - The lefty tool interprets the lefty script (faAnim.lefty), which
      produces the animations. Lefty provides the text view of the
      script. To start running the animation we have to call the
      functions provided in the lefty script:

            - fa.init()
            - fa.main()

      which initialize the lefty tool and the animation. Write these
      functions in the top frame followed by return. At this moment, a
      new window will be displayed that contains the graphic
      representation of the input. The right button of the mouse
      provides a set of operations to run the animation
      (forwards/backwards, adjusting the speed, tracking the path,
      etc)


6- Lecture/Exercise Notes
-------------------------

I have started developing the HaLeX library in 2000 in the context of
a third year course on programming methodology. This course has a
working load of 24 hours of theoretical classes and another 24 hours
of laboratory classes, running for 12 weeks (\ie, a semester). The
theoretical classes introduce the basic concepts of regular
expressions, finite automata and context-free languages. HaLeX is
used to support such classes. In the laboratory (a two hour class per
week) the students have to solve exercises using a computer. 

I have defined eleven exercise sets (one per week), using literate
Haskell, that the students have to complete. Each set of exercises
defines a module of the \HaLeX\ library. Thus, at the end of the
course the students have a complete documentation of all the exercises
and topics covered in the course, and, also, of the HaLeX library.

The Exercise Notes are (still in Portuguese...) avaliable at the HaLeX
homepage.


