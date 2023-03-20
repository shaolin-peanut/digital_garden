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
#### Backlog
### Sprints
1. Sprint 1
	1. [[ft_irc - SPRINT 1]]