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

// Page object Class
package com.FitPeo;

import org.openqa.selenium.Keys;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class Fitpeo_Automation_DropUp extends BaseClass {

    // Constructor to initialize PageFactory elements
    public Fitpeo_Automation_DropUp() {
        super(); // Ensures BaseClass constructor is called
        PageFactory.initElements(driver, this);
    }

    // Locators and Methods

    // Locator for Revenue Calculator
    @FindBy(xpath = "//*[contains(text(),'Revenue Calculator')]")
    WebElement clickRevenueCalculator;

    public void ClickRC() {
        clickRevenueCalculator.click();

        // Scroll using Actions
        Actions actions = new Actions(driver);
        actions.sendKeys(Keys.PAGE_DOWN).perform();
        actions.scrollByAmount(0, 55).perform();
    }

    // Locators for Slider
    @FindBy(xpath = "//*[@class='MuiSlider-root MuiSlider-colorPrimary MuiSlider-sizeMedium css-16i48op']")
    WebElement sliderValueField;

    @FindBy(xpath = "//*[@type='number']")
    WebElement slider;

    public void Slider() {
        // Move Slider
        Actions actions = new Actions(driver);
        actions.clickAndHold(sliderValueField).moveByOffset(-26, 0).release().perform();

        // Verify slider value
        if (slider.getAttribute("value").equals("822")) {
            System.out.println("Slider value updated to 820");
        } else {
            System.out.println("Slider value update failed!");
        }

        // Update slider using text field
        slider.sendKeys(Keys.CONTROL + "a");
        slider.sendKeys(Keys.DELETE);
        slider.sendKeys("560");

        // Validate slider value
        if (slider.getAttribute("value").equals("560")) {
            System.out.println("Slider updated to 560 successfully.");
        } else {
            System.out.println("Slider update to 560 failed!");
        }
        actions.scrollByAmount(0, 20).perform();
    }

    // Locators and Methods for Checkboxes
    @FindBy(xpath = "(//*[@class='PrivateSwitchBase-input css-1m9pwf3'])[1]")
    WebElement checkbox1;

    public void Clickch1() {
        checkbox1.click();
    }

    @FindBy(xpath = "(//*[@class='PrivateSwitchBase-input css-1m9pwf3'])[2]")
    WebElement checkbox2;

    public void Clickch2() {
        checkbox2.click();
        Actions actions = new Actions(driver);
        actions.sendKeys(Keys.PAGE_DOWN).perform();
        actions.scrollByAmount(0, 8).perform();
    }

    @FindBy(xpath = "(//*[@class='PrivateSwitchBase-input css-1m9pwf3'])[3]")
    WebElement checkbox3;

    public void Clickch3() {
        checkbox3.click();
    }

    @FindBy(xpath = "(//*[@class='PrivateSwitchBase-input css-1m9pwf3'])[4]")
    WebElement checkbox4;

    public void Clickch4() {
        checkbox4.click();
    }

    // Locator and Method for Total Recurring Reimbursement
    @FindBy(xpath = "(//*[@class='MuiToolbar-root MuiToolbar-gutters MuiToolbar-regular css-1lnu3ao'])/p[4]")
    WebElement TotalRecurring;

    public void TOtalRM() {
        String totalRecurringText = TotalRecurring.getText();
        System.out.println("Total Recurring Value: " + totalRecurringText);

        // Validate recurring value
        if (totalRecurringText.equals("$95760")) {
            System.out.println("Total recurring reimbursement validated successfully.");
        } else {
            System.out.println("Total recurring reimbursement validation failed!");
        }
    }
}


// TestClass
package com.FitPeo;

import org.testng.annotations.Test;

public class TestCase extends Fitpeo_Automation_DropUp {

    @Test
    public void Launch() throws InterruptedException {
        // Create an object of Fitpeo_Automation_DropUp
        Fitpeo_Automation_DropUp fitpeo = new Fitpeo_Automation_DropUp();

        // Execute Test Steps
        Thread.sleep(2000);
        fitpeo.ClickRC();
        fitpeo.Slider();
        Thread.sleep(10000);

        // Checkbox selections
        fitpeo.Clickch1();
        fitpeo.Clickch2();
        fitpeo.Clickch3();
        fitpeo.Clickch4();

        // Validate Total Recurring Reimbursement
        fitpeo.TOtalRM();
    }
}
