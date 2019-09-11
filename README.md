# r7_weak_creds_checks
Building Weak Credential Vulnerability Checks

Nexpose includes a framework for creating complex vulnerability checks using a simple XML format. 
Nexpose vulnerability checks are split across two or more files which are parsed by Nexpose when the scan engine is started.

There are 2 required types of XML files that make up a vulnerability check:

Vulnerability descriptor - A file ending in the .xml extension which contains information about a specific vulnerability (title, description, severity, CVE IDs, CVSS score, etc.).
Vulnerability check - A file ending in the .vck extension containing multiple tests which are compiled at runtime and used by Nexpose to verify the existence (or non-existence) of the vulnerability described in the descriptor.

A third optional XML file type exists:
Vulnerability Solution File - A file ending in the .sol extension contains vulnerability solution information which may optionally be included in the vulnerability definition .xml file or broken out into a .sol for re-use for other vulnerabilities. Solutions contain information about how to remediate the vulnerability. A solution file allows the solution to be written once and updated in one place when the recommended solution changes for many vulnerabilities that use the same solution.
External reference for solution files

================= USAGE =================

Usage: r7_weak_creds.pl [Options]

Input options:
    
    [service(s)]    Service(s) to generate weak creds checks for (comma-seperated)
    
    [file]          File of usernames (one per line)
    
    [file]          File of passwords (one per line)
    
    [file]          File of realms (one per line) - (*optional*)
    
    [dir]           Output directory (default: $service/) - (*optional*)

For databases, the realm represents the database name. 
If a realm file is not passed, r7_weak_creds.pl uses the default database name.

Supported Services include db2, tds, mysql, postgres, ssh, ftp, telnet, cifs and tomcat

================= EXAMPLE =================

Running r7_weak_creds.pl will generate the new .vck and .xml file(s) within a directory corresponding to the service for the checks.

$ ./r7_weak_creds.pl ssh usernames.txt passwords.txt realms.txt

$ ls cmty*

cmty-cifs-default-account-jdoe-password-Spring2019-mydomain.vck

cmty-cifs-default-account-jdoe-password-Spring2019-mydomain.xml

cmty-cifs-default-account-jdoe-password-Welcome2019-mydomain.vck

cmty-cifs-default-account-jdoe-password-Welcome2019-mydomain.xml

cmty-cifs-default-account-jdoe-password-password123-mydomain.vck

cmty-cifs-default-account-jdoe-password-password123-mydomain.xml



