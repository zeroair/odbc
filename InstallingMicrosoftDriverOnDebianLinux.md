# Introduction #
Oh yes, Microsoft has published its own driver for the Linux platform. While it is only targeted at Red Hat, it can be installed on Debian relatively easily. It supports a few advanced features (MARS for example) that FreeTDS doesn't, but it is closed source and it will only run on x86 (64-bit) platforms.

This page will guide you through setting up the Microsoft ODBC Driver on a Linux machine. This guide assumes a Debian-based distribution. The process may be significantly easier on Fedora.
Make sure you have properly [installed unixODBC](InstallingUnixODBC.md) before following this guide.

# Downloading the driver #
Download the latest Microsoft SQL Driver for Red Hat 6.
```
$ wget http://download.microsoft.com/download/B/C/D/BCDD264C-7517-4B7D-8159-C99FC5535680/RedHat6/msodbcsql-11.0.2270.0.tar.gz
```
_Note: If this link doesn't work, go to http://msdn.microsoft.com/library/hh568451%28SQL.110%29.aspx and download the latest version of the driver for RedHat 6._

Extract the archive and navigate to the extracted path:
```
$ tar xf msodbc*
$ cd msodbc*
```

# Resolving library dependencies #
This is the difficult part. The Microsoft driver expects a RedHat system, and thus expects certain libraries to be at certain locations. Because we are trying to run this on a Debian system, we need to make some symbolic links so the Microsoft driver can find the necessary files. For more information on symbolic links, see [this Wikipedia article](http://en.wikipedia.org/wiki/Symbolic_link).

To find out which drivers are still missing, enter the following command:
```
$ ldd lib64/libmsodbcsql-11.0.so.2270.0
```

You will see a whole lot of information which should look something like this:
<pre>
linux-vdso.so.1 =>  (0x00007fff4a1ff000)<br>
libcrypto.so.10 => not found<br>
libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007fa3ddca5000)<br>
librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x00007fa3dda9c000)<br>
libssl.so.10 => not found<br>
libuuid.so.1 => /lib/x86_64-linux-gnu/libuuid.so.1 (0x00007fa3dd897000)<br>
libodbcinst.so.1 => not found<br>
libkrb5.so.3 => /usr/lib/x86_64-linux-gnu/libkrb5.so.3 (0x00007fa3dd5c2000)<br>
libgssapi_krb5.so.2 => /usr/lib/x86_64-linux-gnu/libgssapi_krb5.so.2 (0x00007fa3dd383000)<br>
libstdc++.so.6 => /usr/lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007fa3dd07b000)<br>
libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007fa3dcdf9000)<br>
libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007fa3dcbe3000)<br>
libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007fa3dc9c6000)<br>
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fa3dc63c000)<br>
/lib64/ld-linux-x86-64.so.2 (0x00007fa3de263000)<br>
libk5crypto.so.3 => /usr/lib/x86_64-linux-gnu/libk5crypto.so.3 (0x00007fa3dc413000)<br>
libcom_err.so.2 => /lib/x86_64-linux-gnu/libcom_err.so.2 (0x00007fa3dc20e000)<br>
libkrb5support.so.0 => /usr/lib/x86_64-linux-gnu/libkrb5support.so.0 (0x00007fa3dc005000)<br>
libkeyutils.so.1 => /lib/x86_64-linux-gnu/libkeyutils.so.1 (0x00007fa3dbe01000)<br>
libresolv.so.2 => /lib/x86_64-linux-gnu/libresolv.so.2 (0x00007fa3dbbea000)<br>
</pre>

These are the names of the libraries that the Microsoft driver needs.
Look for the lines which say `=> not found`.
These are the libraries that we need to make sure can be found by the Microsoft driver.

On my system, the following libraries needed to be found:
```
$ ldd lib64/libmsodbcsql-11.0.so.2270.0 | grep not
```
<pre>
libcrypto.so.10 => not found<br>
libssl.so.10 => not found<br>
</pre>
_Note: If you see `libodbcinst.so.1 => not found` in this list, you have not changed the LIB\_VERSION yet, see the process for [installing unixODBC](InstallingUnixODBC.md)._

To solve these errors, run:
```
$ cd /usr/lib/x86_64-linux-gnu
$ sudo apt-get install libssl-dev
$ sudo ln -s libssl.so.1.0.0 libssl.so.10
$ sudo ln -s libcrypto.so.1.0.0 libcrypto.so.10
```

Verify that all libraries are installed correctly:
```
$ cd PATH_OF_MSDRIVER
$ ldd lib64/libmsodbcsql-11.0.so.2270.0
```
<pre>
linux-vdso.so.1 =>  (0x00007fff753d7000)<br>
libcrypto.so.10 => /usr/lib/x86_64-linux-gnu/libcrypto.so.10 (0x00007f00e62ec000)<br>
libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f00e60e8000)<br>
librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x00007f00e5edf000)<br>
libssl.so.10 => /usr/lib/x86_64-linux-gnu/libssl.so.10 (0x00007f00e5c80000)<br>
libuuid.so.1 => /lib/x86_64-linux-gnu/libuuid.so.1 (0x00007f00e5a7b000)<br>
libodbcinst.so.1 => /usr/local/lib/libodbcinst.so.1 (0x00007f00e5863000)<br>
libkrb5.so.3 => /usr/lib/x86_64-linux-gnu/libkrb5.so.3 (0x00007f00e558f000)<br>
libgssapi_krb5.so.2 => /usr/lib/x86_64-linux-gnu/libgssapi_krb5.so.2 (0x00007f00e5350000)<br>
libstdc++.so.6 => /usr/lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007f00e5048000)<br>
libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f00e4dc6000)<br>
libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007f00e4bb0000)<br>
libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f00e4993000)<br>
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f00e4609000)<br>
libz.so.1 => /lib/x86_64-linux-gnu/libz.so.1 (0x00007f00e43f2000)<br>
/lib64/ld-linux-x86-64.so.2 (0x00007f00e6a8a000)<br>
libk5crypto.so.3 => /usr/lib/x86_64-linux-gnu/libk5crypto.so.3 (0x00007f00e41c8000)<br>
libcom_err.so.2 => /lib/x86_64-linux-gnu/libcom_err.so.2 (0x00007f00e3fc4000)<br>
libkrb5support.so.0 => /usr/lib/x86_64-linux-gnu/libkrb5support.so.0 (0x00007f00e3dbb000)<br>
libkeyutils.so.1 => /lib/x86_64-linux-gnu/libkeyutils.so.1 (0x00007f00e3bb6000)<br>
libresolv.so.2 => /lib/x86_64-linux-gnu/libresolv.so.2 (0x00007f00e39a0000)<br>
</pre>

# Installing the driver #
If all looks okay, start the installation process:
```
$ sudo bash ./install.sh install --force
```

You will see a license agreement. If you agree to this license, press q, type `YES` and press enter. You should now see:
<pre>
Checking for 64 bit Linux compatible OS ................................. FAILED<br>
Checking required libs are installed ............................... NOT CHECKED<br>
unixODBC utilities (odbc_config and odbcinst) installed ............ NOT CHECKED<br>
unixODBC Driver Manager version 2.3.0 installed .................... NOT CHECKED<br>
unixODBC Driver Manager configuration correct ...................... NOT CHECKED<br>
Microsoft SQL Server ODBC Driver V1.0 for Linux already installed .. NOT CHECKED<br>
Microsoft SQL Server ODBC Driver V1.0 for Linux files copied ................ OK<br>
Symbolic links for bcp and sqlcmd created ................................... OK<br>
Microsoft SQL Server ODBC Driver V1.0 for Linux registered ........... INSTALLED<br>
</pre>

This is fine.

You can now test whether the Microsoft library is correctly installed by trying to connect to a non-existing host with `sqlcmd`:
```
$ sqlcmd -S localhost
```

If you see this, you're fine:
<pre>
Sqlcmd: Error: Microsoft ODBC Driver 11 for SQL Server : Login timeout expired.<br>
Sqlcmd: Error: Microsoft ODBC Driver 11 for SQL Server : TCP Provider: Error code 0x274D.<br>
Sqlcmd: Error: Microsoft ODBC Driver 11 for SQL Server : A network-related or instance-specific error has occurred while establishing a connection to SQL Server. Server is not found or not accessible. Check if instance name is correct and if SQL Server is configured to allow remote connections. For more information see SQL Server Books Online..<br>
</pre>

The only thing that is left is letting unixODBC know about the Microsoft library. To this end, create a new text file with the following contents:
```
[MSSQL]
Description	= Official native client
Driver		=/usr/local/lib/libmsodbcsql-11.0.so.2270.0
```

I saved this as tds.driver.template2. Now run:
```
$ sudo odbcinst -i -d -f tds.driver.template2
```

You should now see:
<pre>
odbcinst: Driver installed. Usage count increased to 1.<br>
Target directory is /usr/local/etc<br>
</pre>

If you see this, you have successfully installed the Microsoft driver.

# Acknowledgements #
A lot of commands were taken from http://blog.afoolishmanifesto.com/archives/1855 and only slightly changed to work with the latest version of the driver released by Microsoft.