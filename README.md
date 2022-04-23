Django ODBC
===================

django-odbc is a SQL Server DB backend powered by the pyodbc library. pyodbc is a mature, viable way to access SQL Server from Python in multiple platforms and is actively maintained. It's also used by SQLAlchemy for SQL Server connections.


Installation
------------

Using pip


    $ pip install django-odbc


Example
-----

    DATABASES = {
         'default': {
             'ENGINE': "django_odbc",
             'HOST': "127.0.0.1,1433",
             'USER': "mssql_user",
             'PASSWORD': "mssql_password",
             'NAME': "database_name",
             'OPTIONS': {
                 'host_is_server': True
             },
         }
    }
    python -c 'import pyodbc; print(pyodbc.connect("DSN=foobar_mssql_data_source_name;UID=foo;PWD=bar").cursor().execute("select 1"))'


Settings
-----


``NAME`` String. Database name. Required.

``HOST`` String. SQL Server instance in ``server\instance`` or ``ip,port`` format.

``PORT`` String. SQL Server port.

``USER`` String. Database user name. If not given then MS Integrated Security
will be used.

``PASSWORD`` String. Database user password.

``OPTIONS`` Dictionary. Current available keys:

* ``driver``

  String. ODBC Driver to use. Default is ``"SQL Server"`` on Windows and ``"FreeTDS"`` on other platforms.

* ``dsn``

  String. A named DSN can be used instead of ``HOST``.

* ``autocommit``

  Boolean. Indicates if pyodbc should direct the the ODBC driver to activate the autocommit feature. Default value is ``False``.

* ``MARS_Connection``

  Boolean. Only relevant when running on Windows and with SQL Server 2005 or later through MS *SQL Server Native client* driver (i.e. setting ``driver`` to ``"SQL Server Native Client 11.0"``). See http://msdn.microsoft.com/en-us/library/ms131686.aspx.  Default value is ``False``.

* ``host_is_server``

  Boolean. Only relevant if using the FreeTDS ODBC driver under Unix/Linux.

  By default, when using the FreeTDS ODBC driver the value specified in the ``HOST`` setting is used in a ``SERVERNAME`` ODBC connection string component instead of being used in a ``SERVER`` component; this means that this value should be the name of a *dataserver* definition present in the ``freetds.conf`` FreeTDS configuration file instead of a hostname or an IP address.

  But if this option is present and it's value is True, this special behavior is turned off.

  See http://freetds.org/userguide/dsnless.htm for more information.

* ``extra_params``

  String. Additional parameters for the ODBC connection. The format is
  ``"param=value;param=value"``.

* ``collation``

  String. Name of the collation to use when performing text field lookups against the database. For Chinese language you can set it to ``"Chinese_PRC_CI_AS"``. The default collation for the database will be used if no value is specified.

* ``encoding``

  String. Encoding used to decode data from this database. Default is 'utf-8'.

* ``driver_needs_utf8``

  Boolean. Some drivers (FreeTDS, and other ODBC drivers?) don't support Unicode yet, so SQL clauses' encoding is forced to utf-8 for those cases.

  If this option is not present, the value is guessed according to the driver set.

* ``limit_table_list``

  Boolean.  This will restrict the table list query to the dbo schema.

* ``openedge``

  Boolean.  This will trigger support for Progress Openedge

* ``left_sql_quote`` , ``right_sql_quote``

  String.  Specifies the string to be inserted for left and right quoting of SQL identifiers respectively.  Only set these if django-pyodbc isn't guessing the correct quoting for your system.  
