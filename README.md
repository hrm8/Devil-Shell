/**********************************************
 * Please DO NOT MODIFY the format of this file
 **********************************************/

/*************************
 * Team Info & Time spent
 *************************/

	Name1: Henrique Moraes 	
	NetId1: hrm8	 	
	Time spent: 25 hours 	 

	Name2: Elder Yoshida
	NetId2: emy2 	
	Time spent: 25 hours 	

/******************
 * Files to submit
 ******************/

	dsh.c 	// Header file is not necessary; other *.c files if necessary
	README	// This file filled with the lab implementation details

/************************
 * Implementation details
 *************************/

/* 
 * This section should contain the implementation details and a overview of the
 * results. 

 * You are required to provide a good README document including the
 * implementation details. In particular, you can use pseudocode to describe your
 * implementation details where necessary. However that does not mean to
 * copy/paste your C code. Specifically, you should explain details corresponding 
 * to (1) multiple pipelines (2) job control (3) logging (3) .c commands 
 * compilation and execution. We expect the design and implementation details to 
 * be at most 1-2 pages.  A plain textfile is encouraged. However, a pdf is acceptable. 
 * No other forms are permitted.

 * In case of lab is limited in some functionality, you should provide the
 * details to maximize your partial credit.  
 * */

	After the devil shell initializes it runs a loop to iterate through the list of jobs. The variable called job_head is the pointer to the first job in the sequence. 
The shell first checks for built-in commands:

cd: A simple change of directory. Implemented with chdir function
jobs: It first tries to reap any terminated job. Then it check for the statuses of 
the jobs and logs them into a .log file.
bg: Performs a standard error checking first for any possible error 
that might occur (e.g. wrong arguments, or job specified is wrong) and then continues
the job without seizing the tty.
fg: Performs a standard error checking first for any possible error 
that might occur (e.g. wrong arguments, or job specified is wrong)and then continues
the job seizing the tty and making the shell wait for the job to finish.

	Note that a method 'logger' was created to log any informative message to a .log file
in order to troubleshoot the program. The logger accepts an unformatted string with 
its arguments along with the proper output format, such as STDERR_FILENO for errors or STDOUT_FILENO for regular messages.

	If none of the build in commands corresponds to the current job, the dsh calls spawn_job.
It first iterates through the processes in the job and forks each of them for future
execution. 

	On the child side, piping and output redirection is established (if necessary)
before executing the program. For piping, two pipes are used. The first is assumed to
be before the current process. If the process has a previous pipe, then the reading end
of the pipe is duped to the input of the process. If the process has a following pipe, 
the write end of the pipe is duped so the process direct its output to the pipe. If the 
process is the last process, it transfers the output of the pipe back to stdout.

	The compiler has been implemented as the first feature of this laboratory exercise. The compiler first reads the command int he process and identifies the extension .c or .cpp, which in turn directs it to the right compiler and generates the right args. Since the compilation must occur before the execution of the compiled program, a child-parent relation can be implemented in this scenario. A fork is called and the child executes the compilation, then the parent calls a job for running the recently compiled program.

	On the parent side, it closes the pipes and waits for the child to complete (if the process is executed on the foreground) after each iteration. It also logs the status of any child that has stopped execution while performing the waitpid command.

	Note that the dsh supports batch mode with syntax './dsh < batchFile'
	
       The Devil Shell also supports sigstop, jobs, fg, bg commands! It is known that the bg command is a bit buggy in a sense that it successfully continues to run the program, but the output of the execution is not suppressed.


/************************
 * Feedback on the lab
 ************************/

It was very hard to figure out what to do and where to start. The guidelines were too
general. The handout could be more specific in terms of what to implement and the mechanics behind some of the presented code. The code in the handout did not have 
any in-depth explanation of what it was meant for.

/************************
 * References
 ************************/

http://www.gnu.org/software/libc/manual/html_node/Implementing-a-Shell.html
several http://stackoverflow.com websites
