---
title: "Connecting AWS Python Lambda to an Oracle Database, replace cx_Oracle with python-oracledb"
date: 2022-07-18
categories:
  - aws lambda python oracle database cx_Oracle python-oracledb
tags:
  - aws lambda python oracle database cx_Oracle python-oracledb
---

If you would like to connect an AWS python lambda to an Oracle database, there is a new package called `python-oracledb` which replaces the old package `cx_Oracle`.
A search will reveal the old `cx_Oracle` package is difficult to install, with many work arounds and hacks - due to operating system library requirements.

A new package `python-oracledb` was [released in May of 2022](https://python-oracledb.readthedocs.io/en/latest/release_notes.html#releasenotes), and simplifies the installation process.  
`python-oracledb` can run in **Thin** mode (the default), with an optional **Thick** mode requiring additional client dependencies.

More information on **Thin** vs **Thick** and upgrading from the `cx_Oracle` package, can be found on the [python-oracledb](https://python-oracledb.readthedocs.io/en/latest/user_guide/installation.html) website.  
A feature comparison of [python-oracledb Thin vs Thick vs cx_Oracle](https://python-oracledb.readthedocs.io/en/latest/user_guide/appendix_a.html#featuresummary) is also available.

Its worth noting that references to the package as `python-oracledb` and `oracledb` are used interchangeably.  
The former, typically being used in documentation to distinguish it from other languages.

# Installtion Trouble shooting

## Error "cannot import name 'base_impl'"
If you receive the error below, its worth checking that your package installation is correct:  
```json
{
   "errorMessage": "Unable to import module 'main': cannot import name 'base_impl' from partially initialized module 'oracledb'
    (most likely due to a circular import) (/var/task/oracledb/init.py)",
   "errorType": "Runtime.ImportModuleError",
   {snip}
}
```
Remember that packaging is platform dependant and ensure your [build process hasn't excluded any files](https://github.com/oracle/python-oracledb/discussions/32).

## Error "DPY-6000: cannot connect to database"
I received the following error, as the Oracle database I was trying to connect to required some encryption settings:
```python
DPY-6000: cannot connect to database. Listener refused connection. (Similar to ORA-12660)
```
The error `ORA-12660` indicates some [Encryption settings in `sqlnet.ora` need to be set](https://support.oracle.com/knowledge/Middleware/2395707_1.html).  
`sqlnet.ora` settings can only be done in Thick mode with the `python-oracledb` package.  
See the [Oracle `python-oracledb` documentation on the `sqlnet.ora`](https://python-oracledb.readthedocs.io/en/latest/user_guide/initialization.html#optional-oracle-net-configuration-files), on how this can be set.  


{: .notice--primary}
<strong>References:</strong>  
[python-oracledb - docs](https://python-oracledb.readthedocs.io/en/latest/user_guide/installation.html)  
[python-oracledb - github](https://oracle.github.io/python-oracledb)  
[Open Source Python Thin Driver for Oracle Database](https://levelup.gitconnected.com/open-source-python-thin-driver-for-oracle-database-e82aac7ecf5a)  
[ORA-12660: Encryption Or Crypto-checksumming Parameters Incompatible When Connecting To Application](https://support.oracle.com/knowledge/Middleware/2395707_1.html)