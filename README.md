Instruction on how to build and run the application
The codebase contains a run.sh file in the root directory of the project. Please change its access mode using chmod +x run.sh command so that it can be run as an executable. On running this shell script, it will build a docker image, then build the source code (using make build cmd) and then execute the match_engine binary. As mentioned in the Runtime Requirements section of the assignment, the run.sh script is capable of accepting piped input. A sample_input.txt file is also present in the root directory of the project.

Sample run:

virtual-machine:~/gemini/GeminiMatchingEngine$ cat sample_input.txt | ./run.sh

Please refer https://stackoverflow.com/questions/47854463/docker-got-permission-denied-while-trying-to-connect-to-the-docker-daemon-socke in case you face any docker permission related issue like the following docker: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.

The above issue can be fixed by running the following commands: virtual-machine:/gemini/GeminiMatchingEngine$ sudo usermod -a -G docker [your_user] virtual-machine:/gemini/GeminiMatchingEngine$ newgrp docker

Instruction on how to build and run the tests
The codebase contains a test.sh file in the root directory of the project. Please change its access mode using chmod +x test.sh command so that it can be run as an executable. On running this shell script, it will build a docker image, then build the unit tests (using make test cmd) and then execute the match_engine_test binary which will run all the unit tests.

Sample run:

virtual-machine:~/gemini/GeminiMatchingEngine$ ./test.sh
How I approached the problem?
I divided the entire problem statement into 3 core components, MatchingEngine, OrderBook and PriceBucket.
The PriceBucket consists of a list of orders at a particular price. It provides APIs to execute/fill the order, access the number of orders and total volume at a particular price in the order book at any given time.
The OrderBook maintains the buy and sell order books separately. Both the books are a map of price to PriceBucket. The buy order book map is sorted in decreasing order of price and the sell order book is sorted in increasing order of price. Whenever an order is added in the OrderBook it first tries to execute the order by checking the counter book. It iterates over the each price point in the counter book map and if the spread is crossed it tries to fill this order with the list of orders present in the PriceBucket of the counter order book. If after fill any quantity is left, this order is added in its corresponding side order book.
PLEASE NOTE THAT THE OrderBook ONLY HANDLES LIMIT ORDERS. IT DOES NOT HANDLE MARKET ORDERS (that is the orders with price = 0), AS THERE WAS NOTHING MENTIONED IN THE ASSIGNMENT ABOUT HOW TO HANDLE MARKET ORDERS AND WHAT TO DO IF THE MARKET ORDER EMPTIES THE ENTIRE COUNTER BOOK WITH ITS QUANTITY STILL REMAINING TO BE FILLED. I DID NOT MAKE ANY ASSUMPTIONS AROUND IT AND HENCE LEFT THE PART TO HANDLE MARKET ORDERS.
MatchingEngine maintains a map of symbol to OrderBook, that is the OrderBook is maintained per symbol. Whenever an order is added in the matching engine it will add the order in the OrderBook of the symbol for which this order was placed. It also exposes API to access all the remaining orders in the order of arrival time.
Time spent on this project
6 hours












































# GoogleTest

### Announcements

#### Live at Head

GoogleTest now follows the
[Abseil Live at Head philosophy](https://abseil.io/about/philosophy#upgrade-support).
We recommend
[updating to the latest commit in the `main` branch as often as possible](https://github.com/abseil/abseil-cpp/blob/master/FAQ.md#what-is-live-at-head-and-how-do-i-do-it).

#### Documentation Updates

Our documentation is now live on GitHub Pages at
https://google.github.io/googletest/. We recommend browsing the documentation on
GitHub Pages rather than directly in the repository.

#### Release 1.12.1

[Release 1.12.1](https://github.com/google/googletest/releases/tag/release-1.12.1)
is now available.

The 1.12.x branch will be the last to support C++11. Future releases will
require at least C++14.

#### Coming Soon

*   We are planning to take a dependency on
    [Abseil](https://github.com/abseil/abseil-cpp).
*   More documentation improvements are planned.

## Welcome to **GoogleTest**, Google's C++ test framework!

This repository is a merger of the formerly separate GoogleTest and GoogleMock
projects. These were so closely related that it makes sense to maintain and
release them together.

### Getting Started

See the [GoogleTest User's Guide](https://google.github.io/googletest/) for
documentation. We recommend starting with the
[GoogleTest Primer](https://google.github.io/googletest/primer.html).

More information about building GoogleTest can be found at
[googletest/README.md](googletest/README.md).

## Features

*   An [xUnit](https://en.wikipedia.org/wiki/XUnit) test framework.
*   Test discovery.
*   A rich set of assertions.
*   User-defined assertions.
*   Death tests.
*   Fatal and non-fatal failures.
*   Value-parameterized tests.
*   Type-parameterized tests.
*   Various options for running the tests.
*   XML test report generation.

## Supported Platforms

GoogleTest follows Google's
[Foundational C++ Support Policy](https://opensource.google/documentation/policies/cplusplus-support).
See
[this table](https://github.com/google/oss-policies-info/blob/main/foundational-cxx-support-matrix.md)
for a list of currently supported versions compilers, platforms, and build
tools.

## Who Is Using GoogleTest?

In addition to many internal projects at Google, GoogleTest is also used by the
following notable projects:

*   The [Chromium projects](http://www.chromium.org/) (behind the Chrome browser
    and Chrome OS).
*   The [LLVM](http://llvm.org/) compiler.
*   [Protocol Buffers](https://github.com/google/protobuf), Google's data
    interchange format.
*   The [OpenCV](http://opencv.org/) computer vision library.

## Related Open Source Projects

[GTest Runner](https://github.com/nholthaus/gtest-runner) is a Qt5 based
automated test-runner and Graphical User Interface with powerful features for
Windows and Linux platforms.

[GoogleTest UI](https://github.com/ospector/gtest-gbar) is a test runner that
runs your test binary, allows you to track its progress via a progress bar, and
displays a list of test failures. Clicking on one shows failure text. GoogleTest
UI is written in C#.

[GTest TAP Listener](https://github.com/kinow/gtest-tap-listener) is an event
listener for GoogleTest that implements the
[TAP protocol](https://en.wikipedia.org/wiki/Test_Anything_Protocol) for test
result output. If your test runner understands TAP, you may find it useful.

[gtest-parallel](https://github.com/google/gtest-parallel) is a test runner that
runs tests from your binary in parallel to provide significant speed-up.

[GoogleTest Adapter](https://marketplace.visualstudio.com/items?itemName=DavidSchuldenfrei.gtest-adapter)
is a VS Code extension allowing to view GoogleTest in a tree view and run/debug
your tests.

[C++ TestMate](https://github.com/matepek/vscode-catch2-test-adapter) is a VS
Code extension allowing to view GoogleTest in a tree view and run/debug your
tests.

[Cornichon](https://pypi.org/project/cornichon/) is a small Gherkin DSL parser
that generates stub code for GoogleTest.

## Contributing Changes

Please read
[`CONTRIBUTING.md`](https://github.com/google/googletest/blob/main/CONTRIBUTING.md)
for details on how to contribute to this project.

Happy testing!
