# Vagrant Ubuntu box with Java 8 and Tomcat 9
Run a Vagrant Ubuntu box with Java 8 and Tomcat 9, where remote deploying is configured on port 1099. 
This unsecure box is made for development purposes.

## Requirements

Vagrant 1.9.2

Virtualbox 5.1.32

## Quick Start

1. Go to the project directory and enter the following command:

`vagrant up --provider=virtualbox`

2. Open the browser and enter the URL:

`http://localhost:8080/manager/html`

3. Enter the following credentials:

Username `admin` and password `password`. Now you can manually deploy a .war file. To do a remote deploy, follow the section below.

## Remote Deploy to Tomcat with IntelliJ IDEA

The manual on how to remote deploy is coming soon.

Start a new maven project with archetype webapp added.

In the top menu, go to 'Run' and hit 'Edit Configurations...'. Press the green '+' button in the left corner of the window. Search for 'Tomcat Server' and click 'Remote'.

Enter a name, for example 'vagrant'. Configure the Tomcat 9 application server. This can be done by downloading the 9.0.131 version from the Tomcat website. 

For the 'Remote Staging' settings, press 'sftp'.

...
