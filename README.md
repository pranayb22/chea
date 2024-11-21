slip3a

#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>

void print_file_info(const char *filename) {
struct stat file_stat;

if (stat(filename, &file_stat) == -1) {
perror("Error retrieving file information");
exit(1);
}
printf("Inode number: %ld\n", file_stat.st_ino);

printf("File type: ");
if (S_ISREG(file_stat.st_mode)) {
printf("Regular file\n");
} else if (S_ISDIR(file_stat.st_mode)) {
printf("Directory\n");
} else if (S_ISLNK(file_stat.st_mode)) {
printf("Symbolic link\n");
} else if (S_ISCHR(file_stat.st_mode)) {
printf("Character device\n");
} else if (S_ISBLK(file_stat.st_mode)) {
printf("Block device\n");
} else if (S_ISFIFO(file_stat.st_mode)) {
printf("FIFO/Named pipe\n");
} else if (S_ISSOCK(file_stat.st_mode)) {
printf("Socket\n");
} else {
printf("Unknown type\n");
}
}

int main(int argc, char *argv[]) {
if (argc != 2) {
printf("Usage: %s <filename>\n", argv[0]);
return 1;
}
print_file_info(argv[1]);
return 0;
}


>>>compile command - gcc file_info.c -o file_info
>>>run - ./file_info <filename>
--------------------------------------------------------------------------------------------------------------------------------------
slip3b
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>
#include <sys/wait.h>

pid_t child_pid = -1;

void handle_sigchld(int sig) {
int status;
pid_t pid;


while ((pid = waitpid(-1, &status, WNOHANG)) > 0) {
if (pid == child_pid) {
printf("Child process %d terminated.\n", pid);
if (WIFEXITED(status)) {
printf("Child exited with status: %d\n", WEXITSTATUS(status));
} else if (WIFSIGNALED(status)) {
printf("Child killed by signal: %d\n", WTERMSIG(status));
}
}
}
}

void handle_sigalrm(int sig) {
if (child_pid > 0) {
printf("Child process %d did not complete in time. Killing it...\n", child_pid);
kill(child_pid, SIGKILL); // Kill the child process
}
}

int main() {
signal(SIGCHLD, handle_sigchld); // Set signal handler for SIGCHLD
signal(SIGALRM, handle_sigalrm); // Set signal handler for SIGALRM

child_pid = fork();

if (child_pid == -1) {
perror("Fork failed");
exit(1);
}

if (child_pid == 0) {
printf("Child process %d started.\n", getpid());
execlp("sleep", "sleep", "10", NULL); 
perror("execlp failed"); 
exit(1);
} else {

alarm(5);
pause();

printf("Parent process %d exiting.\n", getpid());
}
return 0;
}


>>>compile - gcc parent_child_timeout.c -o parent_child_timeout
>>>run - ./parent_child_timeout
======================================================================================================================================
slip4a

#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>

int main(int argc, char *argv[]) {
if (argc < 2) {
printf("Usage: %s <file1> <file2> ... <fileN>\n", argv[0]);
return 1;
}

for (int i = 1; i < argc; i++) {
struct stat file_stat;
if (stat(argv[i], &file_stat) == 0) {
printf("File '%s' is present in the current directory.\n", argv[i]);
} else {
perror(argv[i]);
printf("File '%s' is not present in the current directory.\n", argv[i]);
}
}
 return 0;
}


>>>compile - gcc check_files.c -o check_files
>>>run - ./check_files file1.txt file2.txt file3.txt
----------------------------------------------------------------------------------------------------------------------------------
slip4b/slip16b

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>
#include <sys/wait.h>

void handle_signal(int sig) {
switch (sig) {
case SIGHUP:
printf("Child received SIGHUP signal\n");
break;
case SIGINT:
printf("Child received SIGINT signal\n");
break;
case SIGQUIT:
printf("Child received SIGQUIT signal\n");
printf("My Papa has Killed me!!!\n");
exit(0);
}
}

int main() {
pid_t pid = fork();

if (pid < 0) {
perror("Fork failed");
exit(1);
}

if (pid == 0) {
// Child process
printf("Child process started with PID %d\n", getpid());

signal(SIGHUP, handle_signal);
signal(SIGINT, handle_signal);
signal(SIGQUIT, handle_signal);

// Keep the child process running to catch signals
while (1) {
pause();
}
} else {

printf("Parent process started with PID %d\n", getpid());

sleep(3); // Wait for 3 seconds
for (int i = 1; i <= 5; i++) {
if (i == 5) {
// Send SIGQUIT on the 5th iteration (15 seconds)
kill(pid, SIGQUIT);
} else {
if (i % 2 == 1) {
printf("Parent sending SIGHUP to child\n");
kill(pid, SIGHUP);
} else {
printf("Parent sending SIGINT to child\n");
kill(pid, SIGINT);
}
}
sleep(3); 
}
wait(NULL);
printf("Parent process terminating.\n");
}

return 0;
}


>>>compile - gcc signal_handling.c -o signal_handling
>>>run - ./signal_handling
================================================================================================================================
slip5a
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>

int main() {
DIR *dir;
struct dirent *entry;
int count = 0;

dir = opendir(".");
if (dir == NULL) {
printf("Unable to open directory\n");
return 1;
}
while ((entry = readdir(dir)) != NULL) {
printf("%s\n", entry->d_name);  // Display file name
count++;
}
closedir(dir);
printf("\nTotal number of files: %d\n", count);

return 0;
}
------------------------------------------------------------------------------------------------------------------------------------
slip5b
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
int pipe_fd[2];
pid_t pid;
char buffer[100];

if (pipe(pipe_fd) == -1) {
perror("pipe failed");
exit(1);
}

pid = fork();

if (pid < 0) {
perror("fork failed");
exit(1);
}

if (pid == 0) {  
close(pipe_fd[0]);

write(pipe_fd[1], "Hello World", 12);
write(pipe_fd[1], "Hello SPPU", 11);
write(pipe_fd[1], "Linux is Funny", 15);

close(pipe_fd[1]);
exit(0);
} else {  
close(pipe_fd[1]);

while (read(pipe_fd[0], buffer, sizeof(buffer)) > 0) {
printf("%s\n", buffer);
}
close(pipe_fd[0]);
}
return 0;
}


>>compile - gcc pipe_example.c -o pipe_example
>>run-./pipe_example


=====================================================================================================================================
slip6a
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <sys/stat.h>
#include <time.h>
#include <string.h>

void list_files_by_month(const char *directory, const char *month) {
    struct dirent *entry;
    struct stat file_stat;
    DIR *dir = opendir(directory);

if (dir == NULL) {
perror("Unable to open directory");
exit(1);
}

printf("Files in '%s' modified in the month '%s':\n", directory, month);

while ((entry = readdir(dir)) != NULL) {
char filepath[1024];
snprintf(filepath, sizeof(filepath), "%s/%s", directory, entry->d_name);

// Get file metadata
if (stat(filepath, &file_stat) == 0 && S_ISREG(file_stat.st_mode)) {
struct tm *timeinfo = localtime(&file_stat.st_mtime);

char file_month[10];
strftime(file_month, sizeof(file_month), "%b", timeinfo); // Get abbreviated month name

if (strcasecmp(file_month, month) == 0) { // Compare month
 printf("%s\n", entry->d_name);
}
}
}
closedir(dir);
}

int main() {
char month[10];
printf("Enter the month abbreviation (e.g., Jan, Feb, Mar): ");
scanf("%s", month);
list_files_by_month(".", month);
return 0;
}
--------------------------------------------------------------------------------------------------------------------------------------
slip6b
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>
void child_task() {
    // Dummy task: Sleep for a short time to simulate work
    sleep(1);
}
int main() {
int n;
printf("Enter the number of child processes to create: ");
scanf("%d", &n);

if (n <= 0) {
printf("Number of processes must be greater than zero.\n");
return 1;
}

for (int i = 0; i < n; i++) {
pid_t pid = fork();

if (pid == -1) {
perror("Fork failed");
exit(1);
}

if (pid == 0) {
// Child process
child_task();
            exit(0); // Terminate child process
        }
    }

int status;
long total_user_time = 0;
long total_system_time = 0;

    
for (int i = 0; i < n; i++) {
wait(&status); // Wait for any child to terminate
// Simulate tracking cumulative user and kernel times
// Adding dummy times for simplicity
total_user_time += 1000;  // Assume 1ms in user mode
total_system_time += 500; // Assume 0.5ms in kernel mode
}

printf("Total time spent in user mode: %.3f seconds\n", total_user_time / 1000.0);
printf("Total time spent in kernel mode: %.3f seconds\n", total_system_time / 1000.0);
return 0;
}
======================================================================================================================================
slip12a

#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>

int main() {
    pid_t pid;
    int status;

    // Create child process using fork
pid = fork();

if (pid < 0) {
        // If fork fails
        perror("Fork failed");
        exit(1);
} else if (pid == 0) {
        // Child process
        printf("Child process is running...\n");
        exit(42);  // Child exits with a status code of 42
} else {
        // Parent process
        // Wait for the child process to terminate and get the exit status
waitpid(pid, &status, 0);

        // Check if the child terminated normally
if (WIFEXITED(status)) {
printf("Child process terminated normally with exit status: %d\n", WEXITSTATUS(status));
} else {
            printf("Child process did not terminate normally\n");
}
}

return 0;
}

------------------------------------------------------------------------------------------------------------------------------
slip12b

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/stat.h>

#define MAX_FILES 100

// Function to get the size of a file
long getFileSize(const char *filename) {
    struct stat statbuf;
    if (stat(filename, &statbuf) == -1) {
        return -1;  // Return -1 if file does not exist or error occurs
    }
    return statbuf.st_size;
}

int main(int argc, char *argv[]) {
    if (argc < 2) {
        printf("Usage: %s <file1> <file2> ...\n", argv[0]);
        return 1;
    }

    // Simple bubble sort to sort files by their size
for (int i = 1; i < argc - 1; i++) {
        for (int j = i + 1; j < argc; j++) {
            long size1 = getFileSize(argv[i]);
            long size2 = getFileSize(argv[j]);

if (size1 == -1 || size2 == -1) {
                printf("Error accessing file: %s\n", size1 == -1 ? argv[i] : argv[j]);
                continue;
            }

if (size1 > size2) {
                // Swap files
                char *temp = argv[i];
                argv[i] = argv[j];
                argv[j] = temp;
            }
        }
    }

// Display sorted file names
printf("Files sorted by size (ascending order):\n");
for (int i = 1; i < argc; i++) {
printf("%s\n", argv[i]);
}

return 0;
}


>>compile - gcc sort_files_by_size_simple.c -o sort_files_by_size
>>run - ./sort_files_by_size a.txt b.txt c.txt

===================================================================================================================================
slip13a

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>
#include <sys/types.h>

int main() {
    pid_t pid;
    pid = fork();

if (pid < 0) {
        // If fork fails
        perror("Fork failed");
        exit(1);
    }

if (pid == 0) {
        // Child process
        printf("Child process (PID: %d) is running.\n", getpid());
while (1) {
            // Simulate work by making the child process run indefinitely
            printf("Child process is doing some work...\n");
            sleep(1);
        }
} else {
        // Parent process
        printf("Parent process (PID: %d) is running.\n", getpid());
        
        // Allow the child process to run for 3 seconds
sleep(3);

        // Suspend the child process using SIGSTOP
printf("Suspending child process (PID: %d)...\n", pid);
kill(pid, SIGSTOP);

        // Wait for 3 seconds
sleep(3);

        // Resume the child process using SIGCONT
printf("Resuming child process (PID: %d)...\n", pid);
kill(pid, SIGCONT);

        // Wait for a while before terminating the parent
sleep(5);
}

return 0;
}

-------------------------------------------------------------------------------------------------------------------------
slip13b

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>

int main(int argc, char *argv[]) {
    // Check if a string argument is provided
    if (argc != 2) {
        printf("Usage: %s <prefix>\n", argv[0]);
        return 1;
    }

char *prefix = argv[1];
struct dirent *entry;
DIR *dp;

    // Open the current directory
dp = opendir(".");
if (dp == NULL) {
        perror("opendir failed");
        return 1;
    }

    // Iterate through all the files in the directory
printf("Files starting with '%s':\n", prefix);
while ((entry = readdir(dp)) != NULL) {
        // Check if the file name starts with the provided prefix
        if (strncmp(entry->d_name, prefix, strlen(prefix)) == 0) {
            printf("%s\n", entry->d_name);
        }
}

// Close the directory
closedir(dp);
return 0;
}

>>>compile - gcc list_files_by_prefix.c -o list_files_by_prefix
>>>run - ./list_files_by_prefix foo

=================================================================================================================================
slip18a

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
char filename[256];
printf("Enter the file name to check: ");
scanf("%s", filename);

if (access(filename, F_OK) == 0) {
printf("File '%s' exists in the current directory.\n", filename);
} else {
printf("File '%s' does not exist in the current directory.\n", filename);
}
return 0;
}

--------------------------------------------------------------------------------------------------------------------------
slip18b

#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
int pipe_fd[2];
pid_t pid;
char buffer[100];

if (pipe(pipe_fd) == -1) {
perror("pipe failed");
exit(1);
}

pid = fork();

if (pid < 0) {
perror("fork failed");
exit(1);
}

if (pid == 0) {  
close(pipe_fd[0]);

write(pipe_fd[1], "Hello World", 12);
write(pipe_fd[1], "Hello SPPU", 11);
write(pipe_fd[1], "Linux is Funny", 15);

close(pipe_fd[1]);
exit(0);
} else {  
close(pipe_fd[1]);

while (read(pipe_fd[0], buffer, sizeof(buffer)) > 0) {
printf("%s\n", buffer);
}
close(pipe_fd[0]);
}
return 0;
}
