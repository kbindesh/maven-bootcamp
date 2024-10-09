# Creating a new Java project using Maven (on Windows)

## 00. Prerequisites

- JAVA (min jdk17)
- Maven
- Git
- IDE (VS Code/Eclipse)

## 01. Creating a new Java project

### 1.1 Using IDE (Visual Studio Code)

- Download and Install **VS Code**. You may refer this link: https://code.visualstudio.com/download
- Install the below listed **VS Code extensions**:

  - [Maven for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-maven)
  - [Extension Pack for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)

- Create a new folder, _binmavenproject_ >> Open folder in VS Code
- Press **Ctrl+Shift+P** >> Select **Maven: New Project**
  - archetype: maven-archetype-quickstart
  - archetype version: 1.4
  - groupId: com.bindesh.io
  - artifactId: binmavenproject
  - Locate the folder for creating this maven project i.e. _binmavenproject_

### 1.2 Using Maven CLI

- Launch the _command prompt_ or _terminal_ on your system.
- Create a new directory, say **MavenApp**, a Maven-based Java project directory >> Run the following command:

```
mvn archetype:generate -DgroupId=com.bindesh.io -DartifactId=BinMavenProject -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.5 -DinteractiveMode=false

```

- An archetype is an original pattern or a model for creating similar projects.
- In Maven, an _archetype_ is a template of a project that is combined with user input to produce a working Maven project.
- The following table describes what the template does:
  | Archetype ArtifactIds | Description |
  |----------------------------|-----------------------------------------------------------|
  | mvn archetype:generate | Creates a project |
  | -DgroupId=com.bindesh.io | Creates the com.bindesh.io dependency package structure |
  | maven-archetype-quickstart | Creates a Java project |
  | -DinteractiveMode=false | Sets interactive mode to false |

- The Java project named **BinMavenProject** is created. The following table presents the project details:
  | Project Structure | Description |
  |-------------------|-----------------------------------------------------------------------------------------------------------------|
  | BinMavenProject | Contains src folder and pom.xml |
  | src/main/java | Contains Java code files under the package structure |
  | | (com.bindesh/io) |
  | src/test | Contains test code files under the package structure |
  | | (com.bindesh/io) |
  | pom.xml | Contains information about the project and details of various configurations used by Maven to build the project |

## 02. Developing the Application logic

## 03. Reviewing the created Java project files & folder

- **App.java**
- **AppTest.java**
- **pom.xml**
  - Each project has a single pom.xml file.
  - Each pom.xml file has a project element and three mandatory fields:
    1. groupId
    2. artifactId
    3. version
  - Notice that Maven has already added _JUnit_ as the test framework.
  - The following table describes what each node does:
    | Node | Description |
    |--------------|--------------------------------------------------------------------------------------------------------|
    | project | Top-level element in all Maven pom.xml files |
    | modelVersion | Object model version that this POM is using |
    | groupId | Project groupId (for example, com.bindesh.io) |
    | artifactId | Project ID (for example, MavenLearning) |
    | packaging | Project files converted into a JAR file |
    | version | Project version used in the artifact's repository to separate each version (for example, 1.0-SNAPSHOT) |
    | name | Project display name |
    | url | Location of the project site  |

## 04. Creating a manifest using Maven

- In this section, you will learn how to use **maven-jar-plugin** to create a manifest file and package it by adding it to the JAR file.

- The manifest file performs the following tasks:

  - Defines the entry point of the project and creates an executable JAR file.
  - Adds the class path of the project dependencies.

## 05. Testing the Applicati

```
mvn test
```

## 06. Building the Application

- Clean and build your Maven project, and review the output:

```
mvn clean package
```

- Navigate to your project folder and notice that a **target** folder is created.
- Open the target folder, and review the folder structure.

| Folder name                      | Description                                                                                                                |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| classes                          | Contains .class files of Java source files                                                                                 |
| test-classes                     | Contains .class files of Java test files                                                                                   |
| maven-archiver                   | Contains the pom.properties file                                                                                           |
| surefire-reports                 | Contains the report of the application when mvn command is executed.                                                       |
| java                             | Empty Java file                                                                                                            |
| binmavenproject-1.0-SNAPSHOT.jar | Contains all the project related details in a single zip file. This is the executable JAR file used to run the application |

## 07. Packaging & Running the Application

- Navigate to the directory where you installed Maven >> conf >> Open the **settings.xml** file.

- Clean and package the files, plug-ins, and libraries before running the application:

```
mvn clean package
```

- Use the Maven Local repository to run your Maven application:

## 05. References
