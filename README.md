# Automate
1. Automate any web login flow using Selenium
import org.openqa.selenium.By;

import org.openqa.selenium.WebDriver;

import org.openqa.selenium.WebElement;

import org.openqa.selenium.chrome.ChromeDriver;

 

public class WebLoginAutomation {

 

    public static void main(String[] args) {

        // Set the path to your chromedriver executable

        System.setProperty("webdriver.chrome.driver", "path/to/chromedriver");

 

        // Initialize the Chrome driver

        WebDriver driver = new ChromeDriver();

 

        try {

            // Navigate to the login page

            driver.get("https://example.com/login");

 

            // Maximize the browser window

            driver.manage().window().maximize();

 

            // Find username and password fields and input credentials

            WebElement username = driver.findElement(By.id("username"));

            WebElement password = driver.findElement(By.id("password"));

 

            username.sendKeys("yourUsername");

            password.sendKeys("yourPassword");

 

            // Click the login button

            WebElement loginButton = driver.findElement(By.id("loginBtn"));

            loginButton.click();

 

            // Optionally, add a simple wait or check to verify login success

            Thread.sleep(3000); // Simple wait, replace with WebDriverWait in real use

 

            if (driver.getCurrentUrl().contains("dashboard")) {

                System.out.println("Login successful!");

            } else {

                System.out.println("Login failed or redirected incorrectly.");

            }

 

        } catch (Exception e) {

            e.printStackTrace();

        } finally {

            // Close the browser

            driver.quit();

        }

    }

}

2.  Automate any POST API

import io.restassured.RestAssured;

import io.restassured.response.Response;

import org.json.JSONObject;

import org.junit.jupiter.api.Test;

 

import static io.restassured.RestAssured.given;

import static org.junit.jupiter.api.Assertions.assertEquals;

import static org.junit.jupiter.api.Assertions.assertTrue;

 

public class LoginApiTest {

 

    @Test

    public void testLoginSuccess() {

        // Set Base URI

        RestAssured.baseURI = "https://api.ecommerce.com/v1";

 

        // Create JSON payload

        JSONObject requestBody = new JSONObject();

        requestBody.put("email", "user@example.com");

        requestBody.put("password", "securePassword123");

 

        // Make POST request

        Response response = given()

            .header("Content-Type", "application/json")

            .body(requestBody.toString())

            .when()

            .post("/auth/login");

 

        // Assertions

        assertEquals(200, response.getStatusCode(), "Expected 200 OK");

        String token = response.jsonPath().getString("token");

        assertTrue(token != null && token.length() > 10, "Token should be returned");

    }

}

3. Automate any mobile app flow

import io.appium.java_client.android.AndroidDriver;

import io.appium.java_client.android.options.UiAutomator2Options;

import org.openqa.selenium.By;

import org.openqa.selenium.remote.RemoteWebDriver;

import org.testng.annotations.*;

 

import java.net.MalformedURLException;

import java.net.URL;

import java.time.Duration;

 

public class LoginTest {

 

    private AndroidDriver driver;

 

    @BeforeClass

    public void setup() throws MalformedURLException {

        UiAutomator2Options options = new UiAutomator2Options()

                .setDeviceName("Android Emulator") // or your device name

                .setAppPackage("com.ecommerce.app") // replace with your app's package

                .setAppActivity("com.ecommerce.app.MainActivity") // replace with your app's main activity

                .setPlatformName("Android")

                .setAutomationName("UiAutomator2");

 

        driver = new AndroidDriver(new URL("http://127.0.0.1:4723/wd/hub"), options);

        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));

    }

 

    @Test

    public void testLoginFlow() {

        // Find and interact with login form

        driver.findElement(By.id("com.ecommerce.app:id/email")).sendKeys("user@example.com");

        driver.findElement(By.id("com.ecommerce.app:id/password")).sendKeys("Password123");

        driver.findElement(By.id("com.ecommerce.app:id/login_button")).click();

 

        // Assertion - check for home screen element

        boolean isLoggedIn = driver.findElements(By.id("com.ecommerce.app:id/home_screen")).size() > 0;

        assert isLoggedIn : "Login failed or home screen not displayed.";

    }

 

    @AfterClass

    public void tearDown() {

        if (driver != null) {

            driver.quit();

        }

    }

}


