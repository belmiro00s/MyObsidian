

### Why use the Maven Surefire Plugin?

The Maven Surefire Plugin is used primarily for **executing unit and integration tests** during the build lifecycle of a Maven project. It also handles **generating reports** for these tests. In your project's configuration, it's specifically set up to run tests defined in TestNG XML suite files. This allows for automated and configurable test execution as part of your development process.


## What Surefire does:

- Runs unit tests during the `test` phase of Maven build
[- Generates test reports (XML and text formats)](obsidian://open?vault=Obsidian%20Vault&file=SDET%2FSelenium%2FTestNG%2FTestNG%20Reports)
- Handles test discovery and execution
- Manages test classpath and JVM settings



### Understanding Your Configuration

Your `pom.xml` snippet looks like this:

```
<properties>
    <maven.compiler.source>24</maven.compiler.source>
    <maven.compiler.target>24</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <suiteXmlFile>smoketests.xml</suiteXmlFile>
</properties>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.5.3</version>
            <configuration>
                <suiteXmlFiles>
                    <suiteXmlFile>src/test/java/resources/TestSuites/${suiteXmlFile}</suiteXmlFile>
                </suiteXmlFiles>
            </configuration>
        </plugin>
    </plugins>
</build>

```

Key points from this configuration:

- **Default Test Suite:** By default, if you don't specify otherwise, the `suiteXmlFile` property is set to `smoketests.xml`. This means running `mvn test` will execute the tests defined in `src/test/java/resources/TestSuites/smoketests.xml`.
    
- **Dynamic Test Suite Selection:** The `${suiteXmlFile}` placeholder allows you to dynamically choose which TestNG XML file to run from the command line.
    

### Running Tests from the Command Line

To run your tests, you'll use the `mvn` command followed by the `test` goal.

#### 1. Running the Default Test Suite

To run the `smoketests.xml` suite (which is the default specified in your `pom.xml`), simply execute:

```
mvn test

```

Maven will locate the `maven-surefire-plugin`, read its configuration, resolve `${suiteXmlFile}` to `smoketests.xml`, and then execute the tests defined in `src/test/java/resources/TestSuites/smoketests.xml`.

#### 2. Running a Specific Test Suite

To run a _different_ TestNG XML suite (e.g., `testng.xml` as shown in your project structure, or any other suite file you create), you can override the `suiteXmlFile` property using the `-D` flag:

```
mvn test -DsuiteXmlFile=testng.xml

```

In this command:

- `mvn test`: Tells Maven to execute the `test` phase of the build lifecycle, which includes running tests via the Surefire plugin.
    
- `-DsuiteXmlFile=testng.xml`: This is how you pass a system property to Maven. It overrides the `suiteXmlFile` property defined in your `pom.xml` for this particular build. Maven will now resolve `${suiteXmlFile}` to `testng.xml`.
    

Therefore, the Surefire plugin will execute the tests defined in `src/test/java/resources/TestSuites/testng.xml`.

#### Example with Your Project Structure

Based on your project's file structure, you have:

```
├── src
│   └── test
│       ├── java
│       │   └── resources
│       │       └── TestSuites
│       │           ├── smoketests.xml
│       │           └── testng.xml
│       └── ...
└── pom.xml

```

- **To run `smoketests.xml` (default):**
    
    ```
    mvn test
    
    ```
    
- **To run `testng.xml`:**
    
    ```
    mvn test -DsuiteXmlFile=testng.xml
    ```



## TestNG Reports

