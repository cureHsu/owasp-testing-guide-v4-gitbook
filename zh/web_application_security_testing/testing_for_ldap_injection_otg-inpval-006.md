# Testing for LDAP Injection (OTG-INPVAL-006)

##  Summary
The Lightweight Directory Access Protocol (LDAP) is used to store information about users, hosts, and many other objects. [LDAP injection](https://www.owasp.org/index.php/LDAP_injection) is a server side attack, which could allow sensitive information about users and hosts represented in an LDAP structure to be disclosed, modified, or inserted. This is done by manipulating input parameters afterwards passed to internal search, add, and modify functions.


A web application could use LDAP in order to let users authenticate or search other users' information
inside a corporate structure. The goal of LDAP injection attacks is to inject LDAP search filters metacharacters in a query which will be executed by the application.

[[Rfc2254](http://www.ietf.org/rfc/rfc2254.txt)]
defines a grammar on how to build a search filter on LDAPv3 and
extends [[Rfc1960](http://www.ietf.org/rfc/rfc1960.txt)] (LDAPv2).


An LDAP search filter is constructed in Polish notation,
also known as [[prefix notation](http://en.wikipedia.org/wiki/Polish_notation)].


This means that a pseudo code condition on a search filter like this:
```
 find("cn=John & userPassword=mypass")
```
will be represented as:
```
 find("(&(cn=John)(userPassword=mypass))")
```

Boolean conditions and group aggregations on an
LDAP search filter could be applied by using
the following metacharacters:

| Metachar | Meaning              |
|----------|----------------------|
| &        | Boolean AND          |
| &#x7c;       | Boolean OR           |
| !        | Boolean NOT          |
| =        | Equals               |
| ~=       | Approx               |
| >=       | Greater than         |
| <=       | Less than            |
| *        | Any character        |
| ()       | Grouping parenthesis |


More complete examples on how to build a search filter can be
found in the related RFC.


A successful exploitation of an LDAP injection vulnerability could allow the tester to:

* Access unauthorized content
* Evade application restrictions
* Gather unauthorized informations
* Add or modify Objects inside LDAP tree structure.


## How to Test


### Example 1: Search Filters

Let's suppose we have a web application using a search
filter like the following one:
```
 searchfilter="(cn="+user+")"
```
which is instantiated by an HTTP request like this:
```
 http://www.example.com/ldapsearch?user=John
```
If the value 'John' is replaced with a '*',
by sending the request:
```
 http://www.example.com/ldapsearch?user=*
```
the filter will look like:
```
 searchfilter="(cn=*)"
```
which matches every object with a 'cn' attribute equals to anything.


If the application is vulnerable to LDAP injection, it will display some or all of the users' attributes, depending on the application's execution flow and the permissions of the LDAP connected user.


A tester could use a trial-and-error approach, by inserting in the parameter
'(', '|', '&', '*' and the other characters, in order to check
the application for errors.


### Example 2: Login

If a web application uses LDAP to check user credentials during the login process and it is vulnerable to LDAP injection, it is possible to bypass the authentication check by injecting an always true LDAP query (in a similar way to SQL
and XPATH injection ).


Let's suppose a web application uses a filter to match LDAP user/password pair.
```
searchlogin= "(&(uid="+user+")(userPassword={MD5}"+base64(pack("H*",md5(pass)))+"))";
```

By using the following values:
```
 user=*)(uid=*))(|(uid=*
 pass=password
```
the search filter will results in:
```
 searchlogin="(&(uid=*)(uid=*))(|(uid=*)(userPassword={MD5}X03MO1qnZdYdgyfeuILPmQ==))";
```
which is correct and always true. This way, the tester will gain logged-in status as the first user in LDAP tree.


## Tools
Softerra LDAP Browser - http://www.ldapadministrator.com/

## References
**Whitepapers**<br>
Sacha Faust: "LDAP Injection: Are Your Applications Vulnerable?" - http://www.networkdls.com/articles/ldapinjection.pdf<br>
Bruce Greenblatt: "LDAP Overview" - http://www.directory-applications.com/ldap3_files/frame.htm<br>
IBM paper: "Understanding LDAP" - http://www.redbooks.ibm.com/redbooks/SG244986.html <br>



RFC 1960: "A String Representation of LDAP Search Filters" - http://www.ietf.org/rfc/rfc1960.txt<br>