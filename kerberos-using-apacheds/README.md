# Kerberos server with EAP 7 demo - using ApacheDS

A sample Kerberos project using ApacheDS directory service.

## Build the project

You need to have [Maven](http://maven.apache.org/) installed

	$ cd kerberos-using-apacheds
	$ mvn clean install

## Run the Kerberos server

Launch the generated JAR file. You can put LDIF files as the program arguments:

	$ java -jar target/kerberos-using-apacheds.jar test.ldif

You can use property  `${hostname}` in the LDIF file and it will be replaced by the canonical server host name:

	dn: uid=HTTP,ou=Users,dc=jboss,dc=org
	objectClass: top
	objectClass: person
	objectClass: inetOrgPerson
	objectClass: krb5principal
	objectClass: krb5kdcentry
	cn: HTTP
	sn: Service
	uid: HTTP
	userPassword: httppwd
	krb5PrincipalName: HTTP/${hostname}@JBOSS.ORG
	krb5KeyVersionNumber: 0 

### Bind address

The server binds to `localhost` by default. If you want to change it, set the Java system property `kerberos.bind.address`:

	$ java -Dkerberos.bind.address=192.168.0.1 -jar target/kerberos-using-apacheds.jar test.ldif

### krb5.conf

The application generates simple `krb5.conf` file when launched in the current directory. If you want to use another file, specify the `kerberos.conf.path` system property:

	$ java -Dkerberos.conf.path=/tmp/krb5.conf -jar target/kerberos-using-apacheds.jar test.ldif

Now with the generated krb5.conf, either configure the JBOSS.ORG realm in the `/etc/krb5.conf` or define alternative path using `KRB5_CONFIG` system variable

	$ export KRB5_CONFIG=/tmp/krb5.conf
## Generate keytab

The project contains a simple Kerberos keytab generator: 

	$ java -classpath kerberos-using-apacheds.jar org.jboss.test.kerberos.CreateKeytab
	Kerberos keytab generator
	-------------------------
	Usage:
	java -classpath kerberos-using-apacheds.jar org.jboss.test.kerberos.CreateKeytab <principalName> <passPhrase> [<principalName2> <passPhrase2> ...] <outputKeytabFile>
	
	$ java -classpath kerberos-using-apacheds.jar org.jboss.test.kerberos.CreateKeytab HTTP/localhost@JBOSS.ORG httppwd http.keytab
	Keytab file was created: /home/kwart/kerberos-tests/http.keytab

### Configure a Kerberos Security Domain on EAP 

In order to use Kerberos authentication, you will need to define a login-module on your EAP configuration:

	<security-domain name="host" cache-type="default">
		<authentication>
			<login-module code="Kerberos" flag="required">
				<module-option name="debug" value="true"/>
				<module-option name="storeKey" value="true"/>
				<module-option name="refreshKrb5Config" value="true"/>
				<module-option name="useKeyTab" value="true"/>
				<module-option name="doNotPrompt" value="true"/>
				<module-option name="keyTab" value="/home/jboss/kerberos-eap7/kerberos-using-apacheds/http.keytab"/>
				<module-option name="principal" value="HTTP/localhost@JBOSS.ORG"/>
			</login-module>
		</authentication>
	</security-domain>

Next, we will configure the ManagementRealm to use Kerberos as default management authentication.
The following configuration will now be included in your server's XML file:
	<security-realm name="ManagementRealm">
		<server-identities>
			<kerberos>
				<keytab principal="HTTP/localhost@JBOSS.ORG" path="/home/jboss/kerberos-eap7/kerberos-using-apacheds/http.keytab" debug="true"/>
			</kerberos>
		</server-identities>
	. . .
	</security-realm>

### Testing the Kerberos login against Management Interfaces
We will now test the Kerberos authentication against the Web management interface. Before doing that, we will need obtaining an initial ticket for a principal of our Realm.
The recommended package to do that, is the krb5-workstation package which can be installed on a RHEL or Fedora operating system as follows:

	$ sudo yum install krb5-workstation

Now you can use the kinit command to collect a ticket for authentication. In the LDIF file we have imported, there are a couple of users for that purpose. One of them is hnelson@JBOSS.ORG whose password is “secret”. Execute from the shell:

	$ kinit hnelson@JBOSS.ORG
	Password for hnelson@JBOSS.ORG: secret

Now, from within the same shell, execute your browser pointing to the Admin Console: 

	$ firefox http://localhost:9990

You should be able to connect automatically (as hnelson@JBOSS.ORG) to the Web admin console of EAP 7

## Stop running server

Use `stop` command line argument:

	$ java -jar target/kerberos-using-apacheds.jar stop
