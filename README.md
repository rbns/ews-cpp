# README

EWS is an API that third-party programmers can use to communicate with
Microsoft Exchange Server. The API exists since Exchange Server 2007 and is
continuously up-dated by Microsoft and present in the latest iteration of the
product, Exchange Server 2016.

This library provides a native and platform-independent way to use EWS in your
C++ application.


<img src="ews-overview.png" width=80%>


## Supported Compilers

* Visual Studio 2012
* Visual Studio 2013
* Visual Studio 2015
* Clang 3.5 with libc++
* GCC 4.8 with libstdc++


## Supported Operating Systems

* Microsoft Windows 8.1 and Windows 10
* Mac OS X 10.10
* RHEL 7
* Ubuntu 14.04 LTS (x86_64 and i386)
* SLES12


## Supported Microsoft Exchange Server Versions

* Microsoft Exchange Server 2013 SP1


## Run-time Dependencies

* libcurl, at least version 7.22


## Note Windows Users

You can obtain an up-to-date and easy-to-use binary distribution of libcurl
from here: [confusedbycode.com/curl](http://www.confusedbycode.com/curl/)

Additionally, you probably need to tell CMake where to find it. Just set
`CMAKE_PREFIX_PATH` to the path where you installed libcurl (e.g.
``C:\Program Files\cURL``) and re-configure.


## Source Code

ews-cpp's source code is available as a Git repository. To obtain it, type:

```bash
git clone git@gitlab.otris.de:kircher/ews-cpp.git
cd ews-cpp/
git submodule init
git submodule update
```


## Building

The library is header-only. So there is no need to build anything. Just copy the
`include/ews/` directory wherever you may like.

To build the accompanied tests with debugging symbols and address sanitizer
enabled do something like this

```bash
cmake -DCMAKE_BUILD_TYPE=Debug -DENABLE_ASAN=ON /path/to/source
make
```

Type `make help` to see more configuration options.


## API Docs

Use the `doc` target to create the API documentation with Doxygen.  Type:

```bash
make doc
open html/index.html
```


## Test Suite

Export the following environment variables in order to run individual examples
or the test suite:

```bash
EWS_TEST_DOMAIN
EWS_TEST_USERNAME
EWS_TEST_PASSWORD
EWS_TEST_URI
```

Once you've build the project, you can run the test suite with:

```bash
./tests
```


## Design Notes

ews-cpp is written in a "modern C++" way:

* C++ Standard Library, augmented with rapidxml for XML parsing
* Smart pointers instead of raw pointers
* Pervasive RAII idiom
* Handle errors using C++ exceptions
* Coding conventions inspired by Boost


## API

Just add:

```c++
#include <ews/ews.hpp>
```

to your include directives and you are good to go.

Take a look at the `examples/` directory to get an idea of how the API feels.
ews-cpp is a thin wrapper around Microsoft's EWS API. You will need to refer to
the [EWS reference for
Exchange](https://msdn.microsoft.com/en-us/library/office/bb204119%28v=exchg.150%29.aspx)
for all available parameters to pass and all available attributes on items.
From 10.000ft it looks like this:


<img src="ews-objects.png" width=80%>


You have items and you have **the** service. You use the service whenever you
want to talk to the Exchange server.

Please note one important caveat though. ews-cpp's API is designed to be
"blocking". This means whenever you call one of the service's member functions
to talk to an Exchange server that call blocks until it receives a request from
the server. And that may, well, just take forever (actually until a timeout is
reached). You need to keep this in mind in order to not block your main thread.

Implications of this design choice

Pros:

* A blocking API is much easier to use and understand

Cons:

* You just might accidentally block your UI thread
* You cannot issue thousands of EWS requests asynchronously simply because you
  cannot spawn thousands of threads in your process. You may need additional
  effort here


<!-- vim: et sw=4 ts=4:
-->
