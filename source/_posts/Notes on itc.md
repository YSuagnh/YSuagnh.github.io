---
title: Notes on Introduction to the Theory of Computation
category: Notes
description: Some interesting theorem in the Theory of Computation
date: 2026-5-10
---

$\newcommand{\ol}{\overline}$
$\newcommand{\cp}{\mathrm{P}}$
$\newcommand{\cnp}{\mathrm{NP}}$
# Computability Theory

## Some Basic Definitions

### Turing Machine

$\rm{Definition}$
A $\rm{Turing\ Machine}$ is a 7-tuple, $(Q,\Sigma, \Gamma, \delta, q_0, q_{accept}, q_{reject})$, where $Q,\Sigma,\Gamma$ are finite set and  
1. $Q$ is the set of states,  
2. $\Sigma$ is the input alphabet not containing the blank symbol $\sqcup$,  
3. $\Gamma$ is the tape alphabet, where $\sqcup \in \Gamma, \Sigma \subseteq \Gamma$,  
4. $\delta:Q\times \Gamma \rightarrow Q \times \Gamma \times \{L,R\}$ is the transition function,  
5. $q_0 \in Q$ is the start state,  
6. $q_{accept} \in Q$ is the accept state,
7. $q_{reject} \in Q$ is the reject state, $q_{accept}\ne q_{reject}.$

---

<!-- more -->

A Turing Machine $M=(Q, \Sigma, \Gamma, \delta, q_0, q_{accept}, q_{reject})$ computes as follows. Initially, $M$ receives its input $w=w_1w_2...w_n\in\Sigma^{\*}$ on the leftmost n squares ofthe tape, and the rest of the tape is filled with blank symbol $\sqcup$. The head starts on the leftmost square of the tape. Note that $\Sigma$ does not contain the blank symbol, so the first blank appearing on the tape marks the end of the input. Once $M$ has started, the computation proceeds according to the rules described
by the transition function. If $M$ ever tries to move its head to the left off the left-hand end of the tape, the head stays in the same place for that move, even though the transition function indicates L. The computation continues until it enters either the accept or reject states, at which point it halts. If neither occurs, $M$ goes on forever.

### Configuration

$\rm{Definition}$
As a Turing Machine computes, a setting of current state, current tape contents and current head location is called a $\rm{configuration}$.

### Turing-recognizable(Recursively Enumerable, RE)

$\rm{Definition}$
Call a language Turing-recognizable if some Turing Machine recognizes it.

### Turing-decidable(Decidable, Recursive)

$\rm{Definition}$
Call a language Turing-decidable if some Turing Machine decides it.

---

*A Turing Machine is called a* $\rm{decider}$ *if it never loop and always make a decision to accept or reject.A decider that recognizes some language also is said to* $\rm{decide}$ *that language*

## Other computation models

### Multitape Turing Machine

A multitape Turing Machine is a Turing Machine wiht multiple tapes. Each tape has its own head for reading and writing. Initially the input appears on the first tape, and other tapes stay blank. The transition function $\delta : Q\times \Gamma^k\rightarrow Q \times \Gamma^k \times \{L,R,S\}^k$ allow for reading, writing, and moving the heads on some or all of the tapes in the same time.

$\rm{Theorem}$
Every multitape Turing machine has an equivalent single-tape Turing machine.

$\rm{Proof\ Idea}$
We show how to convert a multitape Turing Machine $M$ to a single-tape Turing Machine $S$. The key idea is to show that we can simulate $M$ with $S$ .  
Say that $M$ has $k$ tapes. To simulate $M$, $S$ needs to store the contents of each tape and keep track of the location of their head on a single tape. $S$ uses a new symbol # as a delimiter to separate the contents of different tapes, and write a dot above a tape symbol to mark the location where the head on that tape would be. We call the contents and dotted symbol stored on the single tape of $S$ virtual tapes and virtual heads. For example, the start configuration for $M$ would be stored as $\\# \dot{w_1}w_2...w_n \\# \dot{\sqcup} \\# ... \\# \dot{\sqcup} \\#$ on the single tape of $S$.  
To simulate a single move on $M$, $S$ scans the whole tape to read the symbol under each virtual heads. And make a second pass to update the virtual tapes according to $M$'s transition function.  
If $S$ moves a virtual head to the right onto a #, $S$ shifts the tape content, from this cell to the rightmost #, one unit to the right and writes blank symbol on this cell, and continues to simulate.

---

$\rm{Corollary}$
A language is Turing-recognizable if and only if some multitape Turing machine
recognizes it.

### Nondeterministic Turing Machine

A nondeterministic Turing Machine may proceed according to several possibilities at any point in a computation. The transition function for a Nondeterministic Turing Machine is defined as $\delta:Q\times\Gamma\rightarrow \cal{P}(Q\times\Gamma\times\{L,R\})$

---

$\rm{Theorem}$
Every nondeterministic Turing machine has an equivalent deterministic Turing machine.

$\rm{Proof\ Idea}$
We can simulate a nondeterministic TM $N$ with a deterministic TM $D$. The key is to perform brandth-first search on $N$'s nondeterministic computation tree.  
To simplify, we use a 3 tape deterministic TM $D$, which we have proved is equivalent to a single-tape TM. Tape 1 always contains the input string and is never altered. Tape 2 maintains a copy of $N$’s tape on some branch of its nondeterministic computation. Tape 3 keeps track of $D$’s location in $N$’s nondeterministic computation tree.  
There is a constant $b$ such that for every node in the tree can have at most $b$ children. For every child of a node, we can assign an element from $\Gamma_b = \{1,2,...,b\}$ to mark it. Thus we can assigne a address $231$ to the node we arrive by starting at the root, going to its 2nd child, going to that node’s 3rd child, and finally going to that node’s 1st child. Thus we can perform brandth-first search on the computation tree.

---

$\rm{Corollary}$
A language is Turing-recognizable if and only if some nondeterministic Turing machine recognizes it.

$\rm{Corollary}$
A language is decidable if and only if some nondeterministic Turing machine decides it.

### Enumerator

An Enumerator is a Turing Machine attached to a printer. Enumerator can use the printer to print strings.Ifthe enumerator doesn’t halt, it may print an infinite list of strings. The language enumerated by $E$ is the collection of all the strings that it eventually prints out.  
A language is recursively enumerable iff some enumerators enumerates it. Next we show that recursively enumerable is equivalent to Turing-recognizable.

$\rm{Theorem}$
A language is Turing-recognizable iff some enumerator enumerator it.

$\rm{Proof}$
First we show that if we have an enumerator $E$ that enumerates a language $A$, there is a TM $T$ performs as follows that recognizes $A$.

$T=$ On input $w$:
1. Run $E$ . Every time $E$ output a string, compare it to $w$,
2. If $w$ appears in the output of $E$, $accept$.

Thus, $T$ accept those strings appear on $E$'s list.

Now we show that if TM $T$ recognizes a language $A$, there is an enumerator performs as follows that enumerates $A$. Say that $s_1, s_2, ... $ is a list of all possible strings in $\Sigma^{\*}$.

$E=$ Ignore the input:
1. Repeat the following for $i=1,2,3,...$,
2. Run $T$ on each input for $i$ steps, $s_1, s_2, s_3,...,s_i$,
3. If any computations accept, print out the corresponding $s_i$.

If $M$ accepts a particular string $s$, eventually it will appear on the list generated by $E$ .

## Decidablity

Review the concept of decidable language. We call a language decidable if it's decided by a decider. A decider is a Turing Machine always halt and make a decision to accept or reject on any input.

$\rm{Theorem}$
Some languages are not Turing-recognizable.

$\rm{Proof\ Idea}$
Consider the set of all Turing Machine $\cal{T}$ and the set of all languages $\cal{L}$ over alphabet $\Sigma$. Let $\cal{B}$ be a set of all *infinite binary sequence*.We prove the theorem by showing that there is no onto function from $\cal{T}$ to $\cal{L}$ . To do so, we prove that there is a one-to-one function from $\cal{T}$ to $\Sigma^{\*}$ and from $\cal{B}$ to $\cal{L}$, and there is no onto function from $\Sigma^{\*}$ to $\cal{B}$ .

$\rm{Proof}$
There is a one-to-one function from $\cal{T}$ to $\Sigma^{\*}$ because every Turing Machine has a encoding to a string $\langle M \rangle$. Thus $f(M)=\langle M \rangle$ is such a function. Now we show that there is a one-to-one function from $\cal{L}$ to $\cal{B}$. Consider all strings in $\Sigma^{\*}$ in a specific order without repetition $s_1,s_2,s_3,...$ . We construct a function $g:\cal{B}\rightarrow\cal{L}$ .For a binary string $B=b_1b_2b_3...$, if $b_i$ is $1$, then $s_i$ appears in the language $g(B)$. If $b_i$ is $0$, then $s_i$ doesn't appears in $g(B)$.   
Now we use diagonalization method to prove that there is no onto function from $\Sigma^{\*}$ to $\cal{B}$. Assume there is a onto function $h:\Sigma^{\*}\rightarrow \cal{B}$, and $h(s_i)=B_i$ is a infinite binary sequence. For a infinite binary sequence $B$, we call the $i$ -th bit of the sequence $B[i]$. Considering a infinite binary sequence $NB=\overline{B_1[1]}\ \overline{B_2[2]}\ \overline{B_3[3]}...$ . We find that for any $i$, $h(s_i)=B_i\ne NB$ because the $i$ -th bit of them are different. Thus, such function don't exist and there is no onto function from $\cal{T}$ to $\cal{L}$, which means some languages are not recognized by any Turing Machine.

---

Now we give an example of a undicidable language.

Let 
$$
A_\sf{TM}=\{\langle M, w\rangle|M\ is\ a\ TM\ and\ M\ accepts\ w\}
$$

$\rm{Theorem}$
$A_\sf{TM}$ undicidable.

$\rm{Proof}$
We assume that $A_\sf{TM}$ is decadable and is decided by a TM $H$ .On input $\langle M,w \rangle$, Where $M$ is a TM and $w$ is a string, $H$ halts and accepts if $M$ accepts $w$, and it halts and rejects if $M$ rejects $w$.  
Now we construct a TM $D$ with $H$ as a subroutine. $D$ take a TM $M$ as a input and runs $M$ on its own description $\langle M\rangle$. $D$ is descriped as follows.

$D=$ On input $\langle M\rangle$, where $M$ is TM:
1. Run $H$ on input $\langle M, \langle M\rangle\rangle$.
2. Output the opposite of what $H$ outputs. That is, if $H$ accepts, *reject*; if $H$ rejects, *accepts*.

Thus we have
{% raw %}
$$D(\langle D\rangle)=\left\{ \begin{matrix} accept, \ \rm{if\ D\ rejects\ \langle D\rangle}  \\ reject, \ \rm{if\ D\ accepts\ \langle D\rangle} \end{matrix}\right.$$
{% endraw %}

No matter what D does, it is forced to do the opposite, which is obviously a contradiction. Thus neither TM $H$ nor $D$ can exist.

---

$\rm{Corollary}$
A language is decidable iff it is Turing-recognizable and co-Turing-recognizable.

In other words, a language is decidable exactly when both it and its complement are Turing-recognizable.

$\rm{Corollary}$
$\overline{A_\sf{TM}}$ is not Turing-recognizable.

Otherwise $A_\sf{TM}$ is decidable.

---

$\rm{Exercise}$
Let $A$ be a Turing-recognizable language consisting of descriptions of Turing machines, $\{\langle M_1\rangle,\langle M_2\rangle ,...\}$, where every $M_i$ is a decider. Prove that some decidable language is not decided by any decider $M_i$ whose description appears in $A$.

$\rm Proof$
We use diagonalization method to construct such language $D$. Consider all strings in $\Sigma^{\*}$ in a specific order $s_1,s_2,...$ without repetition. Let $E$ be a enumerator of $A$, if the $i$-th Turing machine that $E$ output accepts $s_i$, then $s_i$ is not in $D$, otherwise $s_i$ is in $D$. Clearly $D$ is not decided by any Turing machine in $A$. A TM $T$ described as follows decides $D$.

$T=$ On input $w$:
1. Find the $w$'s location in the sequence $s_1,s_2,...$, say $w=s_i$,
2. Run enumerator $E$ until it outputs the $i$-th Turing machine $M_i$,
3. run $M_i$ on input $w$, if it accepts, *reject*. If it rejects, *accept*.

## Reducibility

A $reduction$ is a way of converting one problem to another problem in such a way that a solution to the second problem can be used to solve the first problem.  
By reduction, we can prove the undicidibility of certain language in a much easier way. Here are some examples.

### Some examples

Let
{% raw %}
$$HALT_\sf{TM}=\{\langle M, w\rangle| \rm M\ is\ a\ TM\ and\ M\ halts\ on\ input\ \cal{w}\}$$
{% endraw %}

$\rm Theorem$
$HALT_\sf{TM}$ is undicidable.

$\rm Proof$
Assume $HALT_\sf{TM}$ is decidable and $H$ is its decider. Then we can construct a TM $T$ as follows that decides $A_\sf{TM}$

$T=$ On input $\langle M, w\rangle$, where $M$ is a TM and $w$ is a string:
1. Run $H$ on input $\langle M,w\rangle$,
2. If $H$ rejects, *reject*,
3. If $H$ accepts, simulate $M$ on input $w$,
4. If $M$ accepts, *accept*; if $M$ reject, *reject*.

Thus, if $H$ decides $HALT_\sf{TM}$, then $T$ decides $A_\sf{TM}$, which is a contradiction.

---

Let {% raw %}
$$E_\sf{TM}=\{\langle M\rangle| \rm M\ is\ a\ TM\ and\ L(M)= \emptyset\}$$
{% endraw %}

$\rm Theorem$
$E_\sf{TM}$ is undecidable.

$\rm Proof$
For any given TM $M$ and string $w$, we can construct a TM $M_1$ as follows.

$M_1=$ On input $x$:
1. If $x\ne w$, *reject*,
2. If $x = w$, run $M$ on input $w$ and *accept* if $M$ does.

Assume $E_\sf{TM}$ is decidable and $E$ is its decider, thus we can construct a TM $T$ as follows that decides $A_\sf{TM}$.

$T=$ On input $\langle M, w\rangle$, where $M$ is a TM and w is a string :
1. Use the description of $M$ and $w$ to construct the TM $M_1$ just described,
2. Run $E$ on input $M_1$.
3. If $E$ accepts, *reject*; if $E$ rejects, *accept*.

Thus, if $E$ decides $E_\sf{TM}$, then $A_\sf{TM}$ is decidable, which is a contradiction.

---

Let {% raw %}
$$REGULAR_\sf{TM}=\{\langle M\rangle|\rm M\ is\ a\ TM\ and\ L(M)\ is\ a\ regular\ language\}$$
{% endraw %}

$\rm Theorem$
$REGULAR_\sf{TM}$ is undecidable.

$\rm Proof\ Idea$
Assume $REGULAR_\sf{TM}$ is decidable and $R$ decides it. We construct a TM $M_2$ as follows using the description of TM $M$ and string $w$,  
$M_2=$ On input $x$:
1. If $x$ has the form $0^n1^n$, then *accept*.
2. If $x$ doesn't has this form, run $M$ on input $w$, *accept* if $M$ accept.  
Clearly if $M$ accept $w$, then $L(M_2)=\Sigma^{\*}$, otherwise $L(M_2)$ is a CFL. Thus if we run $R$ on $\langle M_2\rangle$, we can decide $A_\sf{TM}$, which is a contradiction.

---

Let {% raw %}
$$EQ_\sf{TM}=\{\langle M_1, M_2\rangle|\rm M_1,M_2\ are\ TM\ and\ L(M_1)=L(M_2)\}$$
{% endraw %}

$\rm Theorem$
$EQ_\sf{TM}$ is undecidable.

$\rm Proof\ Idea$
We can construct a TM $E$ that rejects all input. Assume $EQ_\sf{TM}$ is decidable and $EQ$ decides it. Thus if we run $EQ$ on input $\langle M, E\rangle$, then we can decide $E_\sf{TM}$, which is a contradiction.

### Computation history

$\rm Definition$
Let $M$ be a TM and $w$ be a input string. An $\rm accepting\ computation\ history$ for $M$ on $w$ is a sequence of configurations $c_1,c_2,...,c_l$, where $c_1$ is the start configuration and $c_l$ is an accepting configuration of $M$, and each $c_i$ legally follows from $c_{i-1}$ according to the rules of $M$. A $\rm rejecting\ computation\ history$ for $M$ on $w$ is defined similarly, except that $c_l$ is a rejecting configuration.

$\rm Definition$
A $\rm linear\ bounded\ automaton$ is a restricted type of Turing machine wherein the tape head isn’t permitted to move off the portion of the tape containing the input. If the machine tries to move its head off either end of the input, the head stays where it is—in the same way that the head will not move off the left-hand end of an ordinary Turing machine’s tape.

In a word, a linear bounded automaton is a Turing machine whose tape length is exactly the length of its input. Keep in mind that any strings in $\Sigma^{\*}$ can be a legal in put for a LBA. It's the tape that be restricted to the input, not the other way.

---

Let{% raw %}
$$A_\sf{LBA}=\{\langle M, w\rangle |\rm M\ is\ a\ LBA\ that\ accepts\ string\ \cal w\}$$
{% endraw %}

$\rm Theorem$
$A_\sf{LBA}$ is decidable.

$\rm Proof\ Idea$
Let $M$ be a LBA with $q$ states and $g$ symbols in the alphabet. It's clear that there are at most $qng^n$ distinct configurations of $M$ for a tape of length $n$. Thus we can simulate $M$ on input $w$ for at most $qng^n$ steps. If $M$ has not halt by then, it must be looping. Otherwise $M$ halts and decides to accept or reject.

---

Let{% raw %}
$$E_\sf{LBA}=\{\langle M\rangle|\rm M\ is\ a\ LBA\ and\ L(M)=\emptyset\}$$
{% endraw %}

$\rm Theorem$
$E_\sf{LBA}$ is undecidable.

$\rm Proof\ Idea$
Assume $E_\sf{LBA}$ is decidable and TM $E$ decides it. For an instance of $A_\sf{TM}$, $\langle M,w \rangle$, we can construct a $LBA\ B$ that accepts all accepting computation history of $M$ on $w$. If we run $E$ on input $\langle B\rangle$ , then we can decide whether $M$ accepts $w$, which makes $A_\sf{TM}$ decidable and is a contradiction.  
Now we give a description of $B$. Recall that an accepting computation history is a sequence of configurations $C_1,C_2,...,C_l$. Assume that the accepting computation history is presented as a single string with the configurations separated from each other by the # symbol. $B$ breaks up the input string into $C_1,C_2,...,C_l$, then determines whether each $C_i$ satisfies the three conditions of an accepting computation configurations.

### Mapping reduction

$\rm Definition$
A function $f:\Sigma^{\*}\rightarrow\Sigma^{\*}$ is a $\rm computable\ function$ if some Turing machine $M$, on every input $w$, halts with just $f(w)$ on its tape.

$\rm Definition$
Language $A$ is $\rm mapping\ reducible$ to language $B$, written $A\le_m B$, if there is a computable function $f:\Sigma^{\*}\rightarrow \Sigma^{\*}$ where for eevery $w$
{% raw %}
$$w\in A \Longleftrightarrow f(w)\in B$$
{% endraw %}
The function $f$ is called the $\rm reduction$ from $A$ to $B$.

$\rm Theorem$
If $A \le_m B$ and $B$ is decidable, then $A$ is decidable.

$\rm Proof$
Let $M$ be the decider for $B$ and $f$ be the reduction from $A$ to $B$. We describe a decider for $A$ as follows.

$N=$ On input $w$:
1. Compute $f(w)$.
2. Run $M$ on input $f(w)$, output whatever $M$ outputs.

Clearly, if $w\in A$, then $f(w)\in B$ because $f$ is a reduction form $A$ to $B$. Thus $M$ accepts $f(w)$ whenever $w\in A$.

$\rm Corollary$
If $A \le_m B$ and $A$ is undecidable, then $B$ is undecidable.

$\rm Theorem$
If $A \le_m B$ ans $B$ is Turing-recognizable, then $A$ is Turing-recognizable.

$\rm Proof$
Same as that of decidibility.

$\rm Corollary$
If $A \le_m B$ ans $A$ is not Turing-recognizable, then $B$ is not Turing-recognizable.

---

The definition of mapping reduction implies that $A \le_m B$ means exactly the same as $\ol{A}\le_m\ol{B}$. We already know that $\ol{A_{\sf{TM}}}$ is not Turing-recognizable. Thus if $A_{\sf{TM}}\le_m \ol{B}$, then $B$ is not Turing-recognizable.

$\rm Theorem$
$EQ_{\sf{TM}}$ is neither Turing-recognizable nor co-Turing-recognizable.

$\rm Proof$
First we show that $EQ_{\sf TM}$ is not Turing-recognizable. We do so by prove $A_{\sf TM} \le_m \ol{EQ_{\sf TM}}$. The reducing function $f$ works as follows.

$F=$ On input $\langle M,w\rangle$, where $M$ is a TM and $w$ is a string:
1. Construct two machines $M1, M2$ :  
    - $M_1=$ On any input:
        1. *Rejects*.
    - $M_2=$ On any input:
        1. Run $M$ on input $w$. If it accepts, *accept*. 
2. Outputs $\langle M_1, M_2\rangle$.

Clearly if $M$ accepts $w$, then $M_2$ accepts all strings. And so the two machines are not equivalent. If $M$ doesn't accept $w$, then $M_2$ accepts nothing, and is equivalent to $M_1$.  
Now we show that $EQ_{\sf TM}$ is not co-Turing-recognizable. We do so by prove $A_{\sf TM} \le_m EQ_{\sf TM}$ .The reducing function $g$ works as follows.

$G=$ On input $\langle M, w\rangle$, where $M$ is a TM and $w$ is a string:
1. Construct two machines $M_1, M_2$:
    - $M_1$ = On any input:
        1. *Accepts*.  
    - $M_2$ = On any input:
        1. Run $M$ on input $w$. If it accepts, *accept*.
2. Output $\langle M_1, M_2\rangle$

The only differenct between $f$ and $g$ is in machine $M_1$.

---

## Advanced Topics in Computability Theory

See [this](/2025/12/10/hello-world/).

# Complexity Theory

## Time Complexity

In this chapter, we analyze how much time an algorithm uses.

$\rm Definition$
Let $M$ be a deterministic Turing machine that halts on all inputs. The $\rm running\ time$ or $\rm time\ complexity$ is a function $f:\mathcal{N\rightarrow N}$, where $f(n)$ is the maximum number of steps that $M$ uses on any input of length $n$. If $f(n)$ is the running time of $M$,we say that $M$ runs in time $f(n)$ and that $M$ is an $f(n)$ time Turing machine. Customarily we use nto represent the length of the input.

### Some basic definition

$\rm Definition$
Let $f$ and $g$ be functions $f,g:\mathcal{N\rightarrow R^+}$. We say $f(n)=O(g(n))$ if positive integers $c$ and $n_0$ exist such that for every integer $n\ge n_0$,
{% raw %}
$$f(n) \le c\ g(n)$$
{% endraw %}
When $f(n)=O(g(n))$, we say that $g(n)$ is an $\rm upper\ bound$ for $f(n)$, or more precisely, that $g(n)$ is an $\rm asymptotic\ upper\ bound$ for
$f(n)$, to emphasize that we are suppressing constant factors. 

$\rm Definition$
Let $f$ and $g$ be functions $f,g:\mathcal{N\rightarrow R^+}$. We say $f(n) = o(g(n))$ if
{% raw %}
$$\lim_{x \to \infty} \frac{f(n)}{g(n)}=0$$
{% endraw %}
In other words, $f(n) = o(g(n))$ means that for any real number $c>0$, a number $n_0$ exists, where $f(n) < c\ g(n)$ for all $n\ge n_0$.

$\rm Definition$
Let $t:\mathcal{N\to R^+}$ be a function. Define the $\rm time\ complexity\ class$, $\rm TIME(\cal t(n))$ to be the collection of all languages that are decidable by an $O(t(n))$ time Turing machine.

### The class P

$\rm Definition$
$\rm P$ is the class of languages that are decidable in polynomial time on a single-tape deterministic Turing machine. In other words,
{% raw %}
$$\rm P=\bigcup_{\cal k} TIME(\cal n^k)$$
{% endraw %}

---

The class $\rm P$ plays a central role in our theory and is important because
1. $\rm P$ is invariant for all models of computation that are polynomially equivalent to the deterministic single-tape Turing machine, and
2. $\rm P$ roughly corresponds to the class of problems that are realistically solvable on a computer.

Now we give some examples in $\rm P$. Let
{% raw %}
$$PATH=\{\langle G, s, t\rangle|G\rm\ is\ a\ directed\ graph\ that\ has\ a\ directed\ path\ from\ \mathcal{s}\ to\ \mathcal{t}\}$$
{% endraw %}

$\rm Theorem$
$PATH \in \rm P$

$\rm Proof\ Idea$ We simply run brand-first search start from $s$ on the graph, and mark every node as we reached.

$\rm Theorem$
Every CFL is a member in $\rm P$.

$\rm Proof\ Idea$
Let $L$ be a CFL generated by CFG $G$ that is in Chomsky normla form. This is a classic problem using $dynamic\ programming$. Given a string $w$, and we need to check if $w\in L$. For all $1 \le l \le r \le |w|$ and every non-terminal symbol $v$ in $G$, we check if substring $s_{[l,r]}$ can be derived by $v$. We start with substrings of length $1$, and check longer substrings using the information of its own substrings. A harder version is in AtCoder [ABC261g](https://atcoder.jp/contests/abc261/tasks/abc261_g) .

### The class NP

$\rm Definition$
A $\rm verifier$ for a language $A$ is an algorithm $V$, where
{% raw %}
$$A=\{w|V \rm accepts\ \cal\langle w,c \rangle \rm for\ some\ string\ \cal c\}$$
{% endraw %}
We measure the time of a verifier only in terms of the length of $w$,so a $\rm polynomial\ time\ verifier$ runs in polynomial time in the length of $w$. A language $A$ is $\rm polynomially\ verifiable$ if it has a polynomial time verifier.

$\rm Definition$
$NP$ is the class of languages that have polynomial time verifiers.

$\rm Definition$
${\rm NTIME}(t(n)) = \{L|L\ {\rm is\ a\ language\ decided\ by\ an}\ O(t(n))\ {\rm time\ nondeterministic\ Turing\ machine}\}$

$\rm Theorem$
A language is in $\rm NP$ iff it is decided by some nondeterministic polynomial time Turing machine.

$\rm Proof$
First we prove the forward direction. Let $A \in \rm NP$ and show that $A$ is decidable by a polynomial time NTM $N$. Let $V$ be a polynomial time verifier for $A$. Assume that $V$ is a TM runs in time $n^k$, we construct $N$ as follows.

$N=$ on input $w$ of length $n$ :
1. Nondeterministic select string $c$ of length at most $n^k$.
2. Run $V$ on input $\langle w, c\rangle$.
3. If $V$ accepts, *accept*. Otherwise, *reject*.

Now we prove the other direction. Assume $A$ is decided by a polynomial time NTM $N$ and we construct a verifier $V$ as follows.

$V=$ On input $\langle w, c\rangle$ :
1. Simulate $N$ on input $w$, treating $c$ as a description of the nondeterministic choice to make at each step.
2. If this branch of $N$'s computation accepts, *accept*. Otherwise, *rejrct*.

$\rm Corollary$
$\rm NP = \bigcup_{\cal k}NTIME(\cal t(n^k))$

---

Now we give some examples in $\rm NP$. Let 
{% raw %}
$$CLIQUE = \{\langle G,k\rangle| G\ {\rm is\ a\ graph\ with\ a\ }k\ \rm clique\}$$
{% endraw %}

$\rm Theorem$
$CLIQUE$ is in NP.

$\rm Proof$
We give a verifier $V$ for $CLIQUE$.

$V=$ On input $\langle \langle G, k \rangle c\rangle$:
1. Test whether $c$ is a subgraph of $G$ with $k$ nodes.
2. Test whether $G$ contains all edges connecting all nodes in $c$.
3. If both pass, *accept*. Otherwise, *reject*.

Or we can give a NTM $N$ for $CLIQUE$.

$N=$ On input $\langle G, k \rangle$:
1. Nondeterministically select a subset $c$ of $G$ with $k$ nodes.
2. Test whether $G$ contains all edges connecting nodes in $c$.
3. If yes, *accept*. Otherwise *reject*.

---

Let 
{% raw %}
$$SUBSETSUM=\{\langle S, t\rangle|S = \{x_1,x_2,...,x_k\}\ {\rm and\ for\ some}$$
 $$\{y_1,...,y_l\}\in S, {\rm we\ have\ } \Sigma y_i=t\}$$
{% endraw %}

$\rm Theorem$
$SUBSETSUM$ is in $\rm NP$.

$\rm Proof$
We have a verifier $V$ for $SUBSETSUM$.

$V=$ On input $\langle \langle S, t\rangle, c \rangle$:
1. Test if $c$ is a collection of number that sum to $t$.
2. Test is $c$ is a subset of $S$.
3. If both pass, *accept*. Otherwise, *reject*.

---

We have present languages in $\rm NP$ but are not known to be in $\rm P$. The question of whether $\rm P = NP$ is one of the greatest problem in tcs and math. If these classes were equal, then any polynomial verifiable problem would be polynomial decidable. Most researchers believe that the two classes are not equal because people have invested enormous effort to find polynomial time algorithms for certain problems in $\rm NP$, without success. Researchers also have tried proving that the classes are unequal, but that would entail showing that no fast algorithm exists to replace brute-force search. Doing so is presently beyond scientific reach. 

### Polynomial time reducibility

$\rm Definition$
A function $f:\Sigma^{\*}\to \Sigma^{\*}$ is a $\rm polynomial\ time\ computable\ function$ if some polynomial time Turing machine exists that halts with just $f(w)$ on its tape, when started on any input $w$.

$\rm Definition$
Language $A$ is $\rm polynomial\ time\ reducible$ to language $B$, written $A\le_{\rm P}$, if a polynomial time computable function $f:\Sigma^{\*}\to\Sigma^{\*}$ exists, where every $w$,
{% raw %}
$$w\in A \Longleftrightarrow f(w)\in B$$
{% endraw %}
The function $f$ is called the $\rm polynomial\ time\ reduction$ of $A$ to $B$.

$\rm Theorem$
If $A\le_\cp B$ and $B\in \cp$, then $B \in \cp$.

$\rm Proof$
Let $M$ be a polynomial time algorithm deciding $B$ and $f$ be a polynomial time reduction from $A$ to $B$. We construct a polynomial time algorithm $N$ deciding $A$ as follows.

$N=$ On input $w$:
1. Compute $f(w)$.
2. Run $M$ on input $f(w)$ and output whatever $M$ outputs.

Clearly $N$ runs in polynomial time because both steps runs in polynomial time. $N$ decides $A$ because $w\in A$ whenever $f(w) \in B$ and $M$ decides $B$.

---

Now we give a example of such reduction. We need to introduce $SAT$ and $3SAT$. First we introduce $SAT$.  
A $\rm Boolean\ variable$ is a variable that can take on the value $\rm TRUE$ and $\rm FALSE$. Usually, we represent $\rm TRUE$ by $1$ and $\rm FALSE$ by $0$. Three $\rm Boolean\ operations$ $\rm AND$ $\rm OR$, $\rm NOT$, represented by symbols $\wedge, \vee, \neg$. A $\rm Boolean\ formula$ is an expression involving Boolean varibles and operations. For example,
{% raw %}
$$\phi = (x \vee y) \wedge (\ol x \vee z)$$
{% endraw %}
A Boolean formula is $\rm satisfiable$ if some assignment of $0\rm s$ and $1\rm s$ to the varibles makes the formula evaluate to $1$ . The $\rm satisfiable\ problem$ is to test whether a Boolean formula is satisfiable. Let
{% raw %}
$$SAT = \{\phi|\phi\rm\ is\ a\ satisfiable\ Boolean\ formula\}$$
{% endraw %}
Now we introduce $3SAT$.  
A $\rm literal$ is a Boolean varible or a negated Boolean varible, as in $x$ or $\ol x$. A $\rm clause$ is several literal connected with $\vee \rm s$, as in $(x\vee y\vee \ol z)$. A Boolean formula is in $\rm conjunctive\ normal\ form$, called a $cnf$-$formula$, if it comprises of several clauses connected with $\wedge \rm s$, as in 
{% raw %}
$$(x_1\vee x_2\vee \ol x_3)\wedge (x_2 \vee x_4)\wedge (x_2 \vee x_3\vee \ol x_5 \vee \ol x_6)$$
{% endraw %}
If all clauses have 3 literal, it is a $3cnf$-$formula$, as in
{% raw %}
$$(x_1 \vee x_2 \vee x_3) \wedge (\ol x_1 \vee x_3 \vee \ol x_5) \wedge (x_3 \vee \ol x_4 \vee x_5)$$
{% endraw %}
Let
$$3SAT = \{\phi| \phi\rm\ is\ a\ satsfiable\ 3cnf\ formula\}$$

$\rm Theorem$
$3SAT$ is polynomial time reducible to $CLIQUE$.

$\rm Proof$
Let $\phi$ be a fuomula with $k$ clauses such as
{% raw %}
$$\phi = (a_1\vee b_1\vee c_1)\wedge(a_2\vee b_2\vee c_2)\wedge ... \wedge(a_k\vee b_k\vee c_k)$$
{% endraw %}
We construct a graph $G$. The nodes in $G$ is organized as $k$ triples $t_1, t_2,...,t_k$. Each triple corresponds to one of the clauses in $\phi$, and each node in a triple corresponds to a literal in the associated clause. Lable each node with its corresponding literal in $\phi$. The edges in $G$ connect all node but two types of pairs of nodes. No edge between nodes in the same triple, and no edge between nodes with contradictory lables, as in $x_2$ and $\ol x_2$.  
Now we show that $\phi$ is satisfiable iff $G$ has a $k$-clique.  
First we show the forward direction. In a satisfiable assignment of $\phi$, at least one literal is true for each clause. If there are more than one literal is true in a clause, we pick one of them. Now we have exactly $k$ literals that are true in $\phi$. The nodes corresponding to these $k$ literals form a $k$-clique in $G$ because these $k$ nodes are in different triples and have no contradictory.  
Now we show the other direction. Suppose $G$ has a $k$-clique. No two of the clique's nodes occur in the same triples. We assign truth value to the variables of $\phi$ so that each literal labeling a clique node is made true. Doing so is always possible because two nodes labeled in a contradictory way are not connected by an edge.

### NP-completeness

$\rm Definition$
A language $A$ is $\cnp$-$\rm complete$ if it satisfies two conditions.
1. $A$ is in $\cp$.
2. Every language $B \in \cnp$ is polynomial time reducible to $A$.

$\rm Theorem$
If $A$ is $\cnp$-complete and $A$ is in $\cp$, then $\cp = \cnp$.

$\rm Proof$
This theorem follows directly from the definition.

$\rm Theorem$
If $A$ is $\cnp$-complete and $A\le_\cp B$ for $B$ in $\cnp$, then $B$ is $\cnp$-complete.

$\rm Proof$
We already know that $B$ is in $\cnp$, so we need to show that every language in $\cnp$ is polynomial time reducible to $B$. Every language in $\cnp$ is polynomial time reducible to $A$, and $A$ in turn is polynomial time reducible to $B$. Hence every language in $\cnp$ is polynomial time reducible to $B$.

### The Cook-Levin Theorem and some NP-complete problems

$\rm The\ Cook$-$\rm Levin\ Theorem$
$SAT$ is $\rm NP$-complete.

Check [this]().

## Space Complexity

In this chapter, we analyze how much space an algorithm uses.

$\rm Definition$
Let $M$ be a deterministic Turing machine that halts on all inputs. The $\rm space\ complexity$ of $M$ is the function $f: \cal N\to N$, where $f(n)$ is the maximum number of tape cells that $M$ scans on any input of length $n$. If the space complexity of $M$ is $f(n)$, we also say that $M$ runs in space $f(n)$.  
If $M$ is a nondeterministic Turing machine wherein all branches halt on all inputs, we define its space complexity $f(n)$ to be the maximum number of tape cells that $M$ scans on any branch of its computation for any input of length $n$.

$\rm Definition$
Let $f:\cal N\to R^+$ be a function. The $\rm space\ complexity\ class$, ${\rm SPACE}(f(n))$ and ${\rm NSPACE}(f(n))$ are defined as follows.
{% raw %}
$${\rm SPACE}(f(n))=\{L|L\ {\rm is\ a\ language\ decided\ by\ }O(f(n)) {\rm\ space\ determinstic\ TM}\}$$
$${\rm NSPACE}(f(n))=\{L|L\ {\rm is\ a\ language\ decided\ by\ }O(f(n)) {\rm\ space\ nondeterminstic\ TM}\}$$
{% endraw %}

$\rm Definition$
$\rm PSPACE$ is the class of language that are decidable in polynomial space on a deterministic Turing machine. In other words,
{% raw %}
$${\rm PSPACE}=\bigcup_k {\rm SPACE}(n^k)$$
{% endraw %}

---

We summarize our knowledge of the relationships among the complexity classes defined so far in the series of containments.
{% raw %}
$$\rm P \subseteq NP \subseteq PSPACE = NPSPACE \subseteq EXPTIME$$
{% endraw %}
We don’t know whether any of these containments is actually an equality.

### Savitch's Theorem

$\rm Savitch's\ Theorem$
For any function $f:\cal N\to R^+$, where $f(n) \ge n$,
{% raw %}
$${\rm SPACE}(f(n)) \subseteq {\rm NSPACE}(f^2(n))$$
{% endraw %}

$\rm Proof$
TODO

### PSPACE-completeness