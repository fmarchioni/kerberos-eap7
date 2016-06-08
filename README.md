# Kerberos Demo for EAP 7

Demo application which shows, how to get Kerberos authentication working in JBoss Application Server v 7.
It is made up of two projects:
 - kerberos-using-apacheds: Starts ApacheDS Server, creates keytab and Kerberos configuration file
 - spnego-demo: Builds a Demo application featuring SPNEGO authentication

### Compile the projects

	$ mvn clean install

Next move into the single projects and see README file
