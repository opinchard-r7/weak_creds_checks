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

$ ./r7_weak_creds.pl cifs usernames.txt passwords.txt realms.txt

$ ls cmty*

cmty-cifs-default-account-jdoe-password-Spring2019-mydomain.vck

cmty-cifs-default-account-jdoe-password-Spring2019-mydomain.xml

cmty-cifs-default-account-jdoe-password-Welcome2019-mydomain.vck

cmty-cifs-default-account-jdoe-password-Welcome2019-mydomain.xml

cmty-cifs-default-account-jdoe-password-password123-mydomain.vck

cmty-cifs-default-account-jdoe-password-password123-mydomain.xml

================= DEPLOY =================

To deploy this vulnerability check into Nexpose, simply copy your .xml and .vck files file(s) into the following directory for the scan console and any attached scan engines:

/opt/rapid7/nexpose/plugins/java/1/CustomScanner/1/

and restart Nexpose. You should see something like the following message in the log:

NSC  3/13/10 11:10 AM: Imported 1 new and 0 modified vulnerabilities in 22 seconds

Within the Nexpose console and scan engines command line interfaces new vulnerability checks and descriptions may be loaded without restarting the respective services. The 'load content' command initiates a background re-load of vulnerability information.

> load content

2018-01-03T11:29:21 [INFO] > load content

2018-01-03T11:29:28 [INFO] Loading vulnerability and solution managers.

2018-01-03T11:29:35 [INFO] [Started: 2018-01-03T16:29:28] [Duration: 0:00:07.102] Completed loading vulnerability and solution managers

2018-01-03T11:29:35 [INFO] Loading vulnerability check manager.

... content trimmed for this article ...

2018-01-03T11:35:02 [INFO] Load Content command complete.

