# Documentation of Code Compilation of Java


---

| Author         | Created on | Version | Last updated by  | Last edited on |
|:----------------:|:------------:|:---------:|:------------------:|:----------------:|
| Palash Kamble  | 13-04-2024 | 1.0     | Palash Kamble    | 13-04-2024     |



---

## Table of Content


---


## Introduction

In the realm of Java CI code compilation, Continuous Integration (CI) orchestrates the automated validation of code alterations. This process entails automatic building and testing of Java source code triggered by developers' push to the version control system. Upon successful testing and error-free verification, the code seamlessly integrates into the main branch. Subsequently, this integrated code is primed for swift deployment to either a server or a cloud service, thereby advancing the CI/CD pipeline efficiency.


---

## Purpose 

The primary Purpose of a Java compiler, like any compiler in programming, is to translate high-level Java source code into machine-readable instructions, represented as 0s and 1s. Once compiled, these instructions are executed to perform the intended tasks efficiently


---

## Work flow 
---

- **Java programing language working during compilation proccess .**

 

![image](https://github.com/palash80/Palash-repo/assets/153359214/dc33e829-ae15-406d-a854-4022ac5bd1e9)

- **First Step:** Create a Java source code file and save it with the ".java" extension, as it's essential for program execution.
    
- **Second Step:** Utilize a compiler to compile the source code, resulting in Java bytecode with a ".class" extension. This bytecode is a platform-independent 
    representation of the original source code.


- **Java Bytecode:** It serves as a redesigned version of the source code, enabling execution across various platforms.

- **Java Virtual Machine (JVM):** Execute the bytecode using the JVM, which interprets and converts the bytecode into machine-level language, allowing the        
    machine to execute the code effectively.


---

## Tools

![image](https://github.com/palash80/Palash-repo/assets/153359214/31895a62-456d-4a8c-b929-a3fb4e4870b9) ![image](https://github.com/palash80/Palash-repo/assets/153359214/248876eb-dd62-4e7c-b065-76435fb30c56)




- **Different tools use for java code compilation.** 
---

| **Tool**   | **Description**                                                                                         |
|------------|---------------------------------------------------------------------------------------------------------|
| **Maven**  | Build automation tool primarily used for Java projects; manages dependencies and builds.                 |
| **Ant**    | Build tool using XML-based configuration files; compiles Java code and performs build tasks.            |
| **Gradle** | Build automation tool with a flexible DSL, capable of compiling Java code and managing tasks.            |
| **javac**  | Standard Java compiler provided by Oracle as part of the Java Development Kit (JDK).                     |



---

## Comparison


| **Feature**                | **javac**                                 | **Apache Maven**                            | **Gradle**                                    | **Apache Ant**                              |
|:----------------------------:|:-------------------------------------------:|:----------------------------------------------:|:-----------------------------------------------:|:---------------------------------------------:|
| **Build File Format**      | .java files                               | pom.xml (XML)                               | build.gradle (Groovy or Kotlin DSL)           | build.xml (XML)                             |
| **Dependency Management**  | Manual (Classpath)                        | Yes (Central Repository)                    | Yes (Central Repository)                      | Yes (Central Repository)                    |
| **Plugins and Extensions** | Limited                                   | Abundant                                    | Extensible                                   | Abundant                                    |
| **Convention over Configuration** | No                                | Yes                                         | Yes                                           | No                                          |
| **Build Lifecycle Phases** | Compile                                   | clean, compile, test, package, install, deploy, etc. | build, clean, test, assemble, etc.          | compile, clean, test, jar, etc.            |
| **Community Support**      | N/A                                       | Active                                      | Active                                        | Moderate                                    |

---

## Advantages

### javac

| Advantages | Description |
|:------------:|-------------|
|  **Standardization:** | javac is the standard Java compiler provided by Oracle, ensuring compatibility with Java language specifications. |
|  **Integration:** | Seamlessly integrates with Java Development Kit (JDK) and other Java-related tools. |

### Apache Maven

| Advantages | Description |
|:------------:|-------------|
|  **Dependency Management:** | Simplifies dependency management by automatically downloading and configuring project dependencies from repositories like Maven Central. |
|  **Convention over Configuration:** | Follows conventions, reducing the need for manual configuration and allowing developers to focus more on coding. |
|  **Build Lifecycle:** | Provides predefined build lifecycle phases, making it easier to perform common tasks such as compilation, testing, packaging, and deployment. |

### Gradle

| Advantages | Description |
|:------------:|-------------|
|  **Flexibility:** | Offers a flexible DSL (Domain Specific Language) that allows for customized build configurations, suitable for a wide range of project types and sizes. |
|  **Performance:** | Gradle's incremental build feature speeds up build times by only recompi


---


##  Best practices

| **Practices** | **Description** |
|:-----------------:|---------------|
| **Automate Builds** | Use tools that automatically compile your Java code whenever you or your team make changes. This saves time and makes sure everyone is using the latest version of the code. |
| **Manage Dependencies** | Keep track of the other bits of code your project needs to work properly. Tools like Apache Maven or Gradle can help you with this. They make sure everything you need is in the right place. |
| **Keep Track of Changes** | Use a system like Git to keep a record of every change you make to your code. This helps you keep things organized and lets you go back to an earlier version if something goes wrong. |
| **Test Your Code** | Write tests to make sure your code does what it's supposed to do. This helps catch mistakes early on and makes sure your code works as expected. |
| **Check Your Code Quality** | Use tools that can look at your code and point out any mistakes or areas where it could be improved. This helps you write better code and catch problems before they cause issues. |
| **Build Things in Parallel** | If you're working on a big project, split it up into smaller parts and build them separately at the same time. This makes things faster and more efficient. |
| **Save Your Finished Code** | Once your code is compiled and tested, save it in a central place where everyone on your team can access it. This makes it easy to share and use in other projects. |
| **Get Feedback Quickly** | Set up notifications so you know right away if there's a problem with your code. This lets you fix things before they become bigger issues. |
| **Make Sure Everything Stays the Same** | Keep your build environment consistent so that everyone is working with the same tools and settings. This helps avoid surprises and makes sure your code behaves the same way for everyone. |
| **Build Step by Step** | Instead of compiling everything at once, just compile the parts of your code that have changed. This saves time and makes the process faster. |



---

## Why Use Maven ?


Maven is a popular build automation tool primarily used for managing dependencies and the build process in Java projects. While Maven is not directly responsible for code compilation itself (that's typically handled by the Java compiler), it plays a crucial role in facilitating and streamlining the compilation process. Here's why Maven is commonly used in code compilation:


1. **Dependency Management**: Maven simplifies managing the dependencies your project needs by automatically downloading and configuring them from a central repository.
2. **Convention over Configuration**: Maven follows conventions, reducing the need for manual configuration and making it easier to get started with projects.
3. **Build Lifecycle**: Maven provides predefined build lifecycle phases, ensuring consistency and simplifying common tasks like compiling, testing, and packaging code.


---

## Proof of Concept




---

**Step 1:** Installing necessary tools

1.First update the packge:

<pre><code>sudo apt update</code></pre>

2.Switch to sudo user (root user):

<pre><code>sudo su</code></pre>

3. Install JDK 11

<pre><code>apt install openjdk-11-jre-headless -y</code></pre>

![image](https://github.com/palash80/Palash-repo/assets/153359214/aa864747-dd65-48fd-9ca8-ae46b2870bd7)

4. Install Maven

<pre><code>apt install maven -y</code></pre>

![image](https://github.com/palash80/Palash-repo/assets/153359214/d8b45f17-b445-4d42-a43e-69701519e9ab)

5. Clone the Java project repository in the linux instance:

<pre><code>git clone https://github.com/Krishpluto/BoardgameListingWebApp.git</code></pre>

![image](https://github.com/palash80/Palash-repo/assets/153359214/49173137-adde-4717-9ea0-a66ea19dd2bf)


**Step 2:** Building the Project with Maven
  Now, navigate to your project’s directory and use Maven to build it. Maven follows a specific lifecycle consisting of phases. You can execute these phases 
  using Maven commands:
  Execute the Maven lifecycle by running the following command in your project directory:


 
 <pre><code>cd BoardgameListingWebApp/</code></pre>


**Step 4:** Building the Project with Maven
  Maven follows a specific lifecycle consisting of phases. You can execute these phases using Maven commands:

 **clean:** Cleans the project by removing the target directory.
 **validate:** Validates the project.
 **compile:** Compiles the source code.
 **test:** Executes the tests.
 **package:** Packages the compiled code into a JAR or WAR file.
 **install:** Installs the package into the local repository.
 **deploy:** Deploys the package to a remote repository.
 **site:** Generates project documentation.


