                                  My Programming Notes



                                  Spring Boot Basics

+ Spring Boot - A standard Java application, bundled with a lot of pre-configured 
libraries.

+ Maven - A build tool used to install all the necessary dependencies 
for Spring Boot.

+ "Convention and Configuration" - A rule enforced by Maven to place Java files 
into specific directories so that Maven can compile everything automatically and build 
the project without any build errors.

# create a multiple directories using -p flag and pom.xml file from the root directory
mkdir -p src/main/java 
mkdir -p src/main/resources
touch pom.xml


+ src/main/java - Maven demands that this directory structure and java dir, becomes a root  where our project package will start, like com.myproject..  

+ src/main/resources - Maven demands that this directory structure is used to keep non-java files like database connection credentials, applicaton settings and HTML/CSS files. Thisdirectory structure is automatically added to the root of the classpath that JVM searches to find files and clases at runtime.

+ pom.xml - Acronym pom stands for Project Object Model, its used as a brain of my Spring Boot application. It tells Maven what version of Java to use, what project is named, and 
what external dependencies(libraries) project needs to download to work. 


                                 Spring Boot Basics - Cues


# SpringBoot(Java📱(bundled(pre-configured(libraries)))) 
        
# Maven - Build 🧰 -> dependencies -> SpringBoot

# "Convention over Configuration" -> rule(src/main/java/com.mypack.MyClass.java) -> Maven build(✅)

# basic structure: (mkdir -p src/main/java(🫜), src/main/resources(🗄️,⚙️,</>/css), touch pom.xml), -p(📁/📁/📁)


                            The structure of the pom.xml file



### 1. The XML Header Breakdown

Even though developers copy-paste this, understanding it removes the "magic." Think of XML validation similarly to Java typing and interfaces. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
```
*   **The Declaration:** This is the standard first line of any XML file. It tells the parser reading it, "This is an XML document, using version 1.0 rules, and the text is encoded in UTF-8 (which supports all standard characters and symbols)."

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
```
*   **`<project>`:** This is the root node. Everything else in the file lives inside this.
*   **`xmlns` (XML Namespace):** *Java Analogy:* Think of a namespace exactly like a Java `package`. In Java, you might have `java.util.Date` and `java.sql.Date`. In XML, `xmlns` prevents naming conflicts. It tells the parser: "When I use tags in this file, they belong to the Maven POM 4.0.0 dictionary."

```xml
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
```
*   **`xmlns:xsi`:** This imports a standard web dictionary (from the W3C) for XML Schemas. We need this so we can use the `xsi:schemaLocation` attribute on the next line.

```xml
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
```
*   **`schemaLocation`:** *Java Analogy:* Think of an `.xsd` (XML Schema Definition) file as a **Java Interface** or a strict **Validator**. This line links the namespace to a physical file on the internet (`maven-4.0.0.xsd`). If you type a tag that Maven doesn't recognize (like `<myCustomTag>`), your IDE will throw a red error line. Why? Because it checks the `.xsd` file and says, "That tag is not defined in the schema!"

```xml
    <modelVersion>4.0.0</modelVersion>
```
*   **`<modelVersion>`:** This simply tells Maven, "I am using the POM format version 4.0.0." Maven versions 2, 3, and 4 all use the 4.0.0 model, so this basically never changes.

### 2. The "Automated" Terminal Command

Once you have the manual structure perfectly memorized, you will graduate to using Maven's CLI to build this skeleton for you. 

The tool Maven uses for this is called an **Archetype** (essentially a project template). The command looks like this:

`mvn archetype:generate -DgroupId=com.djordje.learning -DartifactId=mvc-project -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

*   **`mvn archetype:generate`**: Tells Maven to generate a new project from a template.
*   **`-D`**: This is how you pass variables (Properties) into a Maven command from the terminal. 
*   It automatically creates the `src/main/java` structure, the `pom.xml`, and even a basic `App.java` file based on the coordinates you provide.

---

