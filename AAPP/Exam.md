//todo
- Describe the main features of Apache Spark
- Describe the main steps of the evolution of parallel programming technologies
- Why data scalability of a program is an advantage?
- Which is the aim of OpenMP run-time routines?
- How can the search of an element inside an (unbalanced) binary tree be accelerated using OpenMP?
- Describe the map parallel pattern and at least one fusion optimization that can be applied to it.

**Which are the advantages of using extensions of sequential languages instead of native parallel languages**?
Anche se i native parallel languages sono nati per scrivere programmi paralleli può risultare conveniente usare delle estensioni dei sequential languages perchè: 
1. non si deve imparare un nuovo linguaggio e una nuova sintassi
2. i compilatori di questi linguaggi sono molto più ottimizzati in quanto esistono da decenni



**Describe the advantages and disadvantages of MPI**
Advantages:
- The same program can be compiled for different architecture
- is a scalable solution, meaning that it can efficiently handle the increase of computational resources 
- Synchronization and communication are explicit
Disadvantage:
- A lot of overhead is generated in the communication
- programming paradigm for distributed memory are harder than the one for shared memory
- the hardware improvement are not immediately reflected on the implementation of mpi

**Which are the factors that can contribute to limit the resource scalability of a program?**
They are:
- Dependencies: if we have some dependencies that avoid more task to run together we cannot exploit all the computational resource but only some of them
- overhead: if we have a lot of overhead we are using the resource but in a "useless" way, meaning that in the worst case we have better performance with only 1 cpu (our resource) that with more

**Which is the aim of the default clause in OpenMP?**
The default clause is used do define the default scope of the variables in a parallel region. It can be shared (if we do not use the clause this is the default behaviout) private (not possible in some implementation), and none. if we use none we have to explicitly define the scope for all variables. 

**Which are the clauses which can be associated with omp for and which is their aim?**
the clause associated with a omp for are:
- schedule: used to define how iterations are splitted among the processes. possible value: static (we equally divide them into bloc of size chunk and give each process a block); Dynamic(when a thread finish to work to a block it starts to work on another); Runtime (depends on env variable omp_schedule)
- nowait: when a process ends it work on its part of the for it can go on an proceed with the other work (we are eliminating the implicit barrier)
- data scoop clauses: to define the scope of the variable in the parallel region.
**Describe when omp simd can be used**
omp simd can be used when we have a for loop that work on array of element doing an operation that can be vectorized. This can suggest to the compiler to generate vector instruction for that operation and use more element of the same array instead of just one at time. this will add hardware level parallelism and can be combined with thread level parallelism.

**Describe how omp parallel directive works**
When we add a omp parallel directive we are saying to the compiler that the next bloc of code (included in  {...}) has to be executed by more thread (number of thread can be specified in several way, E.G. in the clauses num_thread(n)). 
When we use this all the thread will do what is in the next block of code.
at the end of the parallel region there is an implicit barrier.

**Describe how pthread_join works**
pthread_join is used to wait a thread for finish its work. in particular when a thread is created the father has a pointer to the new thread. it can do pthread_join(ptr,retrun_value) and it will wait until the read finish its work and in the variable return_value there will be the copy of the argument of pthread_exit

**Threads created in a pthread application are hierarchically organized: True or False?**
false, thread are peers: there is not an implied hierarchy between thread.

**In order to exploit a parallel technology, hardware and compilation tool chain are the only
required elements: true or false?**
False, with those tool only some kind of parallelism can be extracted like instruction level or bit level parallelism. To completely exploit a parallel technology we need higher level tool to design parallel algorithm

**What is Bit Level Parallelism and on which type of architecture it can be implemented?**
Bit level parallelism is the parallelism that we obtain when we handle data as a set of bites and we have instruction that work with several bit at time ( just think to bitwise and that with 1 instruction do the bitwise and on 32/64 bit). it is mostly used in hardware level (like with vhdl) but can become significan also in software level (architecture with vector instruction can exploit simd parallelism)

**Which is the aim of variable scope clauses in OpenMP?**
The aim of the scope calauses in openMP is to define the scope of each variable used in that parallel section. In particular the clauses define the list of vatiable that are shared or private:
- private means that each process has its own copy of the variables
- shared means that each process see and modify the same part of the memory when they modify or read that variable
- 
**Which is the difference between pragma critical and pragma single?**
The difference is that the code in critical is executed by only a process at time (all thread execute it but one at the dime) in single only a process execute the code after the directive

**What does it happen if pragma parallel is used without any nested directive?**
What happen is that all the code in the next block of code its executed by all the processor

**What is a mutex variable in PThread?**
A mutex variable is a special shared variable used to synchronize thread:
after declared a thread can lock() : only a thread at time can do that, the other one will be in queue waiting for the mutex to be free
after a lock() a thread can free the mutex using unlock() let the other thread to lock it.
This mechanism ensure that only a thread at time can go in the code section between lock() and unlock()

**Describe how it is possible to pass arguments of whatever type to threads created by pthread_create**
in the create function the last argument are the arguments, to pass whatever type you just need to cast the pointer to the arguments in a void pointer before pass it to the function

**Execution time is the only metric to be used to evaluate a parallel program: true or false?**
False, you have also to considered the computation resources used in order to estimate the costs. sometimes you may decrease the execute time but in order to do this you use a lot of resources and it may be not worthy

**Describe the scan parallel pattern**
The scan pattern is a pattern that take as input a sequence of element and give in output another sequence.
Element i of the output sequence is a reduction of all element until element i of the input sequence. We can dividei in :
- exclusive scan: the element i of the input is not included in the reduction
- inclusive scan: element i of the input is included
example: in exclusive scan using add out [3] = in[0]+in[1]+in[2]
To parallelize the scan we can divide it in 2 phase:
1. upSweep, reduce all the input
2. down sweep calculate partial result

**Design of parallel algorithms must be performed independently from the architecture which will be used for the implementation: true or false?**
True, when designing a parallel algorithm we must think dependencies between data and parallel task.

**Automatic parallelization cannot be used because it produces wrong solutions: true or false?**
False, automatic parallelization is done at compiling time, and the compiler have to produce code that produces correct solution. if it is not sure that the solution will be correct it just don' t parallelize it

**Describe the reduce parallel pattern**
The reduce parallel pattern is a patter that takes in input a collection of data and an associative function that can be applied between the element of the collection and its output is a value that is the function applied pairwise of all element of the collections.
In its parallel version we can calculate together partial result (apply the to partial input) and then combine this partial result together. ideally we want to do this following a tree shape obtaining log(n) time complexity.

**OpenMP and pthread can be used to implement the same type of parallel algorithms. True or false?**
Since they are both a shared memory technology i would say true.
we can reach the generality of pthread using pragma task, private variable and if clousure

**Describe pthread condition variable and where they are used.**
The condition variable are special variable used in pthread used to synchronize thread. Their aim is to avoid busy waiting and using signal instead. for example if we need a mutex and we cant lock it instead of constantly try to lock it what we can do is start to wait con a condition variable c. The thread who hold the mutex when ha released it can signal the condition and put it a true; this will wake up the other process that now can lock the mutex.

**In OpenMP it is not possible to explicitly synchronize threads: true or false?**
false, there are directive whose aim is to synchronize thread like the barrier (implicit in a lot of other directive).

**OpenMP supports nested parallelism: true or false?**
Yes id does, if we add a parallel section into another parallel section each tread will spawn other thread

**Which is the aim of the single pragma?**
its aim is to let the following block of code be execute by only one thread

**In OpenMP directives it is possible to specify the scope of variables. True or false?** 
True, using the clause private, shared and default we can define the scope of the variable into the block of code that follow that 
directive
**Which type of processing elements can implement Single Instruction Multiple Data parallelism?**
The processing elements that can execute that type of parallelism are:
- cpu with vector instro
- gpu

**A parallel algorithm is always faster than a sequential algorithm: true or false?**
False, there are several factor that can make a parallel algorithm slower like dependencies, slow link, communication overhead

**Briefly explain the difference between unary and n-ary parallel map**
Unary map work with just 1 collection of data as input and apply the function to all the element of the data. n-ary map work with n collection of data:
- for example 2-ary map with sum as operation work with 2 input collection and does the sum element by element of those 2 collections.
**OpenMP is mainly composed of a library of C functions: true or false?**
False, it is composed from a set of compiler directive

**Describe how OpenMP sections can be used to parallelize a for loop**
We can exploit them write several section, each of them having a for loop working on different index of the array (example: sect 1 work with index from 0 to 3, sect 2 from 4 to 10 and so on).
since each section is done by only a thread we are splitting a for between several thread (dependencies issues has to be considered to understand if this can be done). 

**Describe what is MPI**
MPI is a interface that define a set of function used to make threads communicate in a private memory scenario. It has both function for point to point communication and for collective communication.

**Describe how MPI_Send works**
MPI_send is a function used to send a data from a thread to another thread. it takes in input a  pointer to the data to send, the number of element that has to be sended, the type (MPI_type), the rank of the receiver and the communicator. It has several flavor :
- Ssend to send synchronously 
- Bsend to aggregate several send and send them together
- Rsend to send only if the maching recv is already stared

**Is there any restriction on the arrival of messages sent from different processes?**
Yes, if a thread A recv n message from B, A will recv those message in the same order B has sended them but there is no guarantee that in between there are other message or guarantee of order with messages from other sender. (relative fifo)

**Pthread can be used to implement the MIMD type of parallel algorithms. True or false?**
True, just think thread that do several things and operate on different data at the same time. in order to split the work we can use the thread rank

**Describe pthread create and where it is used.**
Pthread create is the function used to spawn a new thread. it takes as input the variable that will hold the reference to this thread (that can be used to synchronize with this thread), the pointer to the function that this thread has to start when spawned and the pointer to the arguments. 
When it is invoked it just spawn a new thread, that will run the chosen function
In the input we can also pass attribute:
- detachable 
- joinable 
- scheduling
- stack size

**OpenMP is an API dedicated to the acceleration of applications in distributed-memory environments: true or false?**
False, it is a set of compiler directive

**The maximum number of threads that can be spawned in OpenMP depends on the underlying hardware architecture: true or false?**
False, We can also decide to have more threads than the number of core in the underlying architecture using clauses or enviromental variables.
**List and describe the different kinds of parallelism according to the Flynn taxonomy, together with architectures implementing them when possible.**
SISD: sequential architecture
SIMD: gpu, vector instruction cpu
MISD
MIMD: general purpose multicore cpu

**It is not recommendable to mix parallelism technologies since doing so increases the complexity of the compiled binaries: true or false?**
False, in some case can be useful to have different technologies to implement some kind of algorithm (eg openmp + cuda)

**Is the synchronization between processes explicit or implicit**
It is explicit, you can do that using thing like barrier or blocking semantics 

**Describe the behavior of MPI_Bcast.**
MPI_bcast is a function used to brodcast data to all thread into a communicator.
it takes as input a pointer to the data, the number of element to be send, the root (who initially hold the data) and the communicator where to brodcast the data. When a thread invoke the function if it is the root it will send the data to all otherwise it will recive the data in the indicated buffer.

**What are communicators? What is the advantage of defining multiple communicators in an MPI program?**

**How can a thread in pthread communicate the result of its computation after completing its routine?**
if we join a thread, we have the retval argument that when the thread that we have join finish its computation will contain the argument of pthread_exit, we can use this to cummunicate the result.

**Can PThread be used to implement a fixed-depth parallel pipeline? How should the pipeline stages synchronize their execution?**
Yes it can be used to do that.
To synchronize can used barrier: after each thread has done with the work on its chunk it will reach the barrier, when all thread have done this means that new input is ready (the barrier is reached by everyoene) and i can start again.


