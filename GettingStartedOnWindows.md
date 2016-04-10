# Introduction #

So you want to use odbc from your Go project? Perhaps you prefer Microsoft's SQL server over alternatives or you need to connect to a third party database. Look no further!

# Installing #
Installing odbc on Windows is as easy as running `go get code.google.com/p/odbc` from the command line.

_If you get the message that the command '`hg`' was not found, download Mercurial from the following page: http://mercurial.selenic.com/wiki/Download.
You only need the `hg` command, not TortoiseHg if you don't want it._

You are now able to connect to a Microsoft SQL database from a Go program running on Windows.

If all went well, you should consider running the supplied [unit tests](RunningTests.md) to make sure your environment is working.