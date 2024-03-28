.. include:: substitutions.rst

|Rana-s and Safra-s Algorithm|
=========================================



Background and Related Work
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Termination detection in distributed systems is a critical problem essential for ensuring system correctness and facilitating efficient resource management. In distributed computing environments, multiple processes collaborate to accomplish a common computational goal, making it challenging to determine when all processes have completed their tasks or have reached a stable state (Chandy & Misra, 1984). This problem is exacerbated by the asynchronous and decentralized nature of distributed systems, where processes operate independently and communication occurs over potentially unreliable networks.

Several termination detection algorithms have been proposed in the literature, each offering unique approaches to address the challenges of distributed termination detection. Chandy and Misra introduced the global snapshot algorithm, which captures a consistent global state of a distributed system to detect termination (Chandy & Misra, 1984). Similarly, Dijkstra proposed the Token-Based algorithm, where a token is passed among processes to track their progress and detect termination (Dijkstra, 1980). Additionally, Dijkstra and Scholten presented the Distributed Edge Marking algorithm, which uses message passing to detect termination in distributed systems (Dijkstra & Scholten, 1982).

Rana's algorithm is one of the most important algorithms as a distributed solution for termination detection in distributed systems. Rana's algorithm leverages message passing among processes to systematically track the progress of computation and infer termination status through message exchanges. This algorithm offers a decentralized approach to termination detection, ensuring system correctness and facilitating efficient resource management.

Safra's algorithm is another very important algorithm as a distributed solution for termination detection in distributed systems. Safra's algorithm combines message passing and set containment principles to construct a tree structure representing the execution of distributed computations. By analyzing specific patterns within this tree, the algorithm efficiently detects termination, signaling the completion of tasks across distributed processes. This algorithm offers a decentralized approach to termination detection, ensuring system correctness and facilitating efficient resource management.

Distributed Algorithm: |Rana-s and Safra-s Algorithm| 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Termination detection algorithms are used in distributed systems to determine when all processes have completed their tasks or computations. These algorithms typically involve processes exchanging messages to signal their termination status, with each process maintaining information about the termination state of others. Once all processes have acknowledged their termination, the system can conclude that all computations have finished. Termination detection is crucial for ensuring the orderly termination of distributed processes and for coordinating further actions in the system.In this section, we delve into termination detection algorithms, focusing on two prominent ones: Rana's algorithm and Safra's algorithm. We will present the principles behind these algorithms, their implementation details, and compare their strengths and weaknesses.

**Rana's Termination Detection Algorithm:**
Rana's algorithm provides a distributed approach to termination detection, allowing processes in a distributed system to coordinate and terminate gracefully once all tasks have been completed.

    1. Initialization:
        - Each process initializes its local termination flag to false.
        - A counter is set to track the number of termination messages received.
        - A termination vector is initialized to keep track of the termination status of other processes.
    2. Sending Termination Messages:
        - When a process believes it has completed its task, it sends a termination message to all other processes except itself.
        - This message indicates to other processes that the sender has terminated its work.
    3. Receiving Termination Messages:
        - Upon receiving a termination message from another process, the receiving process updates its termination vector to indicate that the sender has terminated.
        - It also increments a counter to keep track of the number of termination messages received.
    4. Local Termination Detection:
        - After updating its termination vector upon receiving a termination message, a process checks whether all other processes have terminated by examining the termination vector.
        - If all elements in the termination vector are true (indicating all processes have terminated), the local termination flag of the process is set to true.
    5. Main Loop and Computation:
        - The algorithm typically operates within a main loop where computations are performed.
        - After completing its portion of work, a process checks if termination conditions are met and sends termination messages accordingly.
    6. Finalization:
        - Once a process detects local termination (all processes have terminated), it sends termination messages to all other processes to inform them of its own termination.
        - It then waits for acknowledgment from all other processes to ensure that they have received the termination message.
        - Finally, the process can safely terminate.
    7. Termination Acknowledgment:
        - Processes wait for acknowledgment from all other processes after sending termination messages to ensure that the termination is globally acknowledged.
        - This step ensures reliable communication and coordination among processes.

Rana's termination detection algorithm is presented in :ref:`Algorithm <RanasAlgorithm>`.

.. _RanasAlgorithm:

.. code-block:: RST
    :linenos:
    :caption: Rana's algorithm.
    
    // Initialize variables and data structures
    initialize:
        set local_termination_flag to false
        set termination_messages_received to 0
        set total_processes to N
        initialize termination_vector with all elements as false

    // Send termination messages to other processes
    send_termination_message():
        for each process p from 1 to N:
            if p is not the current process:
                send "TERMINATE" message from current process to process p

        // Wait for acknowledgment from all other processes
        while termination_messages_received < total_processes - 1:
            wait_for_message()

    // Receive termination messages from other processes
    receive_termination_message(sender_process):
        // Update termination status of the sender process
        set termination_vector[sender_process] to true
        increment termination_messages_received

        // Check if all processes have terminated
        if termination_messages_received equals total_processes - 1:
            set local_termination_flag to true

    // Main function
    main():
        // Initialization
        call initialize()

        // Main loop
        while local_termination_flag is false:
            // Perform computation

            // Check if computation is complete
            if computation is finished:
                // Send termination message to other processes
                call send_termination_message()

        // Finalization
        // Send termination messages to all other processes
        call send_termination_message()

        // Wait for acknowledgment from all other processes
        while termination_messages_received < total_processes - 1:
            wait_for_message()

        // Terminate
        terminate()


**Safra's Termination Detection Algorithm:** 
Safra's algorithm provides a robust method for detecting termination in distributed systems by constructing and analyzing a global wait-for graph. It ensures that all processes have completed their tasks and there are no potential deadlocks before declaring termination.
    
    1. Local State Maintenance:
        - Each process in the distributed system maintains a local state indicating whether it has terminated or not.
        - Initially, all processes are assumed to be active, meaning they are actively executing tasks.
    2. Message Passing:
        - Processes exchange messages with their neighboring processes to gather information about their status and dependencies.
        - Messages exchanged typically include marker messages and acknowledgment messages.
    3. Marker Propagation:
        - To initiate the termination detection process, a special marker message is sent by an initiator process. This marker message is used to mark the beginning of the termination detection phase.
        - Upon receiving a marker message from a neighbor, a process records the receipt of the marker and may initiate further marker propagation.
    4. Recording Dependencies:
        - As marker messages are exchanged, processes record information about the dependencies between them.
        - Each process maintains a set of incoming and outgoing edges in the global wait-for graph based on the marker messages it has received.
    5. Cycle Detection:
        - Safra's algorithm ensures that no cycles exist in the global wait-for graph. The presence of a cycle would indicate the possibility of a deadlock in the distributed system.
        - Processes collaborate to ensure that the global wait-for graph remains acyclic.
    6. Termination Detection:
        - Termination is detected when all processes have determined that there are no cycles in the global wait-for graph.
        - Once a process has received acknowledgment messages from all of its incoming neighbors, it marks itself as passive, indicating that it has terminated.
        - The termination detection process continues until all processes have marked themselves as passive and the global wait-for graph is confirmed to be acyclic.

Safra's termination detection algorithm is presented in :ref:`Algorithm <SafrasAlgorithm>`.

.. _SafrasAlgorithm:

.. code-block:: RST
    :linenos:
    :caption: Safra's algorithm.
    

    Initialization:
    For each process P:
        Set local state of P to active
        Initialize an empty set of received messages for P
        Initialize an empty set of outgoing edges for P

    Termination Detection:
    Repeat until termination is detected:
        For each process P:
            If local state of P is active:
                Send a marker message to each neighboring process Q
                Wait to receive marker messages from all incoming neighbors

            Upon receiving a marker message from neighbor Q:
                Record the marker message from Q
                Add a directed edge from Q to P in P's outgoing edges

            If received marker messages from all incoming neighbors:
                Mark local state of P as passive
                If P is not the initiator:
                    Send marker messages along outgoing edges
                Else:
                    Check if the global wait-for graph is acyclic:
                        If acyclic:
                            Termination detected
                        Else:
                            Continue

    Check for Cycle in the Global Wait-for Graph:
    Function IsAcyclic(graph):
        Initialize a set of visited nodes
        Initialize a set of current nodes

        For each node in graph:
            If node has not been visited:
                If Visit(node, visited, current):
                    Return false (cycle detected)

        Return true (no cycle detected)

    Function Visit(node, visited, current):
        If node is in current:
            Return true (cycle detected)
        If node is in visited:
            Return false

        Add node to current
        Add node to visited

Example
~~~~~~~~

Provide an example for the distributed algorithm.

Correctness
~~~~~~~~~~~

**Correctness of Rana's Algorithm**

Correctness Proof: Initially, all local termination flags are false, and the termination vector is initialized with all elements set to false.  Upon receiving termination messages from all other processes, a process sets its local termination flag to true. It sends termination messages to all other processes, indicating its own termination. This process repeats until all processes have set their local termination flags to true. Once all processes have set their local termination flags to true, the algorithm terminates.

Safety Proof: A process sets its local termination flag to true only if it has received termination messages from all other processes. This ensures that termination is detected only when all processes have indeed terminated their tasks. Each process waits for acknowledgment from all other processes after sending termination messages. This ensures that termination is communicated reliably among processes.

Liveness Proof: Each process sends termination messages to all other processes upon completing its task. Eventually, every process will receive termination messages from all other processes. nce a process receives termination messages from all other processes, it sets its local termination flag to true and sends termination messages to all other processes. This process repeats until all processes have set their local termination flags to true. Since every process sends termination messages to all other processes, eventually, all processes will receive termination messages from every other process, causing them to set their local termination flags to true. Therefore, the algorithm eventually detects termination once all processes have completed their tasks.

Fairness Proof: The algorithm ensures symmetric message exchange among processes, where each process sends termination messages to all other processes and waits for acknowledgment. This ensures fairness in communication. All processes follow the same steps to detect termination. There is no bias or preferential treatment given to any process. All processes wait for acknowledgment from all other processes after sending termination messages. This ensures that no process is left behind in the termination process, ensuring fairness.

**Correctness of Safra's Algorithm**

Correctness Proof: The algorithm constructs a global wait-for graph based on marker messages exchanged between processes. Each process records dependencies and constructs its portion of the graph accurately. Safra's algorithm ensures that the global wait-for graph remains acyclic throughout the termination detection process. This guarantees that deadlock situations are detected accurately. Processes mark themselves as passive (terminated) only when they have received acknowledgment messages from all incoming neighbors. This process continues until all processes have marked themselves as passive. Termination is declared only when all processes have indeed terminated, ensuring correctness. A formal proof by contradiction can be constructed to demonstrate that any assumption of incorrect termination detection leads to a contradiction, thus establishing the correctness of the algorithm.

Safety Proof: The algorithm ensures that all processes reach the same conclusion regarding termination. This property is guaranteed by the consistent construction of the global wait-for graph and the uniform termination detection criteria. Processes accurately record dependencies based on the receipt of marker messages, ensuring that the global wait-for graph correctly represents the dependency relationships between processes. By ensuring the absence of cycles in the global wait-for graph, Safra's algorithm prevents the occurrence of deadlocks, contributing to the safety of the distributed system.

Liveness Proof: The algorithm makes progress towards termination detection by propagating marker messages and updating local states. Processes continue to exchange messages and update their states until termination is detected. Safra's algorithm guarantees that termination detection eventually occurs, regardless of the initial state of the distributed system or the timing of message exchanges. This property ensures the eventual resolution of termination detection.

Fairness Proof: All processes participate equally in the termination detection process by exchanging marker messages and updating their states based on received messages. There is no bias in the treatment of processes. The algorithm exhibits symmetry in its operation, meaning that processes follow the same rules and procedures for termination detection. This symmetry ensures fairness in the treatment of processes.


Complexity 
~~~~~~~~~~
**Complexity of Rana's Algorithm**

Time complexity of the Rana's Algorithm is O(N^2) where N is the total number of processes.

Message complexity of the Rana's Algorithm is O(N^2) where N is the total number of processes.

**Complexity of Safra's Algorithm**

Time complexity of the Safra's Algorithm is O(N^2) where N is the total number of processes.

Message complexity of the Safra's Algorithm is O(N^2) where N is the total number of processes.
