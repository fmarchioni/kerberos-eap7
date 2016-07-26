# Kerberos EAP 7 Demo - using ApacheDS

A sample Kerberos project using ApacheDS directory service.

## How to get the sources

You should have [git](http://git-scm.com/) installed

	$ git clone https://github.com/fmarchioni/kerberos-eap7.git 

or you can download [current sources as a zip file](https://github.com/fmarchioni/kerberos-eap7/archive/master.zip)

## Build the project

You need to have [Maven](http://maven.apache.org/) installed

	$ cd kerberos-eap7
	$ mvn clean install 

## Project modules 

The project contains two modules:

1) kerberos-using-apacheds : This project contains an embedded ApacheDS Server and utilities to generate a keytab so that you can test Kerberos authentication using the EAP 7 Admin Console
2) kerberos-spnego : This project demonsytrates how to use Kerberos + SPNEGO to authenticate in a Web application

See the single projects for more details
