package com.FitPeo;

import java.util.concurrent.TimeUnit;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterTest;
import org.testng.annotations.BeforeTest;
import io.github.bonigarcia.wdm.WebDriverManager;

public class BaseClass {
    // Global Driver
    public static WebDriver driver;

    // Application URL
    public static String Application_Fitpeo = "https://fitpeo.com/";

    // Browser Setup
    @BeforeTest
    public void SetUp() {
        // Auto Browser Launching using WebDriverManager
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();

        // Navigating to FitPeo Home page
        driver.get(Application_Fitpeo);

        // Browser configurations
        driver.manage().deleteAllCookies();
        driver.manage().timeouts().pageLoadTimeout(120, TimeUnit.SECONDS);
        driver.manage().timeouts().implicitlyWait(120, TimeUnit.SECONDS);
        driver.manage().window().maximize();
    }

    // Tear Down the Browser After Tests
    @AfterTest
    public void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }
}
