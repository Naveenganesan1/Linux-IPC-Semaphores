# Linux-IPC-Semaphores
Ex05-Linux IPC-Semaphores

# AIM:
To Write a C program that implements a producer-consumer system with two processes using Semaphores.

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - Sempahores

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## Write a C program that implements a producer-consumer system with two processes using Semaphores.
~~~
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<time.h>
#include<sys/types.h>
#include<sys/ipc.h>
#include<sys/sem.h>
#define NUM_LOOPS 20
#if defined(_GNU_LIBRARY_) && !defined(_SEM_SEMUN_UNDEFINED)
#else
union semun
{
int val;
struct semid_ds *buf;
unsigned short int *array;
struct seminfo *__buf;
};
#endif
int main(int argc,char* argv[])
{
int sem_set_id;
union semun sem_val;
int child_pid;
int i;
struct sembuf sem_op;
int rc;
struct timespec delay;
sem_set_id=semget(IPC_PRIVATE,1,0600);
if(sem_set_id==-1)
{
perror("main: segmet");
exit(1);
}
printf("semaphore set created,semaphore set id '%d'.\n",sem_set_id);
sem_val.val=0;
rc=semctl(sem_set_id,0,SETVAL,sem_val);
child_pid=fork();
switch(child_pid)
{
case -1:
perror("fork");
exit(1);
case 0:
for(i=0;i<NUM_LOOPS;i++)
{
sem_op.sem_num=0;
sem_op.sem_op=-1;
sem_op.sem_flg=0;
semop(sem_set_id,&sem_op,1);
printf("consumer:'%d'\n",i);
fflush(stdout);
}
break;
default:
for(i=0;i<NUM_LOOPS;i++)
{
printf("producer:'%d'\n",i);
fflush(stdout);
sem_op.sem_num=0;
sem_op.sem_op=1;
sem_op.sem_flg=0;
semop(sem_set_id,&sem_op,1);
if(rand()>3*(RAND_MAX/4))
{
delay.tv_sec=0;
delay.tv_nsec=10;
//nanosleep(&delay,NULL);
sleep(10);
}
if(NUM_LOOPS>=10)
{
semctl(sem_set_id,0,IPC_RMID,sem_val);
}}
break;
}       
return 0;
}
~~~




## OUTPUT
$ ./sem.o \



![375743687-183ac9d3-b8c5-4bac-b0f3-7b1813e3e53b](https://github.com/user-attachments/assets/b8f04650-4fea-4a4f-9fcb-2d337d34ba80)

![375743800-d58575fe-55f8-4d66-8c7a-18f6a9e754b8](https://github.com/user-attachments/assets/1af3a0fe-5930-4d68-8f97-fd768a36525e)



$ ipcs
![375743922-7a6d8b89-b4fa-4e98-b304-5a05ba020b06](https://github.com/user-attachments/assets/ab3d28d9-f996-45f2-aad1-eced0d19a67b)





# RESULT:
The program is executed successfully.
