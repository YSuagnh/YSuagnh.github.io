---
title: A linear time exact algorithm for graph simplification
category: algo
date: 2026-5-31
---

## Background

In [this paper](https://arxiv.org/pdf/2505.02493), the author showed that there are a lot of repeated patterns in the data flow graph, and gave an approximate algorithm to simplify the graph. The author claimed that exact algorithm would be intractable because the subgraph isomorphism problem is $\rm NP$-complete.  
However, the isomorphism problem for DAGs (directed acyclic graphs) is in $P$ while the subgraph isomorphism problem for DAGs is $\rm NP$-complete. The properties of DAGs and maximal rooted subgraphs make it possible to design a fast exact algorithm for graph simplification.

<!-- more -->

## Definition of the problem

$\rm Definition\ Rooted\ Subgraph$
Let $G = (V, E)$ be a directed acyclic graph. A subgraph $S \subseteq G$ is a $\rm rooted\ subgraph$ if there exists $v \in V (S)$ such that every vertex in $S$ is reachable from $v$. This $v$ is unique when $G$ is acyclic and is called the $\rm root$ of the subgraph. $S$ is $\rm maximal$ if it is the largest subgraph with root $v$.

The graph simplification problem requires us to merge all isomorphic maximal rooted subgraphs.

## The original approximate algorithm

$\rm Definition\ Backward\ Random\ Walk$
A $\rm backward\ random\ walk$ is a sequence of vertices
{% raw %}
$$P=v_1 v_2...v_k,v_i\in V(G)\ for\ 1\le i\le k$$
{% endraw %}
such that there exists an edge $e=v_{i+1}\to v_i$ for $1\le i\le k-1$ and $v_k$ has no incoming edge. The backward walk visits vertices in the opposite direction of the edges.

$\rm Lemma$
Let $G$ be a directed acyclic graph with no multi-edges. The probability that a vertex $v \in V (G)$ is visited in a random backward walk is
{% raw %}
$$P(v)=\frac{1}{|V(G)|}+\sum_{v_c\in C(v)}\frac{1}{|I_{v_c}|}P(v_c)$$
{% endraw %}
where $C(v)$ denotes the set of children vertices of $v$ in the directed graph and $|I(v_c)|$ denotes the number of incoming edges into $v_c$.

$\rm Theorem$
Let $H_1, H_2 \subseteq G$ be maximal isomorphic rooted subgraphs such that every pair of isomorphic vertices in $H_1$ and $H_2$ contains the same number of incoming edges in $G$. Then the roots $v_1 \in H_1$ and $v_2 \in H_2$ have the same probability of being visited in a random backward walk.

This theorem can be proved by induction. It is justifiable by this theorem that we can approximately merge all maximal isomorphic rooted subgraphs by combining all vertices with the same random backward walk probabilities.  
Though the approximate algorithm might be good enough, I'd still like to introduce my linear-time exact algorithm.

## Linear time exact algorithm for graph simplification

Inspired by the probabilistic method above, I propose a new method using a tag instead of a probability to determine whether two rooted subgraphs are isomorphic.

$\rm Definition\ Tag\ of\ vertices$
We define the $\rm Tag\ of\ vertices$ recursively. For $v\in V(G)$, the tag of $u$, written $tag(u)$, is $\emptyset$ if it has no child. Otherwise, {% raw %}$tag(u)=\{tag(v)|v\in C(u)\}$ {% endraw %}. Where $C(u)$ denotes the set of children vertices of $u$.

Take the graph below as an example.

![graph](/img/graph.png)

{% raw %}
Vertex $c$ has no children, $tag(c)=\emptyset$.  
Vertex $b$ has one child $c$, $tag(b)=\{\emptyset\}$.  
Vertex $a$ has one child $b$, $tag(a)=\{\{\emptyset\}\}$.  
Vertex $d$ has one child $b$, $tag(d)=\{\{\emptyset\}\}$.
{% endraw %}

$\rm Theorem$
Let $H_1,H_2 \in G$ be maximal rooted subgraphs, and $u_1\in H_1, u_2\in H_2$ be their roots. $H_1,H_2$ are isomorphic if and only if $tag(u_1)=tag(u_2)$.

This theorem is trivial and can be proved by induction. If we simply compute the tag of all vertices in $V(G)$ and compare each pair of vertices, it would run in $O(|V(G)|^4)$ time, which is not acceptable. Thus we label every tag with an integer. For each vertex, we do not record all tags from its children. Instead, we use an array to record all labels of its children's tags. This label array has the same effect as the vertex's tag. Then we can run a topological sort starting from vertices with no children, and record the label array for each vertex. We use a Trie to check whether an array has been labeled. If not, we give it a new index and insert it into the Trie. Finally, we put all vertices with the same label in a bucket.  
Take the graph below as an example.

![graph2](/img/graph-2.png)

First we find the vertex with no children, which is $d$, label $tag(d)$ with $1$, and put it in bucket $B_1$. Then we start the topological sort.  
The first vertex is $d$. We find that $a,b,c$ all have an edge to $d$, so the label of $tag(d)$ is added to their arrays. Vertex $b$ has no more children, so we check its array, which is $[1]$, and find the Trie is empty, so $tag(b)$ is labeled $2$. We put $b$ in bucket $B_2$ and insert $[1]$ into the Trie.  
Next the vertex is $b$. We find that $a,c$ all have an edge to $b$, so the label of $tag(b)$ is added to their arrays. Vertices $a,c$ have no more children, so we check their arrays. Both arrays are $[1,2]$, and are not found in the Trie. Thus $tag(a)$ and $tag( c )$ are labeled $3$. We put $a,c$ in bucket $B_3$ and insert $[1,2]$ into the Trie.  
We find that $a$ and $c$ have the same label, thus they have the same maximal rooted subgraph.

The complete algorithm runs as follows.

![algo1](/img/algo1.png)

The algorithm runs in $O(|V(G)+|V(E)|)$ time.

## Implement

[Here](/code/gs.zip) is a demo of the algorithm. Input is a graph, output are multiple sets containing vertices. All vertices in the same set are roots of isomorphic maximal rooted subgraphs. In the demo we didn't concern the depth of rooted subgraphs, which can also be computed in linear time.

The format of input is as follows.  
First line contains two numbers, $n$ and $m$. $n=|V(G)|$, $m=|E(G)|$. 
The following $m$ lines, each line contains two numbers $u$ and $v$, representing a edge in $E(G)$.  
We require the given graph to be a DAG and $0\le u,v < n$.