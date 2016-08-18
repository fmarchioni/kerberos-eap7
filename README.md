# Kerberos EAP 7 Demo - using ApacheDS

A sample Kerberos project using ApacheDS directory service.

## How to get the sources

You should have [git](http://git-scm.com/) installed

	$ git clone https://github.com/mjbeap7/ch11.git 

or you can download [current sources as a zip file](https://github.com/mjbeap7/ch11/archive/master.zip)

## Build the project

You need to have [Maven](http://maven.apache.org/) installed

	$ cd kerberos-eap7
	$ mvn clean install 

## Project modules 

The project contains two modules:

* kerberos-using-apacheds : This project contains an embedded ApacheDS Server and utilities to generate a keytab so that you can test Kerberos authentication using the EAP 7 Admin Console
* kerberos-spnego : This project demonstrates how to use Kerberos + SPNEGO to authenticate in a Web application

See the single projects for more details
