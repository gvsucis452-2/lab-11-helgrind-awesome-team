## Lab 11 -- Answers: Reegan Graham & Bilal Redzic
### Question 1
* Yes, the correct lines of code are pointed to. This can be seen in the Helgrind output at main `(main-race.c:15)` and `(main-race.c:8)`, which identify the lines in main-race.c involved in the race condition.

* The Helgrind output also provides additional information about which threads were created, where the possible data race occurred, the memory location and shared variable involved, and whether any locks were being held at the time.

### Question 2
Helgrind does not find any race conditions, as seen below:
![alt text](image.png)

### Question 3
Helgrind still reports a data race because the other place where `balance` is modified is still unprotected.
![alt text](image-1.png)

### Question 4
Helgrind does not find any race conditions, as seen below:
![alt text](image-2.png)

### Question 5
* A deadlock is when locks are formatted in a way that does not let either thread access resources, this causes both threads to stop execution.

* This causes a deadlock because one thread locks `m1` then waits for `m2` while another locks `m2` then waits for `m1`, so both threads are stuck waiting on each other.

### Question 6
Helgrind reports inaccurate accquistion of locks (deadlock), as seen below:
![alt text](image-3.png)

### Question 7
* No, `main-deadlock-global.c` does not have the same real deadlock problem as `main-deadlock.c`.

* It avoids deadlock because the global lock `g` only lets one thread at a time lock `m1` and `m2`.

* Helgrind still reports a lock-order warning because one path locks `m1` then `m2`, while the other locks `m2` then `m1`.

* This shows that Helgrind is useful, but it can still report false positives.
Helgrind reports a lock-order warning, as seen below:
![alt text](<Screenshot 2026-04-06 at 9.41.00 AM.png>)
![alt text](<Screenshot 2026-04-06 at 9.41.07 AM.png>)


### Question 8
This code is inefficient because the parent thread busy-waits in `while (done == 0) ;`. Instead of sleeping, it repeatedly checks `done` over and over, so if the child takes a long time to finish, the parent wastes CPU time spinning.

### Question 9
* Helgrind reports a data race on the shared variable `done` because one thread writes `done = 1` and the other repeatedly reads `done` without any lock or other synchronization. Specifically, the output says there is a "Possible data race during read" in `main (main-signal.c:16)` that conflicts with a previous write in `worker (main-signal.c:9)`.

* The code is not correct. Even though it may seem to work sometimes, the read and write of `done` are unsynchronized, so the program has a race condition and its behavior is not reliable.
Helgrind reports a data race on `done`, as seen below:
![alt text](<Screenshot 2026-04-06 at 9.41.31 AM.png>)

### Question 10
`main-signal-cv.c` is preferred because it uses a mutex and condition variable instead of busy-waiting. The waiting thread sleeps until it is signaled, so it does not waste CPU time, and access to the shared state is properly synchronized. Because of that, it is better for both correctness/performance.

### Question 11
Helgrind does not report any errors, as seen below:

![alt text](<Screenshot 2026-04-06 at 9.41.42 AM.png>)