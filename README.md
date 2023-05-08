Download Link: https://assignmentchef.com/product/solved-solved-assignment-6-threads-and-semaphores
<br>
What is a Thread?In simple words, a thread is a code thread, a program module like a separate routine but can be running independently. Hence, sometimes it is termed a light-weight process.

A process can create several threads to run in parallel and they share the same global-data space. This convenient arrangement make communication easy. Still, to avoid racing/collision condition during accessing global data, some synchronization method is needed.

Since threads are all within a single process, the code be cannot distribute over several hosts (as several separate processes).

Sample program on how to create threads and use a semaphore to guard data are illustrated in Thread and Semaphore Example.

What is a Semaphore?A semaphore is like a flag that can be set up to control threads (or processes) by stopping them or allowing to proceed the next instruction.

A semaphore has a control count that is initially given to a certain value requested by a process with a syscall sem_init() for inter-thread usage (or sem_open() for inter-process usage). A semaphore ID will be the return of the syscall to be subsequently among multiple threads (or processes).

The syscalls sem_wait() and sem_post() are the actual usage of a semaphore. As a thread (or process) calls sem_wait(), the OS checks if the control count in the semaphore is greater than zero. If so, the OS downcount it and let the thread (or process) continue (in code). Otherwise (control count is already at 0), the thread (or process) will be suspended (put into a waiting queue) until some other thread (or process) eventually calls sem_post() to release it.

As a thread (or process) calls sem_post(), the OS checks if there have been processes previous suspended (by calling sem_wait() like above) and If so, the OS releases a waiting process (FIFO) so it can continue within its code execution. Otherwise (they have been no waiting processes), the OS will just upcount the control count in the semaphore.

If the starting control count of the semaphore was request with 1, the semaphore acts like a control for mutual exclusion (mutex). As all threads (or processes) calls sem_wait() and then sem_post() to synchronize the access of “guarded” critical data between the pair of syscalls. The example would be racing/collision avoidance over the access (read/write) of global data among threads.

Another way to see how this “mutex” works is that since the control count starts with 1, it can only be downcounted to 0 and start to block any new threads (or processes) that come to call sem_wait(). And, the control count can only be count up to 1 (not higher) since each sem_post() call (upcount) must be preceeded by a sem_wait() call (downcount).

Display-Cursor ControlIn the Cursor Control Example folder, code to control the location of the display cursor is demonstrated. See below for the requirement of re-coding from the example code into more useful modular routines.

Assignment DescriptionWrite a single program (CR.c) that can run like the demo (CR). Use a single thread as the creations of two running threads: Chaser and Runner.

Two semaphores are needed are to guard the use of the display (otherwise, racing to control cursor will happen) as well as the global data associated with the current positions of both C and R. The main thread (like a parent) will also use the semaphore to access the global data to check if a collision has arrived in order to show the result and end the run.

Make useful subroutines listed below to modularize your cursor-control functions (see the cursor-control example code to do this):PutChar(): show a letterCursorHome(): position curosr to top-leftCursorOff(): hide cursorCursorOn(): show (restore) cursorClearScr(): clear the whole screenInitScr(): arrange the boxFlashScr(): flash the scrren

In the assignment webpage, the demo folder has an executable CR (short for Chaser and Runner). When run, they seem to be animated letter “C” for Chaser and “R” for Runner.

A CR skeleton code has the main() thread (as the parent) that creates two Child threads to each is give a different pointer to point to their individual information:a single character C or R: symbolinitial position coordinate: row and colwhere on the screen to show current position: show_r and show_c

There will have to be a semaphore sem_video to access the display mutually and exclusively. So, any program statements about writing to the display should be placed between sem_wait() and sem_post() syscalls.

Similarly, to read/write global data, another semaphore sem_data is needed.

Extra CreditUse processes to do the same CR runs instead. Each process does same duties like in threads: each child process does its own randomization, cursor control, show symbol, and show/update its new location as well as location data. The parent prepare things, create children, monitor location updates, and show the result and stop the run upon collision.

The access to the video (cursor control and print out a character) needs a semaphore used among processes. The creation and access of a semaphore in process environment is a little different from threads. Please see Process Semaphore Example.A pipe is needed for data transfers since that is the only channel we know to enable data transmissions among processes (so far). The parent creates a shared pipe for child processes to write back their new positions. The pipe, designated symbol, and initial position are given to a child process during the process creation (as arguments).

The parent will request two more semaphores besides the one for video, in order to post them (while a child waits on a designated semaphore) in a round-robin play sequence. Otherwise, one child may play “faster” than the other. (With multipipes it won’t solve this “too fast” issue, and, still, a semaphore is needed.)

Also, with this additional individual semaphore (parent posts, child waits), the timing to read/write the shared pipe can be done correctly: A child writes its updated new location information into the pipe during its turn playing. The parent process reads the pipes knowing which child the updated information is from. And once a turn is played by both child processes, compare locations read for collision (in its loop).Use These Sys/Lib Calls Onlyfor randomness: getpid(), srand(), rand()for cursor control: write(), strlen(), sprintf()for delay: usleep()for semaphores: sem_init(), sem_wait(), sem_post()for threads: pthread_attr_init(), pthread_create()use only at the end (show collision): printf()Assignment Turn-inSubmit your single source code file (CR.3) in the designated folder as before. No E-mail or late turn-ins. If repeating to submit, use new filenames such as: CR-2.c, CR-3.c, etc.

Clean up your code with a clean format, preamble header, and comments where suited (and can help the grader to understand your code). Do not mess with your code and submision location since this may cause point deduction.