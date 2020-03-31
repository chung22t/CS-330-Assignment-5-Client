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

Mar. 30
-server begins asking questions for client
-client begins responding to questions
-current challenge is trying to get the server and client to keep continuing to play the game or exit if the client requests to.


/* The following is the code for the client */
#include <stdio.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <string.h>
#include <iostream>
#include <vector>
using namespace std;
#define PORT 8080

int main(int argc, char const *argv[])
{
    const int MsgLen = 100; //length of message
    int sock = 0, valread;
    char  client_category; //game category
    struct sockaddr_in serv_addr;
    char *hello = "Hello from client";
    char buffer[1024] = {0};
    char AnswerBuffer[MsgLen];
    int difficulty;
    int quit;
   // vector<char> answer(MsgLen);
    string answer;
    int QuitBuffer[MsgLen] = {0};

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

    cin >> client_category;
    send(sock, &client_category, 1, 0);

   /* valread = read (sock, buffer, 1024);
    printf("%s\n", buffer);*/

    if (client_category == 's')  //if client picks science category
  {
    //clear buffer and display difficulty message for client
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);
    cout << buffer << endl;

    // send difficulty level to server
    cin >> difficulty;
    send (sock, &difficulty, MsgLen, 0);

    //receive first question from server
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);
    cout << buffer << endl;  //display question to client
    //cin >> answer;
    cin >> AnswerBuffer;

    //send answer to server
    send(sock, AnswerBuffer, sizeof(AnswerBuffer), 0);

   /* memset(AnswerBuffer, 0, MsgLen);
    recv(sock, AnswerBuffer, MsgLen, 0);  //receive quit message
    cout << AnswerBuffer << endl;  //display quit message
   */
    cout << "Quit?" << endl;
    cin >> quit;
    send(sock, &quit, MsgLen, 0); //send quit response

  }

    return 0;
}

