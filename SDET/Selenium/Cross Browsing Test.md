
# Browser Factory Setup 

## Create a utility class to handle different browser configurations

Exemple: 

`package base;  
  
import org.openqa.selenium.WebDriver;  
import org.openqa.selenium.chrome.ChromeDriver;  
import org.openqa.selenium.edge.EdgeDriver;  
import org.openqa.selenium.firefox.FirefoxDriver;  
import org.openqa.selenium.safari.SafariDriver;  
import io.github.bonigarcia.wdm.WebDriverManager;  
import org.testng.annotations.*;  
  
public class BaseTest {  
  
    protected WebDriver driver;  
  
    @Parameters("browser")   // The browsers came from the testng.xml
    @BeforeMethod  
    public void setUp(String browser) {  
        switch (browser.toLowerCase()) {  
            case "chrome":  
                WebDriverManager.chromedriver().setup();  
                driver = new ChromeDriver();  
                break;  
            case "firefox":  
                WebDriverManager.firefoxdriver().setup();  
                driver = new FirefoxDriver();  
                break;  
            case "edge":  
                WebDriverManager.edgedriver().setup();  
                driver = new EdgeDriver();  
                break;  
            case "safari":  
                WebDriverManager.safaridriver().setup();  
                driver = new SafariDriver();  
                break;  
            default:  
                throw new IllegalArgumentException("Browser not supported:" + browser);  
        }  
    }  
  
    @AfterMethod  
        public void tearDown(){  
        if(driver != null){  
            driver.quit();  
        }  
    }  
}



## TestNG configuration Methods


Create testng.xml to handle with different browsers an run in parallel 
```
<suite name="CrossBrowserSuite" parallel="tests" thread-count="4">  
    <test name="ChromeTest">  
        <parameter name="browser" value="chrome" />  
        <classes>  
            <class name="com.practicetestautomation.tests.login.NegativeLoginTests"/>  
            <class name="com.practicetestautomation.tests.login.PositiveLoginTests"/>  
        </classes>  
    </test>  
    <test name="FirefoxTest">  
        <parameter name="browser" value="firefox" />  
        <classes>  
            <class name="com.practicetestautomation.tests.login.NegativeLoginTests"/>  
            <class name="com.practicetestautomation.tests.login.PositiveLoginTests"/>  
        </classes>  
    </test>  
    <test name="EdgeTest">  
        <parameter name="browser" value="edge" />  
        <classes>  
            <class name="com.practicetestautomation.tests.login.NegativeLoginTests"/>  
            <class name="com.practicetestautomation.tests.login.PositiveLoginTests"/>  
        </classes>  
    </test>  
    <test name="SafariTest">  
        <parameter name="browser" value="safari" />  
        <classes>  
            <class name="com.practicetestautomation.tests.login.NegativeLoginTests"/>  
            <class name="com.practicetestautomation.tests.login.PositiveLoginTests"/>  
        </classes>  
    </test>  
</suite>
```