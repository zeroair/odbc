# Introduction #
This page will help you set up FreeTDS (an open-source driver for accessing Microsoft SQL Server and Sybase database servers).
Make sure you have properly [installed unixODBC](InstallingUnixODBC.md) before following this guide.

# TDS Versions #
The TDS protocol (the protocol Microsoft SQL Server and Sybase use) knows a lot of different versions. FreeTDS needs to know which version to use when connecting to a database. The following table details the different TDS versions:
| Microsoft SQL Server 2008 | 7.2 |
|:--------------------------|:----|
| Microsoft SQL Server 2005 | 7.2 |
| Microsoft SQL Server 2000 | 7.1 |
| Microsoft SQL Server 7.0  | 7.0 |
| Sybase System SQL Anywhere | 5.0 |
| Sybase System 10 and above | 5.0 |
| Sybase before System 10, Microsoft SQL Server 6.x | 4.2 |

You can build FreeTDS with a default version or you can specify the TDS version in the connection string.
## Compile FreeTDS with a default version ##
Easy, when installing FreeTDS (see below) substitute the following command:
```
$ ./configure
```

With:
```
$ ./configure --with-tdsver=VER
```
Where VER is the version you want to use (see table above).
However, I have found that this doesn't always work. Should you encounter problems, specify the protocol version in the connection string.

## Specify protocol version in connection string ##
This can be necessary if you need to connect to different servers which use different protocol versions.

I'll use the unit-test that comes with odbc as an example.
In mssql\_test.go, find the section that says:
```
params = map[string]string{
    "driver":   "freetds",
    "server":   *mssrv,
    "port":     "1433",
    "database": *msdb,
    "uid":      *msuser,
    "pwd":      *mspass,
    //"clientcharset": "UTF-8",
    //"debugflags": "0xffff",
}
```
Change it to add the TDS\_Version section like so:
```
params = map[string]string{
    "driver":   "freetds",
    "server":   *mssrv,
    "port":     "1433",
    "database": *msdb,
    "uid":      *msuser,
    "pwd":      *mspass,
    "TDS_Version": "7.2",
    //"clientcharset": "UTF-8",
    //"debugflags": "0xffff",
}
```
_I used TDS version 7.2 in this example_

# Actually installing FreeTDS #
Download FreeTDS:
```
$ wget ftp://ftp.astron.com/pub/freetds/stable/freetds-stable.tgz
```

Extract FreeTDS:
```
$ tar xf freetds-stable.tgz 
```

Compile and install FreeTDS:
```
$ cd freetds-0.91/
$ ./configure #Optionally specify TDS version here with --with-tdsver=VER
$ make
$ sudo make install
```

Now FreeTDS has been built, but unixODBC still needs to know about it.
To do that, put the following in a text file:
```
[FreeTDS] 
Description     = Open source FreeTDS driver
Driver          =/usr/local/lib/libtdsodbc.so
```
Save it as anything you would like (I use tds.driver.template) and then run:

```
$ sudo odbcinst -i -d -f tds.driver.template
```
<pre>
odbcinst: Driver installed. Usage count increased to 1.<br>
Target directory is /usr/local/etc<br>
</pre>
You can now use FreeTDS from your Go projects.

# Troubleshooting #
This section is intended for any problems you may encounter when following the above guide.

## FreeTDS: TDS\_Version auto-recognition not working ##
If you see something similar to:
<pre>
[unixODBC][FreeTDS][SQL Server]Unexpected EOF from the server<br>
[unixODBC][FreeTDS][SQL Server]Adaptive Server connection failed<br>
</pre>
when running tests then freetds fails to detect the version of the TDS-protocol your server is using. This can be fixed in two ways. See the section on default TDS version [above](InstallingFreeTDS#Specify_protocol_version_in_connection_string.md).