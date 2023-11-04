# Competitive Analysis
Competitive analysis, in the context of online and offline algorithms, is a technique used in computer science and optimization to compare the performance of two different algorithms when solving a particular problem. The primary focus is on measuring how well an online algorithm performs in comparison to an optimal offline algorithm when dealing with a sequence of inputs.
## Online vs Offline Algorithms
1. **Online Algorithm**: An online algorithm is designed to make decisions in a dynamic or streaming environment where it receives input data sequentially and must make decisions without knowledge of future data. Online algorithms are typically evaluated in terms of how well they perform compared to an optimal offline algorithm.
    
2. **Offline Algorithm**: An offline algorithm, on the other hand, has access to the entire input in advance and can make decisions with full knowledge of the future. The goal of competitive analysis is to compare the performance of an online algorithm, which has constraints due to its limited information, against an offline algorithm, which has perfect knowledge.

**Formally**: Defining **S** as a sequence of operation provided one at a time. For each operation:
- An on-line algorithm A must execute the operation immediately without any knowledge of the future operation
- An off-line algorithm may see the whole sequence in advance

We define as **C<sub>A</sub>(S)** as the cost of the online algorithm on a sequence S. Our goal is to minimize C<sub>A</sub>.
## α-Competitive
We define an **on-line** Algorithm **A** as **α-Competitive** if there exists a constant k such that for any sequence of operation **S** :
	C<sub>A</sub>(S)<= α C<sub>OPT</sub>(S)+k
Where OPT is the optimal offline algorithm.


# Self organizing lists

## Idea
We have a lists L of n elements.
The operation Access(x) costs:
- **Rank <sub>L</sub> (X)**= distance of x from the head of L

L can be reordered by transposing adjacent elements at cost of 1.

this is an online algorithm that aims to have most requested element near to the head.

The worst case of this is when someone always look for the tail of the list. (each operation in s look for the last element)
C<sub>A</sub> (S)=omega(|S|*n*)

## Move To Front heuristic

This is a way to implement a self organizing lists, each time you access an element x you move x to the head of the list using transposes. This cost you :
![[Pasted image 20231104200659.png]]
**It is proved that MTF is 4-competitive for self organizing lists**
The proof use what we have seen in [[12.Amortized Analysis#Potential method]].

It is also proved that if **we can move x in the head of the list for "free"** the MTF is **2-competitive**