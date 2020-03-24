# CS-330-Assignment-5-Client
This is the program code for the client side of a socket

/*
The goal of this assignment is socket communication between a server and a client.
   This program is to resemble a jeopardy game. A client will connect to the server (game master). The client will get to pick
   what category and what difficulty (harder questions reward nore points). The player will only have 3 lives. that is if they make 
   more than 3 mistakes, it is game over.
   
   
Mar. 23
-included information about the game -server will ask client (player) for category and what difficulty 
-client messages for category & difficulty received by server

next step is to continue this process for the other categories and begin writing the questions of the game. */


/* The following is the code for the client */
#include <stdio.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <string.h>
#define PORT 8080

int main(int argc, char const *argv[])
{
    int sock = 0, valread;
    struct sockaddr_in serv_addr;
    char *hello = "Hello from client";

    char *client_category;

    char buffer[1024] = {0};
    if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0)
    {
        printf("\n Socket creation error \n");
        return -1;
    }

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(PORT);

    // Convert IPv4 and IPv6 addresses from text to binary form
    if(inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr)<=0)
    {
        printf("\nInvalid address/ Address not supported \n");
        return -1;
    }

    if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0)
    {
        printf("\nConnection Failed \n");
        return -1;
    }
    send(sock , hello , strlen(hello) , 0 );
    printf("Hello message sent\n");
    valread = read( sock , buffer, 1024);
    printf("%s\n",buffer );

    //server asks client what category they would like to play


    return 0;
}

