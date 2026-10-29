# Linux-IPC-Shared-memory
Ex06-Linux IPC-Shared-memory

# AIM:
To Write a C program that illustrates two processes communicating using shared memory.

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - Shared Memory

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## Write a C program that illustrates two processes communicating using shared memory.

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/wait.h>

int main()
{
    int shmid;
    char *str;

    shmid = shmget(IPC_PRIVATE, 1024, IPC_CREAT | 0666);

    if (shmid < 0)
    {
        perror("shmget");
        return 1;
    }

    str = (char *)shmat(shmid, NULL, 0);

    if (str == (char *)-1)
    {
        perror("shmat");
        shmctl(shmid, IPC_RMID, NULL);
        return 1;
    }

    if (fork() == 0)
    {
        strcpy(str, "Hello from child process!");
        printf("Child writes: %s\n", str);
        shmdt(str);
        exit(0);
    }
    else
    {
        wait(NULL);
        printf("Parent reads: %s\n", str);
    }

    shmdt(str);
    shmctl(shmid, IPC_RMID, NULL);

    return 0;
}
```

## OUTPUT
<img width="607" height="195" alt="image" src="https://github.com/user-attachments/assets/7dcda535-9955-455f-86e8-d8b5c51a22e3" />


# RESULT:
The program is executed successfully.
