# TorPlusPlus
C++ library for talking to a TOR hidden service.
## You need to manually do this:
Requires the "tor" folder from the "Tor Expert Bundle" to be placed next to the executable.
(This file can be downloaded from https://www.torproject.org/download/tor/)


I might make some kind of script to automate doing this, I dont want to just place the files in here as they will become outdated.

# Platform support
Currently, I have this header set up for windows exclusively. Will add linux support in the future™

# Example
The following example code sends a HTTP GET request to "CryptBB", and prints the result.
```c++
#include <string>
#include <stdio.h>

#include "torplusplus.hpp"

#define TEST_ONION "cryptbbtg65gibadeeo2awe3j7s6evg7eklserehqr4w4e2bis5tebid.onion"

int main(){
    torSocketGlobals::DEBUG = true; // Enable debug messages
    torSocket torSock; // Create a torSocket object. Doing this starts the TOR proxy and connects to it
    torSock.connectTo(TEST_ONION); // Connect the proxy to the onion address
    std::string httpReq = "GET / HTTP/1.1\r\nHost: " + std::string(TEST_ONION) + "\r\n\r\n"; // Assemble a request to send to the site
    torSock.proxySend(httpReq.c_str(), (int)httpReq.length()); // Send the request to the hidden service
    char buf[16384] = {0}; // Up to 16KB of memory for whatever gets sent back
    torSock.proxyRecv(buf, sizeof(buf) * sizeof(char)); // Receive a response to the GET request
    printf("%s\n", buf); // Print whatever the server sent back
    return 0;
}
```

# ⚠️ Security ⚠️
⚠️ Absolutely no guarantees on security/anonymity when using this code to talk to TOR ⚠️
