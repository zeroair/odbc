# Overview #
There are two major drivers that you can use to connect to a Microsoft SQL Server from Linux. They are:
  * FreeTDS ([how to install](InstallingFreeTDS.md))
  * Microsoft ODBC Driver 11 for SQL Server on Linux ([how to install](InstallingMicrosoftDriverOnDebianLinux.md))

Oh yes, Microsoft has published its own driver for the Linux platform. While it is only targeted at Red Hat, it can be installed on Debian relatively easily. It supports a few advanced features (MARS for example) that FreeTDS doesn't, but it is closed source and it will only run on x86 (32 and 64-bit) platforms.

FreeTDS on the other hand is an open source driver for TDS servers (Microsoft and Sybase) and should run on any platform.

You can expect better support for FreeTDS but slightly more features from Microsoft.

You can choose yourself which driver you want to run. You can even install both and have them run in parallel (thus is the power of unixODBC).

# Installing the driver(s) #
[Installing FreeTDS](InstallingFreeTDS.md)

[Installing the Microsoft driver](InstallingMicrosoftDriverOnDebianLinux.md)