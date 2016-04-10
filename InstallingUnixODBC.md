# unixODBC #

unixODBC is an open-source ODBC-library that you can run on non-Windows platforms. It is the glue between odbc (this project) and your SQL driver.
This page tells you how to install unixODBC so it can be used with odbc.

## Version matters! ##
If you want to use odbc with unixODBC make sure you are running unixODBC 2.3.1!

**Debian/Ubuntu** --- While you'd think you should be able to just `apt-get install unixodbc` on Debian, this is not true. Even though unixODBC 2.3.1 was released nearly two years ago, Debian has not updated their version of unixODBC yet, not even in sid. This means you'll have to install from source in Debian. While this might sound daunting, it is actually quite simple. The process should be practically identical on Ubuntu but hasn't been tested on Ubuntu yet.

**Fedora** --- On Fedora, you should be able to just `yum install unixodbc` but I haven't tested this yet. Make sure you get unixODBC 2.3.1 or higher. If not, install from source (see below).

**Other distributions** --- Check whether your distro supplies you with version 2.3.1. Anything lower than that and you should probably consider...

## Installing unixODBC from source ##
_Note: For this guide I assume you are using Debian. While this should work on other distributions, you might need to tweak some paths and use different tools at some stages._

Before we get started, let's make sure that any previous versions of unixODBC are removed like so:
```
sudo apt-get remove libodbc1 unixodbc unixodbc-dev
```

Keep in mind that this will also remove libraries that depend on unixODBC, such as freetds.

Alright then, let's get started!
First, download unixODBC:
```
wget ftp://ftp.unixodbc.org/pub/unixODBC/unixODBC-2.3.1.tar.gz
```

Now, extract that archive:
```
tar xf unixODBC-2.3.1.tar.gz
```

<a href='Hidden comment: 
_Note: Some guides will advise you to change the library version from 2:0:0 to 1:0:0 in the configure script. I would prefer making a symbolic link from version 2 to version 1 as such:_
```
cd /usr/local/lib/
sudo ln -s libodbc.so.2.0.0 libodbc.so.1
sudo ln -s libodbcinst.so.2.0.0 libodbcinst.so.1
```
but that doesn"t seem to work correctly. So for now, we go with changing the API version.
'></a>

If you want to use the Microsoft ODBC SQL Driver, you need to do the following as well:
  * Open `configure` in a text editor of your choice (e.g. `vi configure`)
  * Replace the string `LIB_VERSION="2:0:0"` with `LIB_VERSION="1:0:0"`

Now, give the following commands to build and install unixODBC:
```
cd unixODBC-2.3.1
./configure --disable-gui --disable-drivers --enable-iconv --with-iconv-char-enc=UTF8 --with-iconv-ucode-enc=UTF16LE
make
sudo make install
```

Make sure your system can find the unixODBC library you just installed to /usr/local/lib by editing the file `/etc/ld.so.conf.d/x86_64-linux-gnu.conf` to append:
```
/usr/local/lib/
```
Then run:
```
sudo ldconfig
```

You have now successfully installed unixODBC. However, that's not all you need, you still need a driver to be able to actually connect with an SQL server.