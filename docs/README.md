# What is Maven?
Maven is a project management and comprehension tool. It provides developers a complete build lifecycle framework.
Maven provides developers ways to manage following:
- Builds
- Documentation
- Reporting
- Dependencies
- SCMs
- Releases
- Distribution
- mailing list

Maven primary goal is to provide developer
- A comprehensive model for projects which is reusable, maintainable, and easier to comprehend.
- plugins or tools that interact with this declarative model.

Maven project structure and contents are declared in an xml file, `pom.xml`, which is the fundamental unit of the entire Maven system.

## Convention Over Configuration
When a Maven project is created, Maven creates default project structure. 

| Item | Default |
| :------------- |:------------- |
|source code        | `${basedir}/src/main/java`      |
|resources	        | `${basedir}/src/main/resources` |
|Tests	            | `${basedir}/src/test`           |
|distributable JAR  | `${basedir}/target`             |
|Complied byte code	| `${basedir}/target/classes`     |

```
${basedir}
├── src
│   ├── main
│   │   ├── java
│   │   └── resources
│   └── test
│       ├── java
│       └── resources
└── target
    └── classes
```

## POM

?> POM stands for "Project Object Model".

The POM contains information about the project and various configuration detail used by Maven to build the project(s). POM also contains the goals and plugins.

While executing a task or goal, Maven looks for the POM in the current directory. It reads the POM, gets the needed configuration information, then executes the goal.

### Example POM
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
   http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>

   <groupId>com.companyname.project-group</groupId>
   <artifactId>project</artifactId>
   <version>1.0</version>
 
</project>
```

!> It should be noted that there should be a single POM file for each project. 

Before creating a POM, we should first decide the project group (`groupId`), its name(`artifactId`) and its `version` as these attributes help in uniquely identifying the project in repository.

| Node	| Description |
| ------------- |:-------------|
|groupId |	This is an Id of project's group. This is generally unique amongst an organization or a project. For example, a banking group com.company.bank has all bank related projects. |
| artifactId |	This is an Id of the project.This is generally name of the project. For example, consumer-banking. Along with the groupId, the artifactId defines the artifact's location within the repository. |
| version | This is the version of the project.Along with the groupId, It is used within an artifact's repository to separate versions from each other. |


- `com.company.bank:consumer-banking:1.0`
- `com.company.bank:consumer-banking:1.1`

### Super POM
All POMs inherit from a parent (despite explicitly defined or not). This base POM is known as the **Super POM**, and contains values inherited by default.

?>Take a look at the [super POM](https://maven.apache.org/pom.html#The_Super_POM) for maven 3.0.4.

Maven use the effective pom (configuration from super pom plus project configuration) to execute relevant goal.

An easy way to look at the default configurations of the super POM is by running the following command: 
```bash
mvn help:effective-pom
```

## Build Life cycle
A Build Life cycle is a well defined sequence of phases which define the order in which the goals are to be executed. Here phase represents a stage in life cycle.

- A `lifecyle` consists of different `phases`.
- A `phase` consists of different `goals`.
- A `goal` represents a specific task which contributes to the building and managing of a project.

### Goal
A `goal` may be bound to zero or more build `phases`. A goal not bound to any build phase could be executed outside of the build `lifecycle` by direct invocation.

The order of execution depends on the order in which the goal(s) and the build phase(s) are invoked. For example, consider the command below. The clean and package arguments are build phases while the `dependency:copy-dependencies` is a goal.

```bash
mvn clean dependency:copy-dependencies package
```

Here the `clean` phase will be executed first, and then the `dependency:copy-dependencies` goal will be executed, and finally `package` phase will be executed.

### Phase
`phase` represents a stage in life cycle.

### Lifecycle
A Build Lifecycle is a well defined sequence of phases which define the order in which the goals are to be executed.

- clean
- default(or build)
- site

?> Checkout this [link](https://www.tutorialspoint.com/maven/maven_build_life_cycle.htm) for more details.

#### clean Lifecycle
```xml
<phases>
  <phase>pre-clean</phase>
  <phase>clean</phase>
  <phase>post-clean</phase>
</phases>
```

#### default Lifecycle
```xml
<phases>
  <phase>validate</phase>
  <phase>initialize</phase>
  <phase>generate-sources</phase>
  <phase>process-sources</phase>
  <phase>generate-resources</phase>
  <phase>process-resources</phase>
  <phase>compile</phase>
  <phase>process-classes</phase>
  <phase>generate-test-sources</phase>
  <phase>process-test-sources</phase>
  <phase>generate-test-resources</phase>
  <phase>process-test-resources</phase>
  <phase>test-compile</phase>
  <phase>process-test-classes</phase>
  <phase>test</phase>
  <phase>prepare-package</phase>
  <phase>package</phase>
  <phase>pre-integration-test</phase>
  <phase>integration-test</phase>
  <phase>post-integration-test</phase>
  <phase>verify</phase>
  <phase>install</phase>
  <phase>deploy</phase>
</phases>
```

#### site Lifecycle
```xml
<phases>
  <phase>pre-site</phase>
  <phase>site</phase>
  <phase>post-site</phase>
  <phase>site-deploy</phase>
</phases>
```


## Plugins
Maven is actually a plugin execution framework where every task is actually done by plugins.
```
mvn [plugin-name]:[goal-name]
```
For example
```
mvn compiler:compile
```

## Repository
In Maven terminology, a repository is a place i.e. directory where all the project jars, library jar, plugins or any other project specific artifacts are stored and can be used by Maven easily.
- local (~/.m2)
- central
- remote

When we execute Maven build commands, Maven starts looking for dependency libraries in the following sequence:

1. Search in local repository, if not found, move to step 2.
2. Search in central repository, if not found and remote repository/repositories is/are mentioned then move to step 4. If found, then it is downloaded to local repository.
3. If a remote repository has not been mentioned, Maven simply stops the processing and throws error (Unable to find dependency).
4. Search in remote repositories, if found then it is downloaded to local repository. Otherwise throws error (Unable to find dependency).

Example for remote repositories:
```xml
<repositories>
    <repository>
        <id>companyname.lib1</id>
        <url>http://download.companyname.org/maven2/lib1</url>
    </repository>
    <repository>
        <id>companyname.lib2</id>
        <url>http://download.companyname.org/maven2/lib2</url>
    </repository>
</repositories>
```

## Creating Project
Create a project based on a template.
```bash
mvn archetype:generate \
-DgroupId=com.companyname.bank 
-DartifactId=consumerBanking 
-DarchetypeArtifactId=maven-archetype-quickstart 
-DinteractiveMode=false
```

## Settings
The settings element in the `settings.xml` file contains elements used to define values which configure Maven execution in various ways, like the pom.xml, but should not be bundled to any specific project, or distributed to an audience. These include values such as the local repository location, alternate remote repository servers, and authentication information.

There are two locations where a settings.xml file may live:

- The Maven install: `${maven.home}/conf/settings.xml`
- A user’s install: `${user.home}/.m2/settings.xml`

The former settings.xml are also called global settings, the latter settings.xml are referred to as user settings. If both files exists, their contents gets merged, with the user-specific settings.xml being dominant.

Here is an overview of the top elements under settings:

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                        https://maven.apache.org/xsd/settings-1.0.0.xsd">
    <localRepository/>
    <interactiveMode/>
    <usePluginRegistry/>
    <offline/>
    <pluginGroups/>
    <servers/>
    <mirrors/>
    <proxies/>
    <profiles/>
    <activeProfiles/>
</settings>
```

## Reference
!>Disclaimer: This is a personal study notes for Maven.
- [Apache Maven Project](https://maven.apache.org/index.html)
- [TutorialPoint Maven](https://www.tutorialspoint.com/maven/index.htm)
