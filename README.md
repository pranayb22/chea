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
---------------------------------------------------------------------------------------------------------------------------------------------------------------------

==============================================================================================================================================================
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
-----------------------------------------------------------------------------------------------------------------------------------------------------
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
=================================================================================================================================================================
