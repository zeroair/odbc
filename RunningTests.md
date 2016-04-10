# Introduction #

So, you've set up your environment and you are ready to get cracking on some mean code. Before you do that though, it might be wise to run the unit tests that come with odbc.

This page will tell you how.


# Running the tests #
Luckily, odbc comes with its own unit tests which you can use to make sure your environment is working.
You are going to need your own MS-SQL database running somewhere. The tests drop and create a table named '`dbo.temp`'.

First make sure you have succesfully retrieved and built odbc:
```
go get code.google.com/p/odbc
```

Then, navigate to the following path: `$GOPATH/src/code.google.com/p/odbc/` (`%GOPATH%\src\code.google.com\p\odbc\` on Windows)

Now, run from the command line (Substitute capital words with your own details):
```
go test -mssrv=SERVERNAME.COM -msdb=DATABASENAME -msuser=USERNAME -mspass=YOURPASSWORD -v -run=MS
```
The flag `-run=MS` tells the go tool to only run tests that start with the letters 'MS', thus skipping the FoxPro tests.

If all went well, you should see something like:
```
=== RUN TestMSSQLCreateInsertDelete
--- PASS: TestMSSQLCreateInsertDelete (7.44 seconds)
=== RUN TestMSSQLTransactions
--- PASS: TestMSSQLTransactions (0.88 seconds)
=== RUN TestMSSQLTypes
--- PASS: TestMSSQLTypes (3.79 seconds)
=== RUN TestMSSQLIntAfterText
--- PASS: TestMSSQLIntAfterText (0.05 seconds)
=== RUN TestMSSQLStmtAndRows
--- PASS: TestMSSQLStmtAndRows (1.99 seconds)
=== RUN TestMSSQLIssue5
--- PASS: TestMSSQLIssue5 (1.14 seconds)
=== RUN TestMSSQLDeleteNonExistent
--- PASS: TestMSSQLDeleteNonExistent (0.77 seconds)
PASS
ok      code.google.com/p/odbc  16.071s
```

You are now able to connect to a Microsoft SQL database from a Go program.

# Notes #
If you run the Microsoft library **on linux** instead of FreeTDS add the flag `-msdriver="ODBC Driver 11 for SQL Server`".

If you need to connect to a different port with FreeTDS add the flag `-msport=YOURPORT` where YOURPORT is the port-number you want to connect to.

If you need to connect to a different port with a Microsoft driver simply add the port to the servername parameter like so: `-mssrv=SERVERNAME.COM,YOURPORT`.