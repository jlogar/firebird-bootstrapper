Firebird bootstrapper for ClickOnce
===================================

[Clickonce](http://msdn.microsoft.com/en-us/library/t71a733d(v=vs.100).aspx) is Microsofts deployment/autoupdate technology and [Firebird](http://www.firebirdsql.org/) an opensource RDBMS.

In general an application that requires a database server would require the server preinstalled in a previous step. By bootstrapping the Firebird installer to your clickonce deployed application you can have a single installer that includes both your application and the database server.

USAGE
--------------



TODO
--------------

- Ask the Firebird team to return something other than error when the installation fails because of an existing server installation