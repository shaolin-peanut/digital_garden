---
type: #project 
aliases:
---
### Desired End State
### Desired End Date
#### Journal
- What is an irc server?
	- IRC is a internet relay chat protocol. There is a specification to follow to write servers, clients, and other irc-related tools, so that they may be compatible. If you don't follow the spec you're out of the network, you're not irc, basically. That's the same with webservers for example, follow the specs so that your webapps are accepted/accessible by the browser.
	- Features include (these are the main ones we have to implement)
		- Creating users, with nicknames, passwords, etc
		- Operators, which are users that have administrative rights in a channel
		- Messaging to channels (forwards to every client that joined the channel), creating channels, changing the topic
		- Commands in the `/CMD` format, which enable different actions
			- for operators only
				- /KICK (to implement)
				- /MODE
				- /INVITE
				- /TOPIC
			- for all
- What are the basic tasks of an irc server?
	- define the protocol = a.k.a what messages can be sent, received, etc
	- implement the server logic
		- handle incoming client connections
			- that's what ldominiq has done in some way
		- authenticate clients
			- How is client authentication working?
		- manage channels and messages
			- Which commands do we have to implement, and how are we going to manage them in general?
	- concurrency (has to handle multiple client connections at the same time)
- Automated testing
	- I want to set up automated [[testing (code)]]. I will use catch2 because it's compatible with cmake and clion, and easy to setup apparently.
		- If I want to test with irssi, I have to write a bash testing script, don't want to do this for now
		- I can code up a client, it's actually simpler and this way I can test all sorts of specific scenarios. Chatgpt's advice
			- Create client socket with `socket`
			- send messages to server (connection procedure, emulate irssi) in irc format
			- receive msgs from server with `recv`
			- Connect and join channel sequence code (footnote 1)
#### Backlog
### Sprints
1. Sprint 1
	1. [[ft_irc - SPRINT 1]]
### Footnotes
footnote 1
```
#include <iostream>
#include <string>
#include <cstring>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>

int main() {
int sockfd = socket(AF_INET, SOCK_STREAM, 0);
struct sockaddr_in servaddr;
bzero(&servaddr, sizeof(servaddr));
servaddr.sin_family = AF_INET;
servaddr.sin_port = htons(6667);
inet_pton(AF_INET, "127.0.0.1", &servaddr.sin_addr);

connect(sockfd, (struct sockaddr*)&servaddr, sizeof(servaddr));

char recvline[1024];
memset(recvline, 0, sizeof(recvline));
ssize_t n = recv(sockfd, recvline, sizeof(recvline), 0);
std::cout << recvline << std::endl;

std::string nick = "testuser";
std::string user = "testuser";
std::string realname = "Test User";
std::string channel = "#test";

std::string msg = "NICK " + nick + "\r\n";
send(sockfd, msg.c_str(), msg.length(), 0);
msg = "USER " + user + " 0 * :" + realname + "\r\n";
send(sockfd, msg.c_str(), msg.length(), 0);
msg = "JOIN " + channel + "\r\n";
send(sockfd, msg.c_str(), msg.length(), 0);

while (true) {
	memset(recvline, 0, sizeof(recvline));
	n = recv(sockfd, recvline, sizeof(recvline), 0);
	std::cout << recvline << std::endl;
	if (n <= 0) {
					break;
				}
			}
		
			close(sockfd);
		
			return 0;
		}
```