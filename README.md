easywsclient
============

Forked, includes a fix where the connection is established with one call to ::send
This is somehow needed for the network in which i tested it, because the server would simply shutdown the TCP while sending dingle lines.
Thus, this fix

Before:
```c++
for(auto& line : HtmlLines)
  ::send(line)
```

Now:
```c++
std::string allLines;
for(auto& line : HtmlLines)
  allLines += line
::send(allLines);
```

Usage
=====

```c++
#include "easywsclient.hpp"
//#include "easywsclient.cpp" // <-- include only if you don't want compile separately

int
main()
{
    ...
    using easywsclient::WebSocket;
    WebSocket::pointer ws = WebSocket::from_url("ws://localhost:8126/foo");
    assert(ws);
    while (true) {
        ws->poll();
        ws->send("hello");
        ws->dispatch(handle_message);
        // ...do more stuff...
    }
    ...
    delete ws; // alternatively, use unique_ptr<> if you have C++11
    return 0;
}
```
