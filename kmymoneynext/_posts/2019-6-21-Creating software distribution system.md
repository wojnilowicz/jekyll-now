---
layout: post
title: Creating software distribution system
---

Good software distribution means the least effort for a user to try out the newest software version. Least effort means:
###### 1) No compilation from source code.
User only needs to know how to run the code and not how to compile it.
######  2) No installation of 3rd party software.
There is always a chance of setting up a wrong combination.
###### 3) Small distribution package.
Less to download means shorter waiting time.

KDE does above by running its own [Binary Factory](https://binary-factory.kde.org/). It's a build environment that through build system produces single package with a software ready to use by users of Linux, MacOS, and Windows. It's a reference for creating own build system.

----

 In order to create own build system, freely available build environments like [Travis CI](https://travis-ci.com/) and [AppVeyor](https://www.appveyor.com/) have been chosen. Travis CI has good support for Linux and MacOS but rising support for Windows, so another service called AppVeyor has been chosen for that.

The advantages are:
###### 1) flexibility
Libraries can be set up to build faster and be smaller.
###### 2) stability
No failed build periods because of unwanted changes that destabilize build environment or system.
###### 3) no personal issues
No non-technical hindrances while asking for build or set up.
###### 4) portability
Build system can be easily moved to competetive building services.

The disadvantages are:
###### 1) all needs to be done by onself
No team devoted to maintain the build system.
###### 2) time limits
Building task is time limited on the server, so it must be divided and coordinated.

----

The result of work is shown in the figure below and compared with BinaryFactory result.
[![Comparison of sizes](/images/2019-06-21 comparison.png)](/images/2019-06-21 comparison.png){: .center-image }

As one can see, there are three single files:
1. exe file for Windows Vista and later
2. AppImage file for Linux distribution technically equal to Ubuntu 16.04 and latter
3. dmg file for MacOS Sierra and later

All three are feature full. In comparison with BinaryFactory they are noticeably smaller and that's because flexibility of own build system allowed to make cuts in right places.

The files are available to download for everyone free of charge from [releases](https://github.com/wojnilowicz/kmymoneynext/releases) website.