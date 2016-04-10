# unixODBC #

unixODBC is an open-source ODBC-library that you can run on non-Windows platforms. It is the glue between odbc (this project) and your SQL driver.
This page tells you how to install unixODBC so it can be used with odbc.

## Version matters! ##
If you want to use odbc with unixODBC make sure you are running unixODBC 2.3.1!


_as of the time of this writing, Oct-13, MacPorts is using 2.3.1_

## Installing unixODBC & FreeTDS using MacPorts ##

A few prerequisites will need to be satisfied before we can perform the install.

**XCode** --- Not every version of OS X has the necessary components to perform the below builds, so you'll need to make sure to have XCode installed first. This can be done from within the App Store under developer tools.

**Apple's Command Line Developer Tools** --- As of version 4.3 of XCode this package is not longer installed by default. From XCode's preferences panel select the `Downloads` tab. Underneath the `Components` section click the down arrow to download, and install, this package.

**MacPorts** --- Follow the instructions from the MacPorts website _https://www.macports.org/_


We'll install the FreeTDS database driver with unixODBC.
Microsoft provides an additional driver for MSSQL Server, but it is only available for Linux systems _(Red Hat, Suse)_.

From the terminal enter one of the following commands.

FreeTDS and unixODBC
```
sudo port install freetds +odbc +universal
```

Same as above with Microsoft SQL Server support
```
sudo port install freetds +mssql +odbc +universal
```