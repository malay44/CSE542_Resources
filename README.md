# CSE542_Resources

[Raft Paper](https://raft.github.io/raft.pdf)

**Topic: Log Replication in Raft Consensus Algorithm**

**1. Introduction to Log Replication**
- Logs are maintained to ensure consistency among distributed servers.
- The Raft consensus algorithm ensures that logs are replicated and consistent across all servers.

**2. Log Entry Structure**
- Each log entry has a term number, command, and log slot number.
- Term number identifies the leader for that term.
- Log slot number indicates the position of the entry in the log.

**3. Append Entries RPC**
- Leader sends append entries RPCs to followers to replicate log entries.
- Append entries contain information about previous log index and term to maintain consistency.

**4. Handling Append Entries**
- Followers check previous log index and term to validate incoming append entries.
- If the previous log index and term match, followers accept the new log entry.
- If not, followers reject the append entries and request correction from the leader.

**5. Backup Mechanism**
- Followers maintain a next index field to track their progress in replicating log entries.
- If append entries are rejected, the leader decrements the next index for the follower and retries.

**6. Election Restrictions**
- Raft imposes restrictions on leader election to prevent incorrect leaders.
- Candidates must have a log with a higher term and equal or greater length than the voter's log.

**7. Faster Log Backup**
- Raft introduces strategies for faster log backup to handle missed log entries efficiently.
- Followers provide additional information to the leader to determine the extent of backup needed.
- Three cases: missing term in leader's log, conflicting entries with known term, and missing entries in follower's log.

**8. Conclusion**
- Raft's log replication ensures consistency and fault tolerance in distributed systems.
- Strategies for faster log backup improve efficiency and resilience in handling missed entries.

**Note:** Detailed explanation of handling append entries, backup mechanism, election restrictions, and faster log backup strategies provided. The lecture emphasizes the importance of maintaining log consistency and introduces techniques to optimize log replication in Raft.

**Topic: Persistence and Log Compaction in Raft Consensus Algorithm**

**1. Introduction to Persistence:**
- In Raft, certain elements of the state are marked as persistent, meaning they are stored on disk or non-volatile storage.
- Persistence ensures that essential information is preserved across server reboots or crashes, allowing servers to resume their operation seamlessly.

**2. Significance of Persistence:**
- Persistent data ensures that servers can pick up where they left off after a crash or restart, maintaining system integrity.
- It distinguishes between volatile and persistent data, emphasizing the importance of preserving critical information for fault tolerance.

**3. Implementation Challenges:**
- Writing persistent data to disk incurs a performance cost, particularly with mechanical hard drives, where disk writes can take around 10 milliseconds.
- This cost limits system performance, necessitating efficient strategies for persisting data without sacrificing speed.

**4. Ensuring Data Persistence:**
- To ensure data persistence, systems typically use mechanisms like the `fsync` system call in Unix-based systems, which ensures that data is safely written to disk.
- Batch processing of data updates before persisting them can help reduce the overhead associated with disk writes, improving system performance.

**5. Log Compaction and Snapshots:**
- As systems run for extended periods, the Raft log can become excessively large, consuming significant memory and disk space.
- Log compaction and snapshots offer a solution by allowing systems to capture a snapshot of the application state at a particular point in the log.
- These snapshots, along with the subsequent log entries, enable systems to discard earlier log entries, reducing memory and disk usage while preserving system state.

**6. Benefits of Log Compaction:**
- Log compaction optimizes storage by discarding redundant or outdated log entries, effectively compressing the log size.
- By storing snapshots and discarding unnecessary log entries, Raft systems can efficiently manage system resources and improve performance.

**7. Implementation Considerations:**
- Raft systems must carefully manage snapshots and ensure that essential data is preserved while discarding unnecessary log entries.
- Snapshots must be annotated with the corresponding log index to facilitate recovery and consistency across servers.

**8. Conclusion:**
- Persistence and log compaction are crucial aspects of the Raft consensus algorithm, ensuring system resilience and efficient resource utilization.
- By implementing effective persistence strategies and leveraging log compaction techniques, Raft systems can achieve high performance and fault tolerance, even in extended operation scenarios.

**Note:** The discussion highlights the importance of persistence, challenges in implementation, benefits of log compaction, and considerations for effective system design. Strategies for ensuring data integrity and optimizing performance are emphasized throughout the explanation.

It seems like you're discussing concepts related to system restarts, snapshotting, and linearizability in distributed systems. Here's a summary of the key points discussed:

1. **System Restart and Snapshotting:**
   - After a restart, the application needs to initialize itself by absorbing the latest snapshot found by Raft.
   - Raft manages snapshotting, but the snapshot contents are specific to the application.
   - The leader can discard parts of its log, requiring a mechanism like the Install Snapshot RPC to bridge the gap between the follower's log and the leader's log.

2. **Linearizability:**
   - Linearizability is a notion of correctness for replicated services or any other service, ensuring that client requests and responses behave as if there was just one server executing commands sequentially without crashes.
   - An execution history is linearizable if there exists a total order of operations that matches the real-time order of requests and ensures that reads see the value from the most recent preceding write.
   - Linearizability helps determine if a system's behavior is correct, even in complex scenarios where RPCs can be unreliable or out of order.

3. **Example Illustrations:**
   - Examples were provided to illustrate linearizability using execution histories with write and read operations.
   - In one example, the execution history was shown to be linearizable because it could be ordered to meet the constraints of linearizability.
   - In another example, the execution history was shown not to be linearizable due to constraints that couldn't be satisfied simultaneously in any order.

Overall, the discussion covers the intricacies of ensuring correctness in distributed systems, especially in scenarios involving system restarts, snapshotting, and linearizability.

<img width="750" alt="Screenshot 2024-02-18 at 2 02 01 PM" src="https://github.com/malay44/CSE542_Resources/assets/101856674/1a77bf71-fa8c-41e0-8467-706067f782e6">

<img width="1062" alt="Screenshot 2024-02-18 at 10 34 43 PM" src="https://github.com/malay44/CSE542_Resources/assets/101856674/1c07ac80-81d4-4e89-be80-663a0267c4b4">
