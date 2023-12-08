# Automatic Parallelization
A first idea to exploit parallel machine is to automatic parallelize the already existing sequential code.
![[Pasted image 20231208120155.png]]
Here the idea is to let the compiler understand which instruction can be executed at the same time, exploiting the **instruction level parallelism**.
This way of extracting parallelism not always work:
![[Pasted image 20231208120416.png]]
In the above example we can see two for loop that we can think they can be ran in parallel: this is not true, because the two input **and** and **or** can be the same pointer, so the compiler has to give a **correct implementation** and so the two for must be executed in a sequential way: maybe the designer will never use the function with two overlapping input, but the compiler does not know.
## Conclusion
Complete automatic parallelization is not feasible at the moment. Tool are not able to extract **all** the available parallelism from a specification designed to be executed in a sequential way.
# Parallelization by hand
When parallelizing a program by hand the programmer need to give hints to the tool. He will write the parallel algorithm using an high level parallel code and he will leave only the compilation phase to the tool:
![[Pasted image 20231208121315.png]]
Here the critical aspect are:
- Which type of parallelism has to be considered?
- How to design parallel algorithm (parallelize existing algorithm vs writing algorithm from scratch)
- How to provide information about the parallelism to the tool
## Types of parallelism 
There is not a single kind of parallelism, a primary division can be:
- **Instruction level parallelism**
- **Data parallelism**

### Flynn's Taxonomy
This two kind of parallelism can be combined, and this is what we can see in the **Flynn's Taxonomy**.
The Flynn's Taxonomy describes types of parallelism, dividing it in four categories:
- Single Instruction Single Data
- Single Instruction Multiple Data
- Multiple Instruction Single Data
- Multiple Instruction Multiple Data
#### Single Instruction Single Data
This is the sequential case:
- The cpu process single instruction stream, on a single input stream
In this case we will have deterministi execution. 
Used in all single core architecture.
![[Pasted image 20231208122201.png]]
#### Single Instruction Multiple Data
In This case all the core in the architecture execute the same instruction in the same clock cycles but they elaborate different data.
Here we have synchronous and deterministi execution.
This type of parallelism is used in the modern GPU's
![[Pasted image 20231208122406.png]]
#### Multiple Instruction Single Data
Each core process the data with different instruction with a single data stream that is fed into multiple units. Is hard to map this kind of parallelism to an existing architecture.
![[Pasted image 20231208123110.png]]
#### Multiple Instruction Multiple Data
Each core execute different instructions and process different data.
Execution can be synchronous or not, deterministic or not.
This tipe of parallelism is present in every heterogeneous multi-core architecture.
![[Pasted image 20231208123310.png]]

### Bit level parallelism 
We can see bits composing words represent different data. A single instruction can manipulate different data (set of bits) at the same time: just think bit-wise operation.
This kind of parallelism is very relevant in hardware implementation of algorithm but it can become also significant in software implementation(e.g. representing set of elements as strings of bits)

### Instruction level parallelism
Different instructions executed at the same time on the same core. This is supported by multiple execution units, pipeline, vector ecc..
This is the kind of parallelism that can be easily extracted by compilers: to find parallelism the compiler just have to watch the dependencies on the data read and the data written.

### Task level parallelism
==A task is a logically discrete section of computational work==
We can have multiple task running on multiple processor. We cannot automatically extract different task from sequential code, we have to explicit program different task and their work to make them work properly and in a parallel way.

#### Parallel Task Graph
The parallel task Graph is an useful tool when designing a parallel algorithm using task.
In the graph each **vertices is a task** and each **edges represent precedencies data communications**.
Ideally each task execute only once.
![[Pasted image 20231208140709.png]]


#### Pipeline
In this kind of task level parallelism we have sequence of task, that work all at the same time but each one on different data. This is useful when you have to parallelize steaming elaboration such as audio and video encoding. 
Here we can find a producer-consumer way of work: each task work on the output data of the previous one in the chain of dependencies 
![[Pasted image 20231208140628.png]]
#### Communication
How the data are exchanged between tasks? There are mainly two way:
1. **Shared Memory**: all the processor, so all the tasks, share a global memory with the same address space. All modifications in a memory location performed by a processor are seen by all the other processors
2. **Message passing**: Each task has its private address space, so tasks have to explicitly communicate by sending and receiving messages.

# Sum Up
As we have seen we can have different type of parallelism, also in the same program.
example:
![[Pasted image 20231208170959.png]]

From the code above, if the memory location do not overlap we can extract parallelism at different level:
1. **Task**: the two loops can run in parallel (we have only concurrent read, that does not represent a problem from a point of view of the dependencies)
2. **Instructions**: reach iteration of the loop can run in parallel(each instruction work on different index, so they write on different part of the array)
3. **Bit**: each bit can be computed in parallel (bit-wise operation)

Some times design a parallel algorithm by extracting all the available parallelism is not enough, not all the extracted parallelism is exploitable on a real architecture:
- we need to consider which parallelism is available on the considered architecture, **no suitable parallelism can introduce overhead**


# Practical parallel programming
When we have to choose how to write a parallel program we have basically two way:
1. Use **new programming languages**, that are born to write parallel program but they does not have a lot of success since there are to immature compiler and developer need to learn the new language
2. Use **extensions** for existing programming language, more easy to use, that use solid compilers but can describe only some types of parallelism.




