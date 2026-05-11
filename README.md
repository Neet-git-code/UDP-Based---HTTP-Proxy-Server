🌐 UDP-Based HTTP Proxy Server
A Computer Networks course project built in C++ that implements a fully functional HTTP proxy server using UDP sockets from scratch.
Most proxy servers use TCP. This one intentionally uses UDP between the client and proxy to demonstrate unreliable transport — and then solves that unreliability by implementing a custom reliability layer on top, the same way real-world protocols like QUIC and game networking engines do.

✨ Features

📡 UDP socket communication between client and proxy
🔁 Custom reliability layer — sequence numbers, ACKs, retransmission on timeout
🌍 HTTP GET request parsing — extracts host, path, and port from raw HTTP text
⚡ In-memory caching — serves repeated requests instantly with CACHE HIT / CACHE MISS logs
🚫 Website blocking — blocks facebook.com, instagram.com, tiktok.com out of the box
🧵 Multithreading — handles multiple clients simultaneously using std::thread
🔒 Thread-safe cache — mutex-protected shared cache across all threads
📋 Detailed terminal logs — every packet, cache event, and blocked request is logged


🏗️ Architecture
Client ──UDP──► Proxy Server ──TCP──► Real Web Server
                     │
                Cache Manager

Client ↔ Proxy — UDP with custom reliability (seq numbers + ACKs)
Proxy ↔ Web Server — standard TCP (as HTTP requires)
Cache — in-memory hash map shared across threads


🛠️ Tech Stack

Language — C++17
Sockets — POSIX BSD sockets (sys/socket.h)
Transport — UDP (SOCK_DGRAM) + TCP (SOCK_STREAM)
Concurrency — std::thread, std::mutex
Build — Makefile + g++
Platform — Linux / macOS


📁 Project Structure
├── main.cpp            # Entry point
├── proxy_server.h      # Shared header: PacketHeader, constants
├── proxy_server.cpp    # Core proxy logic, threading, UDP socket
├── client.cpp          # UDP client with reliability layer
├── parser.h / .cpp     # HTTP GET request parser
├── cache_manager.h / .cpp  # Thread-safe in-memory cache
├── Makefile            # One-command build
└── README.txt          # Full documentation

🚀 Quick Start
bash# Build
make

# Terminal 1 — start proxy
./proxy 8888

# Terminal 2 — send a request
./client 127.0.0.1 8888 http://example.com/

# Test caching (run same URL twice)
./client 127.0.0.1 8888 http://example.com/   # CACHE MISS
./client 127.0.0.1 8888 http://example.com/   # CACHE HIT

# Test blocking
./client 127.0.0.1 8888 http://facebook.com/

📚 Networking Concepts Covered
ConceptImplementationUDP vs TCPClient↔Proxy uses UDP, Proxy↔Server uses TCPReliability over UDPSequence numbers, ACKs, retransmissionHTTP ParsingExtract host, path, port from raw GET requestCachingIn-memory hash map with HIT/MISS loggingMultithreadingstd::thread per client, mutex-protected cacheWebsite BlockingHardcoded blocklist checked before forwardingNetwork Byte Orderhtonl() / ntohl() for cross-platform integersDNS Resolutiongetaddrinfo() to resolve hostnames to IPs

⚠️ Limitations

HTTP only — HTTPS/TLS not implemented
GET requests only — no POST, PUT, DELETE
Cache has no TTL — entries persist until proxy restarts
POSIX only — does not run natively on Windows (use WSL2)


Built as part of a Computer Networks course to demonstrate socket programming, transport layer protocol design, and network application architecture.
