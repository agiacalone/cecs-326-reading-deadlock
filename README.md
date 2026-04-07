# CECS 326 Reading Assignment: Deadlock

### Assignment Description
Answer the following questions based on the Chapter 6 reading from your textbook and the lecture *Deadlock — 57 Year Old Bug*. Be complete with your answers. You may work on these questions with one or two other partners, but *all* students must submit the document individually in their own repositories along with each student’s name documented with the submission.

1. A container orchestration system (such as Kubernetes) manages a fixed pool of CPU and memory across a cluster of nodes. Two pods are scheduled concurrently: each requests CPU first, and then memory. Under what conditions may a deadlock occur if cluster resources are limited? How may the deadlock be avoided?

2. In the preceding question, classify CPU time and memory as either preemptable or nonpreemptable resources. How does your classification affect which deadlock prevention strategies can be applied to each?

3. The four conditions (mutual exclusion, hold and wait, no preemption, and circular wait) are necessary for a resource deadlock to occur. Give an example to show that these conditions are not sufficient for a resource deadlock to occur. When are these conditions sufficient for a resource deadlock to occur?

4. Suppose four cars each approach an intersection from four different directions simultaneously. Each corner of the intersection has a stop sign. Assume that traffic regulations require that when two cars approach adjacent stop signs at the same time, the car on the left must yield to the car on the right. Thus, as four cars each drive up to their individual stop signs, each waits (indefinitely) for the car on the left to proceed. Is this anomaly a communication deadlock? Is it a resource deadlock? Identify a scenario in modern concurrent software (e.g., thread synchronization or networked services) that exhibits the same underlying structure.

5. A system has two processes. Process A needs at most one printer and two disk drives. Process B needs at most two printers and one disk drive. The system has two printers and two disk drives. Is deadlock possible? Explain your answer.

6. The banker’s algorithm is being run in a system with `m` resource classes and `n` processes. In the limit of large `m` and `n`, the number of operations that must be performed to check a state for safety is proportional to `m^{a}*n^{b}`. What are the values of `a` and `b`? Given this complexity and the requirement to know each process’s maximum resource needs in advance, explain why the banker’s algorithm is rarely implemented in production operating system kernels. What strategy do most real systems use instead?

7. Two microservices, A and B, communicate over a message queue. Service A sends a request to B and blocks waiting for B’s reply. At the same time, Service B sends a request to A and blocks waiting for A’s reply. Neither service processes incoming requests while blocked. Is this a resource deadlock or a communication deadlock? How could the communication protocol be redesigned to prevent it?

8. One way to prevent deadlocks is to eliminate the hold-and-wait condition. In the textbook it was proposed that before asking for a new resource, a process must first release whatever resources it already holds (assuming that is possible). However, doing so introduces the danger that it may get the new resource but lose some of the existing ones to competing processes. Propose an improvement to this scheme. The Linux kernel enforces a global lock-ordering convention to prevent circular wait — how does this approach relate to your proposed improvement, and what are its limitations?

9. A network protocol handles transmission collisions using exponential backoff: when two nodes transmit simultaneously and collide, each waits a random interval before retrying. (a) Under what conditions could this protocol produce livelock rather than deadlock? (b) Could a node be starved under this protocol? (c) Compare livelock and starvation to deadlock: which of the three requires external intervention to resolve, and why?

10. In 2026, researchers found a bug in the Apollo Guidance Computer’s gyro control code that had survived 57 years of scrutiny. The AGC ran a real-time multitasking OS with shared resource locks. The `LGYRO` lock was acquired at the start of a gyro torque operation and released on normal completion — but if the crew activated the cage switch (an emergency gimbal clamp) mid-torque, the error path exited without releasing the lock. Two instructions were missing from the `BADEND` error routine. Once stuck, every subsequent gyro operation found the lock held and slept waiting for a wake signal that would never come. The DSKY accepted commands normally and produced no alarm.

    (a) Map each of the four Coffman conditions to this scenario. The `LGYRO` bug involves no circular chain of waiting processes — the lock holder simply ceased to exist. Which condition is absent or weakened, and what does this tell you about the four conditions as a *sufficient* criterion for deadlock?

    (b) The AGC’s restart logic cleared `LGYRO` as a side effect of full memory initialization. Explain why this recovery mechanism likely prevented the bug from being detected during testing. Relate your answer to the distinction between *safety properties* and *liveness properties*.

    (c) The fix requires two missing instructions: `CAF ZERO` followed by `TS LGYRO`, added to the `BADEND` error routine. Using Tanenbaum’s terminology, which deadlock prevention strategy does this fix implement, and which Coffman condition does it attack?

    (d) POSIX `sem_wait()` and `sem_post()` are the direct descendants of the AGC’s lock mechanism — both rely on the programmer to release the resource on every exit path. If a process holding a POSIX semaphore is killed before calling `sem_post()`, describe the resulting system state and explain how it mirrors the AGC failure.

### Deliverables
* Your writeup file *must* be done in [Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) format and must be included in the repository as a separate file. View the file [`README.md`](README.md?plain=1) for an example of Markdown.
* Any included images or screenshots should be done in `*.jpg`, `*.png`, or `*.gif` formats, and be included individually as files in your repository (i.e. no binary ‘document’ with the images pasted inside).
* Screenshots or images *may* be linked in your Markdown file writeup if you wish to do so.