# C++-Lock-free-transactional-KV-Engine
NOTE : Iam working on C++ to replace datasets which use standard mutexes and lack performance. I am working on the atomic T to fix this. Iam first learning Adv. DSA to make this project huge.



A Faster Way to Share Data
Moving from "Stop-and-Wait" to "Go-and-Check":
In C++, using a std::mutex is the old way to keep things safe. But when you have a lot of CPU cores, making threads wait in line is slow. This project looks at Transactional Key-Value Stores (TKVS)—a way to let everyone work at once without crashing.
1. The Problem with Locks
Standard locks are "Negative." They assume everyone is going to mess up the data, so they lock the door before anyone can enter.
 * The Traffic Jam: Only one thread can work at a time. If the thread holding the lock gets stuck, everyone else just sits there.
 * The Deadlock: This is the worst. Thread A waits for B, and B waits for A. The whole system freezes forever.
2. The Better Way: Just Try It
A Transactional store uses a "Try and Verify" cycle. Instead of blocking people, we just check the results at the end.
 * Private Workspace: A thread doesn't touch the main data yet. It makes a quick local copy.
 * Parallel Work: Everyone does their math and logic at the same time. No one is waiting at a gate.
 * The Check: When the work is done, the system asks: "Did anyone else change the main data while I was busy with my copy?"
 * The Finish:
   * If No Conflict: Save the changes to the main store instantly.
   * If Conflict: Throw the work away and try again.
3. The Comparison
| Feature | The Old Lock Way | The New "Try It" Way |
|---|---|---|
| Logic | Assume everyone will crash. | Assume everyone will stay apart. |
| Speed | One at a time. | Everyone works at once. |
| Risk | System freezes forever. | A thread might have to retry. |
| Result | Slow when busy. | Super fast when threads use different keys. |
4. How it Works
We use Version Numbers. For every piece of data, we give it a version number T.
> The Simple Logic:
> If the version T is the same when you start and when you finish, your work is good.
> If the version went up, someone else beat you to it. Abort and try again!
> 
Note: This is great when threads aren't fighting over the exact same spot. If everyone fights for one key, retrying becomes a chore. Use the right tool for the job.


CONCLUSION:


This method moves the hard work from waiting to checking. By letting threads work together and only checking for mistakes at the end, we use the full power of the CPU. It is just a smarter way to handle data.
