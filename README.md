# linux-c-lab4
Exercise 1: Basic Fork
What I did: I wrote a C program that calls fork() to create a child process. The parent prints "I am the parent" and the child prints "I am the child," each showing its own PID.

Testing: I compiled with gcc fork1.c -o fork1 and ran ./fork1. The output showed both messages with different PIDs, confirming two separate processes ran.

Observations: fork() returns the child's PID to the parent, 0 to the child, and a negative value on failure. This return value is how the program tells the two processes apart. Both processes start running from the same line after the fork.

Exercise 2: Parent-Child-Grandchild
What I did: I extended Exercise 1 so the child calls fork() again, creating a grandchild process.

Testing: Running ./fork2 printed three messages: "I am the parent," "I am the child," and "I am the grandchild," each with a distinct PID.

Observations: The second fork() only happens inside the child branch, so only the child creates a grandchild. This shows that processes are created in a tree structure from a single parent.

Exercise 3: Multiple Children from One Parent
What I did: I wrote a program where the parent calls fork() twice to create multiple child processes.

Testing: Running ./fork3 produced output showing Child 1, Child 2, and the parent, each with its own PID. I noticed the output order varied between runs.

Observations: Calling fork() twice actually creates up to four processes because the second fork() runs in every process, including the first child. The if/else logic selects which role each process plays. The varying output order shows that process scheduling is not guaranteed to be sequential.

Exercise 4: Using Exec to Run Another Program
What I did: I modified the program so the child process runs ls -l using execlp(), while the parent waits for it to finish.

Testing: Running ./exec4 printed "Executing ls command..." followed by the ls -l directory listing, then "Parent process finished."

Observations: execlp() replaces the child's memory image with the new program, so the child becomes ls. The wait(NULL) call in the parent ensures the parent doesn't finish until the child does, which is why "Parent process finished." appears last.

Exercise 5: Implementing a Simple Shell
What I did: I wrote a simple shell program that reads user commands, forks a child, and executes each command using execlp() in a loop until the user types exit.

Testing: I ran ./shell5 and tested ls, pwd, and exit. Each command executed correctly and printed its output, and the shell exited cleanly when I typed exit.

Observations: The shell combines fork() and exec() — fork creates the child, and exec replaces the child with the requested program. The execlp(command, command, NULL) call looks up the command in the PATH. If exec fails, the child prints "Command execution failed!" and exits, because a successful exec would have replaced the child entirely. The parent waits for each command to finish before showing the next prompt.

Overall Observations
Across all five exercises, I learned that fork() creates a new process as a copy of the parent, and exec() replaces a process with a different program. Together they form the basis of how shells work: fork to create a child, then exec to run the user's command. I also learned the importance of wait() so the parent can control when it continues, and how to use the fork return value to give each process a distinct role.

