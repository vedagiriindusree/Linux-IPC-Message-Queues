# Linux-IPC-Message-Queues
Linux IPC-Message Queues

# AIM:
To write a C program that receives a message from message queue and display them

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:
DEVELOPED BY:VEDAGIRI INDU SREE
REG.NO:212223230236
## C program that receives a message from message queue and display them
```
// msqueue.c - Combined Writer/Reader for System V Message Queue
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/ipc.h>
#include <sys/msg.h>

struct mesg_buffer {
    long mesg_type;
    char mesg_text[100];
} message;

int main(int argc, char *argv[]) {
    key_t key;
    int msgid;

    if (argc != 2) {
        printf("Usage: %s writer|reader\n", argv[0]);
        return 1;
    }

    key = ftok("msgq.c", 65);
    if (key == -1) {
        perror("ftok");
        return 1;
    }

    msgid = msgget(key, 0666 | IPC_CREAT);
    if (msgid == -1) {
        perror("msgget");
        return 1;
    }

    if (strcmp(argv[1], "writer") == 0) {
        message.mesg_type = 1;
        printf("Enter Message: ");
        fgets(message.mesg_text, sizeof(message.mesg_text), stdin);
        message.mesg_text[strcspn(message.mesg_text, "\n")] = 0; // remove newline
        if (msgsnd(msgid, &message, sizeof(message), 0) == -1) {
            perror("msgsnd");
            return 1;
        }
        printf("Message sent: %s\n", message.mesg_text);
    } else if (strcmp(argv[1], "reader") == 0) {
        if (msgrcv(msgid, &message, sizeof(message), 1, 0) == -1) {
            perror("msgrcv");
            return 1;
        }
        printf("Message received: %s\n", message.mesg_text);
        msgctl(msgid, IPC_RMID, NULL); // destroy queue
    } else {
        printf("Invalid argument. Use writer or reader.\n");
        return 1;
    }

    return 0;
}
```
## OUTPUT

![WhatsApp Image 2025-05-07 at 15 13 23_6cad9357](https://github.com/user-attachments/assets/ee74b91b-b1c7-42da-b19c-b19ad559d3d9)

# RESULT:
The programs are executed successfully.
