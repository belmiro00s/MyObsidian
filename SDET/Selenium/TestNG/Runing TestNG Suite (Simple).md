
 
Your `testng.xml` exemple **src/test/resources**

```
<suite name="MyTestSuite">
    <test name="AllTests">
        <classes>
            <class name="tests.LoginTest" />
            <class name="tests.SignupTest" />
        </classes>
    </test>
</suite>
```

### 🛠️ Commands to Run TestNG Tests

You can run TestNG tests in a few ways:

#### 1. **Using Maven**

If you are using Maven and have TestNG configured in your `pom.xml`, you can do:
**mvn test**

This will automatically pick up the `testng.xml` if you have it configured under `src/test/resources`.

To run a specific suite:
**mvn test -DsuiteXmlFile=testng.xml**


To run a specific method:
**mvn -Dtest=tests.LoginTest#loginWithValidCredentials test**





