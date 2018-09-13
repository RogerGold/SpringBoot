# maven使用指南

## Maven is a powerful build tool for Java software projects.

## Maven POM Files
A Maven POM file (Project Object Model) is an XML file that describe the resources of the project. 
This includes the directories where the source code, test source etc. is located in,
what external dependencies (JAR files) your projects has etc.

Each project has a POM file. The POM file is named pom.xml and should be located in the root directory of your project. 

### groupId
The groupId element is a unique ID for an organization, or a project (an open source project, for instance).
Most often you will use a group ID which is similar to the root Java package name of the project. 

### artifactId
The artifactId element contains the name of the project you are building.
The artifact ID is used as name for a subdirectory under the group ID directory in the Maven repository. 
The artifact ID is also used as part of the name of the JAR file produced when building the project.

### versionId
The versionId element contains the version number of the project.
If your project has been released in different versions, 
for instance an open source API, then it is useful to version the builds. 
That way users of your project can refer to a specific version of your project. 

## Project Dependencies
You specify in the POM file what external libraries your project depends on, and which version, 
and then Maven downloads them for you and puts them in your local Maven repository. 
If any of these external libraries need other libraries, then these other libraries are also downloaded into your local Maven repository.
 
The dependencies are already found in your local repository, Maven will not download them. 
Only if the dependencies are missing will they be downloaded into your local repository.
 
Each dependency is described by its groupId, artifactId and version.

            <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>4.8.1</version>
              <scope>test</scope>
            </dependency>
        
## Maven Settings File

The settings files are called settings.xml. The two settings files are located at:

- The Maven installation directory: $M2_HOME/conf/settings.xml
- The user's home directory: ${user.home}/.m2/settings.xml

## Maven Repositories

Maven has three types of repository:

- Local repository
- Central repository
- Remote repository

Maven searches these repositories for dependencies in the above sequence. First in the local repository, 
then in the central repository, and third in remote repositories if specified in the POM.

### Local Repository
A local repository is a directory on the developer's computer. 
This repository will contain all the dependencies Maven downloads. 
The same Maven repository is typically used for several different projects.
Thus Maven only needs to download the dependencies once, even if multiple projects depends on them (e.g. Junit).

Your own projects can also be built and installed in your local repository, using the mvn install command. 
That way your other projects can use the packaged JAR files of your own projects as external dependencies 
by specifying them as external dependencies inside their Maven POM files.

Your Maven settings file is also located in your user-home/.m2 directory and is called settings.xml. 
Here is how you specify another location for your local repository:

### Central Repository
The central Maven repository is a repository provided by the Maven community.
By default Maven looks in this central repository for any dependencies needed but not found in your local repository.
Maven then downloads these dependencies into your local repository. 
You need no special configuration to access the central repository.

### Remote Repository
A remote repository is a repository on a web server from which Maven can download dependencies, 
just like the central repository. A remote repository can be located anywhere on the internet, or inside a local network.

A remote repository is often used for hosting projects internal to your organization, which are shared by multiple projects.

You can configure a remote repository in the settings.xml.

           <repository>
              <id>central</id>
              <name>Nexus</name>
              <url>http:XXX</url>
              <releases><enabled>true</enabled></releases>
              <snapshots><enabled>true</enabled></snapshots>
            </repository>

## Maven Build Life Cycles

- validate	Validates that the project is correct and all necessary information is available. This also makes sure the dependencies are downloaded.
- compile	Compiles the source code of the project.
- test	Runs the tests against the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed.
- package	Packs the compiled code in its distributable format, such as a JAR.
- install	Install the package into the local repository, for use as a dependency in other projects locally.
- deploy	Copies the final package to the remote repository for sharing with other developers and projects.


