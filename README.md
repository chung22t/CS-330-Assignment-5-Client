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

Apr. 5
-Server has asked client a category of questions.
-Server will ask client to select one of two categories (science or history).
-I am planning to add a couple more categories such as math and/or geography.
-Server will ask client for category and difficulty level. Then server will ask client quesiton.
-Server will send message to client if they are correct or incorrrect.
-Current issue right now is to get game to keep looping until client run out of lives

Apr 7
-New category (geography) has been included in the game.
-a loop has been created so client and keep playing game until they run out of lives.
-Current issue is trying to get the game to exit. Once client answers question wrong, the game on client side is in
 infinite loop even if a condition was placed on loop.



/************************Client Code**********************************************/

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
    char  difficulty;
    int quit;
    char answer;
    int LIVES = 3;

    int QuitBuffer[MsgLen] = {0};
    char NEWBUFF[1024] = {0};

   for(;;)
  {
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

/*  while (LIVES >= 0)
 {*/
    memset(buffer, 0, 1024);
    valread = read( sock , buffer, 1024);
    printf("%s\n",buffer );

    cin >> client_category;
    send(sock, &client_category, 1, 0);

   /* valread = read (sock, buffer, 1024);
    printf("%s\n", buffer);*/


  /********************************Science***********************************/
    if (client_category == 's')  //if client picks science category
  {
    //clear buffer and display difficulty message for client
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);
    cout << buffer << endl;

    // send difficulty level to server
    cin >> difficulty;
    send (sock, &difficulty, 1, 0);

    if (difficulty == 'a')  //if client selects difficulty a)
   {
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);  //server sends question to client
    cout << buffer << endl;
    cin >> answer;  //client answers question
    send (sock, &answer, 1, 0); //sends answer to server

    if (answer == 'c')  //if correct
    {
      memset(buffer, 0, 1024);
      recv(sock, buffer, MsgLen, 0);
      cout << buffer << endl;
    }
    else
    {
     LIVES--;
     memset(buffer, 0, 1024);
     recv(sock, buffer, MsgLen, 0);
     cout << buffer << endl;
    }
   }
   else if (difficulty == 'b')   //if client selects difficulty b)
   {
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);  //server sends question to client
    cout << buffer << endl;
    cin >> answer;  //client answers question
    send (sock, &answer, 1, 0); //sends answer to server

    if (answer == 'a')
    {
      memset(buffer, 0, 1024);
      recv(sock, buffer, MsgLen, 0);
      cout << buffer << endl;
    }
    else
    {
     LIVES--;
     memset(buffer, 0, 1024);
     recv(sock, buffer, MsgLen, 0);
     cout << buffer << endl;
    }
   }
   else if (difficulty == 'c')  //if client selects difficulty c)
   {
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);  //server sends question to client
    cout << buffer << endl;
    cin >> answer;  //client answers question
    send (sock, &answer, 1, 0); //sends answer to server

    if (answer == 'b')
    {
      memset(buffer, 0, 1024);
      recv(sock, buffer, MsgLen, 0);
      cout << buffer << endl;
    }
    else
    {
     LIVES--;
     memset(buffer, 0, 1024);
     recv(sock, buffer, MsgLen, 0);
     cout << buffer << endl;
    }
   }
 }
  /*****************************History*******************************/
 else if (client_category == 'h')  //if client picks history category
  {
    //clear buffer and display difficulty message for client
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);
    cout << buffer << endl;

    // send difficulty level to server
    cin >> difficulty;
    send (sock, &difficulty, 1, 0);

    if (difficulty == 'a')  //if client selects difficulty a)
   {
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);  //server sends question to client
    cout << buffer << endl;
    cin >> answer;  //client answers question
    send (sock, &answer, 1, 0); //sends answer to server

    if (answer == 'b')
    {
      memset(buffer, 0, 1024);
      recv(sock, buffer, MsgLen, 0);
      cout << buffer << endl;
    }
    else
    {
     LIVES--;
     memset(buffer, 0, 1024);
     recv(sock, buffer, MsgLen, 0);
     cout << buffer << endl;
    }
   }
   else if (difficulty == 'b')   //if client selects difficulty b)
   {
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);  //server sends question to client
    cout << buffer << endl;
    cin >> answer;  //client answers question
    send (sock, &answer, 1, 0); //sends answer to server

    if (answer == 'c')
    {
      memset(buffer, 0, 1024);
      recv(sock, buffer, MsgLen, 0);
      cout << buffer << endl;
    }
    else
    {
     LIVES--;
     memset(buffer, 0, 1024);
     recv(sock, buffer, MsgLen, 0);
     cout << buffer << endl;
    }
   }
   else if (difficulty == 'c')  //if client selects difficulty c)
   {
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);  //server sends question to client
    cout << buffer << endl;
    cin >> answer;  //client answers question
    send (sock, &answer, 1, 0); //sends answer to server

    if (answer == 'a')
    {
      memset(buffer, 0, 1024);
      recv(sock, buffer, MsgLen, 0);
      cout << buffer << endl;
    }
    else
    {
     LIVES--;
     memset(buffer, 0, 1024);
     recv(sock, buffer, MsgLen, 0);
     cout << buffer << endl;
    }
   }
 }
 /***********************************Geography******************************/
 else if (client_category == 'g')
 {
  //clear buffer and display difficulty message for client
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);
    cout << buffer << endl;

    // send difficulty level to server
    cin >> difficulty;
    send (sock, &difficulty, 1, 0);

    if (difficulty == 'a')  //if client selects difficulty a)
   {
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);  //server sends question to client
    cout << buffer << endl;
    cin >> answer;  //client answers question
    send (sock, &answer, 1, 0); //sends answer to server

    if (answer == 'b')  //answer is correct
    {
      memset(buffer, 0, 1024);
      recv(sock, buffer, MsgLen, 0);
      cout << buffer << endl;
    }
    else
    {
     LIVES--;
     memset(buffer, 0, 1024);
     recv(sock, buffer, MsgLen, 0);
     cout << buffer << endl;
    }
   }
   else if (difficulty == 'b')   //if client selects difficulty b)
   {
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);  //server sends question to client
    cout << buffer << endl;
    cin >> answer;  //client answers question
    send (sock, &answer, 1, 0); //sends answer to server

    if (answer == 'a')
    {
      memset(buffer, 0, 1024);
      recv(sock, buffer, MsgLen, 0);
      cout << buffer << endl;
    }
    else
    {
     LIVES--;
     memset(buffer, 0, 1024);
     recv(sock, buffer, MsgLen, 0);
     cout << buffer << endl;
    }
   }
   else if (difficulty == 'c')  //if client selects difficulty c)
   {
    memset(buffer, 0, 1024);
    recv(sock, buffer, MsgLen, 0);  //server sends question to client
    cout << buffer << endl;
    cin >> answer;  //client answers question
    send (sock, &answer, 1, 0); //sends answer to server

    if (answer == 'c')
    {
      memset(buffer, 0, 1024);
      recv(sock, buffer, MsgLen, 0);
      cout << buffer << endl;
    }
    else
    {
     LIVES--;
     memset(buffer, 0, 1024);
     recv(sock, buffer, MsgLen, 0);
     cout << buffer << endl;
    }
   }
  else
  {
    cout << "Incorrect input!" << endl;
    return 1;
  }
 } //end of if statement

 if (LIVES <= 0)
 {
   cout << "You ran out of lives" << endl;
   return 1;
 }
}

close (sock);
//}
    return 0;
}

