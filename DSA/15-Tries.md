# Tries (Prefix Trees)

## Table of Contents
- [What are Tries?](#what-are-tries)
  - [Trie Big O](#trie-big-o)
  - [Use Cases](#use-cases)

## What are Tries?
- A trie is a type of n-ary tree in which each node holds a character and each path down the tree may represent a word.
- `*` nodes (a.k.a. null nodes) indicate that it is a complete word.
- Below: trie example.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/trie.png" alt="Trie" width="80%" />
</p>

### Trie Big O
- Insertion - O(K)
- Search - O(K)
### Use Cases
- It is common to store an entire language for quick prefix lookups (hence, the name prefix tree).
  - cf. hash tables are quick to look up if a string is valid or not. But it cannot determine if a string is a prefix of a valid word or not.
- In certain situations, using tries will keep search time complexities to an optimal limit (the key length).
