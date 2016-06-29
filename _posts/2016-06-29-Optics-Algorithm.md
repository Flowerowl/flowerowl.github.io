---
title: Optics Algorithm
author: Flowerowl
layout: post
permalink: /4022.html
views:
  - 667
categories:
  - DM
tags:
  - DM
---

## Intro

Ordering points to identify the clustering structure (OPTICS) is an algorithm for finding density-based clusters in spatial data. 

It was presented by Mihael Ankerst, Markus M. Breunig, Hans-Peter Kriegel and Jörg Sander. 

Its basic idea is similar to DBSCAN, but it addresses one of DBSCAN's major weaknesses: the problem of detecting meaningful clusters in data of varying density. 

In order to do so, the points of the database are (linearly) ordered such that points which are spatially closest become neighbors in the ordering. 

Additionally, a special distance is stored for each point that represents the density that needs to be accepted for a cluster in order to have both points belong to the same cluster. 

## More

<https://en.wikipedia.org/wiki/OPTICS_algorithm>

## Code

<https://github.com/Flowerowl/optics>
