# Overview
Here's an explanation of sockets and how they work for internet, LAN, and inter-process communication in Linux:
[Getting Started with Networking and Sockets](https://www.kungfudev.com/blog/2024/06/07/getting-started-with-net-and-sockets)
### Internet Communication via Sockets

When sockets communicate over the internet, they typically use network sockets that follow the TCP (Transmission Control Protocol) model [4]. Here are the key points to understand:

- Network sockets use IP (Internet Protocol) to encapsulate and handle sending and receiving data between different networks or systems [4].
- They rely on socket addresses that consist of a transport protocol (like TCP or UDP), an IP address, and a port number [4].
- TCP-based stream sockets are connection-oriented, ensuring ordered delivery of packets [4].
- The socket API provides a standardized way for applications to interact with network connections without worrying about the underlying protocols [3].

Example of an internet socket address:
203.0.113.1:80 (for HTTP on port 80)

### LAN Communication via Sockets

For Local Area Network (LAN) communication, sockets often use Unix Domain Sockets [3][4]. Here are the important aspects:

- Unix Domain Sockets use local files to send and receive data instead of network interfaces and IP packets [4].
- They appear as special file descriptors in the filesystem [3].
- These sockets don't use IP addresses but rather use pathnames to identify the communication endpoint [3].
- They are more efficient than network sockets for inter-process communication on the same machine [3].

Example of a Unix Domain Socket:
/tmp/my_socket

### Inter-Process Communication in Linux via Sockets

For communication between processes within the same system or across different systems, various socket types can be used depending on the specific requirements [2][3]. Here are some key points:

- TCP and UDP network sockets can be used for process-to-process communication, especially when dealing with distributed systems [2].
- Unix Domain Sockets are often preferred for local inter-process communication due to their efficiency and special features like file descriptor passing [2][3].
- The programming model for these socket types is similar (both being bidirectional streams of data), making it easier to switch between them if needed [2].

When choosing between TCP and Unix Domain Sockets for inter-process communication:

- TCP provides more flexibility for distributed systems but has slightly higher overhead [2].
- Unix Domain Sockets are generally faster for local communication but have limited scope to other machines [2].

In summary, sockets provide a versatile mechanism for process-to-process communication, whether over the internet, LAN, or locally on a system. The choice of socket type depends on factors such as the communication requirements, network boundaries, and performance considerations.

Citations:
[1] https://www.quora.com/What-is-the-difference-between-the-socket-and-the-process
[2] https://stackoverflow.com/questions/41447064/communication-between-processes-tcp-vs-unix-sockets-ipc-vs-nats
[3] https://unix.stackexchange.com/questions/236983/differences-between-unix-domain-sockets-and-network-sockets
[4] https://www.digitalocean.com/community/tutorials/understanding-sockets
[5] https://www.reddit.com/r/eli5_programming/comments/15wpdh0/can_someone_explain_how_sockets_really_work/

[7] https://serverfault.com/questions/229259/differences-between-unix-domain-sockets-and-internet-sockets
[8] https://takethenotes.com/socket/
[9] https://serverfault.com/questions/195328/unix-socket-vs-tcp-ip-hostport
[10] https://opensource.com/article/19/4/interprocess-communication-linux-networking