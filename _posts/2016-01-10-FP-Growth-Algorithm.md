---
title: FP-Growth Algorithm
author: Flowerowl
layout: post
permalink: /4013.html
views:
  - 667
categories:
  - DM 
tags:
  - DM Algorithm
---
### Intro

The FP-Growth Algorithm is an alternative way to find frequent itemsets without using candidate generations, thus improving performance. For so much it uses a divide-and-conquer strategy. The core of this method is the usage of a special data structure named frequent-pattern tree (FP-tree), which retains the itemset association information.

In simple words, this algorithm works as follows: first it compresses the input database creating an FP-tree instance to represent frequent items. After this first step it divides the compressed database into a set of conditional databases, each one associated with one frequent pattern. Finally, each such database is mined separately. Using this strategy, the FP-Growth reduces the search costs looking for short patterns recursively and then concatenating them in the long frequent patterns, offering good selectivity.

### FP-Tree structure

The frequent-pattern tree (FP-tree) is a compact structure that stores quantitative information about frequent patterns in a database [4].

Han defines the FP-tree as the tree structure defined below:

1. One root labeled as “null” with a set of item-prefix subtrees as children, and a frequent-item-header table (presented in the left side of Figure 1);

2. Each node in the item-prefix subtree consists of three fields:

    * Item-name: registers which item is represented by the node;

    * Count: the number of transactions represented by the portion of the path reaching the node;

    * Node-link: links to the next node in the FP-tree carrying the same item-name, or null if there is none.

3. Each entry in the frequent-item-header table consists of two fields:

    * Item-name: as the same to the node;

    * Head of node-link: a pointer to the first node in the FP-tree carrying the item-name.

    * Additionally the frequent-item-header table can have the count support for an item.

### FP-tree construction

1. Input: A transaction database DB and a minimum support threshold.

2. Output: FP-tree, the frequent-pattern tree of DB.

3. Method: The FP-tree is constructed as follows.

    1. Scan the transaction database DB once. Collect F, the set of frequent items, and the support of each frequent item. Sort F in support-descending order as FList, the list of frequent items.

    2. Create the root of an FP-tree, T, and label it as “null”. For each transaction Trans in DB do the following:

        * Select the frequent items in Trans and sort them according to the order of FList. Let the sorted frequent-item list in Trans be [p \| P], where p is the first element and P is the remaining list. Call insert tree([ p \| P], T ).

        * The function insert tree([ p \| P], T ) is performed as follows. If T has a child N such that N.item-name = p.item-name, then increment N ’s count by 1; else create a new node N , with its count initialized to 1, its parent link linked to T , and its node-link linked to the nodes with the same item-name via the node-link structure. If P is nonempty, call insert tree(P, N ) recursively.

### FP-Growth Algorithm

After constructing the FP-Tree it’s possible to mine it to find the complete set of frequent patterns. To accomplish this job, Han presents a group of lemmas and properties, and thereafter describes the FP-Growth Algorithm as presented below.

1. Input: A database DB, represented by FP-tree constructed according to Algorithm 1, and a minimum support threshold ?.

2. Output: The complete set of frequent patterns.

3. Method: call FP-growth(FP-tree, null).

4. Procedure FP-growth(Tree, a) {
    (01) if Tree contains a single prefix path then // Mining single prefix-path FP-tree {

    (02) let P be the single prefix-path part of Tree;

    (03) let Q be the multipath part with the top branching node replaced by a null root;

    (04) for each combination (denoted as ß) of the nodes in the path P do

    (05) generate pattern ß ∪ a with support = minimum support of nodes in ß;

    (06) let freq pattern set(P) be the set of patterns so generated;

    }

    (07) else let Q be Tree;

    (08) for each item ai in Q do { // Mining multipath FP-tree

    (09) generate pattern ß = ai ∪ a with support = ai .support;

    (10) construct ß’s conditional pattern-base and then ß’s conditional FP-tree Tree ß;

    (11) if Tree ß ≠ Ø then

    (12) call FP-growth(Tree ß , ß);

    (13) let freq pattern set(Q) be the set of patterns so generated;

    }

    (14) return(freq pattern set(P) ∪ freq pattern set(Q) ∪ (freq pattern set(P) × freq pattern set(Q)))

    }

### Code

https://github.com/Flowerowl/FP-Growth

### Ref

https://en.wikibooks.org/wiki/Data_Mining_Algorithms_In_R/Frequent_Pattern_Mining/The_FP-Growth_Algorithm#FP-Tree_structure
