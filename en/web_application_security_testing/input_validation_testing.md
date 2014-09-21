# Input Validation Testing


The most common web application security weakness is the failure to properly validate input coming from the client or from the environment before using it. This weakness leads to almost all of the major vulnerabilities in web applications, such as cross site scripting, SQL injection, interpreter injection, locale/Unicode attacks, file system attacks, and buffer overflows.


Data from an external entity or client should never be trusted, since it can be arbitrarily tampered with by an attacker. "All Input is Evil", says Michael Howard in his famous book "Writing Secure Code". That is rule number one. Unfortunately, complex applications often have a large number of entry points, which makes it difficult for a developer to enforce this rule. This chapter describes Data Validation testing. This is the task of testing all the possible forms of input to understand if the application sufficiently validates input data before using it.


Input validation testing is split into the following categories:<br>

**Testing for Cross site scripting**<br>
Cross Site Scripting (XSS) testing checks if it is possible to manipulate the input parameters of the application so that it generates malicious output. Testers find an XSS vulnerability when the application does not validate their input and creates an output that is under their control. This vulnerability leads to various attacks, for example, stealing confidential information (such as session cookies) or taking control of the victim's browser. An XSS attack breaks the following pattern: Input -> Output  == cross-site scripting.


In this guide, the following types of XSS testing are discussed in details:<br>
[Testing for Reflected Cross Site Scripting (OTG-INPVAL-001) ](./testing_for_reflected_cross_site_scripting_otg-inpval-001.html)<br>
[Testing for Stored Cross Site Scripting (OTG-INPVAL-002) ](./testing_for_stored_cross_site_scripting_otg-inpval-002.html)<br>

Client side XSS testing, such as DOM XSS and Cross site Flashing is discussed in the Client Side testing section.

**Testing for HTTP Verb Tampering and Parameter Pollution**<br>
HTTP Verb Tampering tests the web application's response to different HTTP methods accessing system objects. For every system object discovered during spidering, the tester should attempt accessing all of those objects with every HTTP method.
HTTP Parameter Pollution tests the applications response to receiving multiple HTTP parameters with the same name. For example, if the parameter *username* is included in the GET or POST parameters twice, which one is honoured, if any.

HTTP Verb Tampering is described in

[Testing for HTTP Verb Tampering (OTG-INPVAL-003)](./testing_for_http_verb_tampering_otg-inpval-003.html)

and HTTP Parameter testing techniques are presented in

[Testing for HTTP Parameter pollution (OTG-INPVAL-004) ](./testing_for_http_parameter_pollution_otg-inpval-004.html)

**[SQL Injection  (OTG-INPVAL-005)](./testing_for_sql_injection_otg-inpval-005.html)<br>**
SQL injection testing checks if it is possible to inject data into the application so that it executes a user-controlled SQL query in the back-end database. Testers find an SQL injection vulnerability if the application uses user input to create SQL queries without proper input validation. A successful exploitation of this class of vulnerability allows an unauthorized user to access or manipulate data in the database. Note that application data often represents the core asset of a company. An SQL Injection attack breaks the following pattern:
Input -> Query SQL == SQL injection


SQL Injection testing is further broken down by product or vendor:<br>
[Oracle Testing](./oracle_testing.html)<br>
[MySQL Testing ](./mysql_testing.html)<br>
[SQL Server Testing ](./sql_server_testing.html)<br>
[Testing PostgreSQL](./testing_postgresql_from_owasp_bsp.html)<br>
[MS Access Testing](./ms_access_testing.html)<br>
[Testing for NoSQL injection](./testing_for_nosql_injection.html)<br>


**[LDAP Injection  (OTG-INPVAL-006)](./testing_for_ldap_injection_otg-inpval-006.html)<br>**
LDAP injection testing is similar to SQL Injection testing. The differences are that testers use the LDAP protocol instead of SQL and the target is an LDAP Server instead of a SQL Server.
An LDAP Injection attack breaks the following pattern:<br>
Input -> Query LDAP == LDAP injection<br>


**[ORM Injection  (OTG-INPVAL-007)](./testing_for_orm_injection_otg-inpval-007.html)<br>**
ORM injection testing is similar to SQL Injection Testing. In this case, testers use a SQL Injection against an ORM-generated data access object model. From the tester's point of view, this attack is virtually identical to a SQL Injection attack. However, the injection vulnerability exists in the code generated by an ORM tool.<br>


**[XML Injection (OTG-INPVAL-008)](./testing_for_xml_injection_otg-inpval-008.html)<br>**
XML injection testing checks if it possible to inject a particular XML document into the application. Testers find an XML injection vulnerability if the XML parser fails to make appropriate data validation.<br>
An XML Injection attack breaks the following pattern:<br>
Input -> XML doc == XML injection<br>


**[SSI Injection  (OTG-INPVAL-009)](./testing_for_ssi_injection_otg-inpval-009.html)<br>**
Web servers usually give developers the ability to add small pieces of dynamic code inside static HTML pages, without having to deal with full-fledged server-side or client-side languages. This feature is incarnated by Server-Side Includes (SSI) Injection. In SSI injection testing, testers check if it is possible to inject into the application data that will be interpreted by SSI mechanisms. A successful exploitation of this vulnerability allows an attacker to inject code into HTML pages or even perform remote code execution.<br>


**[XPath Injection  (OTG-INPVAL-010)](./testing_for_xpath_injection_otg-inpval-010.html)<br>**
XPath is a language that has been designed and developed primarily to address parts of an XML document. In XPath injection testing, testers check if it is possible to inject data into an application so that it executes user-controlled XPath queries. When successfully exploited, this vulnerability may allow an attacker to bypass authentication mechanisms or access information without proper authorization.<br>


**[IMAP/SMTP Injection  (OTG-INPVAL-011)](./imapsmtp_injection_otg-inpval-011.html)<br>**
This threat affects all the applications that communicate with mail servers (IMAP/SMTP), generally web mail applications. In IMAP/SMTP injection testing, testers check if it possible to inject arbitrary IMAP/SMTP commands into the mail servers, due to input data not properly sanitized. <br>
An IMAP/SMTP Injection attack breaks the following pattern:<br>
Input -> IMAP/SMTP command == IMAP/SMTP Injection<br>


**[Code Injection  (OTG-INPVAL-012)](./testing_for_code_injection_otg-inpval-012.html)<br>**
Code injection testing checks if it is possible to inject into an application data that will be later executed by the web server.<br>
A Code Injection attack breaks the following pattern:<br>
Input -> malicious Code == Code Injection<br>


**[OS Commanding   (OTG-INPVAL-013)](./testing_for_command_injection_otg-inpval-013.html)<br>**
In command injection testing testers will try to inject an OS command through an HTTP request into the application.<br>
An OS Command Injection attack breaks the following pattern:<br>
Input -> OS Command == OS Command Injection<br>


**[Buffer overflow (OTG-INPVAL-014)](./testing_for_buffer_overflow_otg-inpval-014.html)<br>**
In these tests, testers check for different types of buffer overflow vulnerabilities. Here are the testing methods for the common types of buffer overflow vulnerabilities:<br>
[Heap overflow ](./testing_for_heap_overflow.html)<br>
[Stack overflow ](./testing_for_stack_overflow.html)<br>
[Format string ](./testing_for_format_string.html)<br>

In general Buffer overflow breaks the following pattern:<br>
Input -> Fixed buffer or format string == overflow<br>


**[Incubated vulnerability (OTG-INPVAL-015)](./testing_for_incubated_vulnerabilities_otg-inpval-015.html) <br>**
Incubated testing is a complex testing that needs more than one data validation vulnerability to work.<br>


[Testing for HTTP Splitting/Smuggling  (OTG-INPVAL-016)](./testing_for_http_splittingsmuggling_otg-inpval-016.html)<br>
Describes how to test for an HTTP Exploit, as HTTP Verb, HTTP Splitting, HTTP Smuggling.


In every pattern shown, the data should be validated by the application before it's trusted and processed. The goal of testing is to verify if the application actually performs validation and does not trust its input.