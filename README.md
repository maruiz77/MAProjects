# MAProjects

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.util.List;

public class AmazonTest {
    public static void main(String[] args) {
        // Set up web driver
        System.setProperty("webdriver.chrome.driver", "path/to/chromedriver");
        WebDriver driver = new ChromeDriver();

        // Go to amazon.com
        driver.get("https://www.amazon.com");

        // Search for "Gel Pen"
        WebElement searchBox = driver.findElement(By.id("twotabsearchtextbox"));
        searchBox.sendKeys("Gel Pen");
        searchBox.submit();

        // Validate search suggestions contain "gel pens"
        WebDriverWait wait = new WebDriverWait(driver, 10);
        wait.until(ExpectedConditions.visibilityOfElementLocated(By.cssSelector(".s-suggestion")));
        List<WebElement> searchSuggestions = driver.findElements(By.cssSelector(".s-suggestion"));
        for (WebElement suggestion : searchSuggestions) {
            if (!suggestion.getText().toLowerCase().contains("gel pens")) {
                throw new AssertionError("Search suggestions do not contain 'gel pens'");
            }
        }

        // Choose last gel pen option from the suggestions
        WebElement lastGelPenOption = searchSuggestions.get(searchSuggestions.size() - 1);
        lastGelPenOption.click();

        // Check the search result ensuring every product item has "Pen" in its title
        wait.until(ExpectedConditions.visibilityOfElementLocated(By.cssSelector(".s-result-item")));
        List<WebElement> searchResults = driver.findElements(By.cssSelector(".s-result-item"));
        for (WebElement result : searchResults) {
            if (!result.findElement(By.cssSelector(".a-size-medium")).getText().contains("Pen")) {
                throw new AssertionError("Product item does not have 'Pen' in its title");
            }
        }

        // Click on the item from that has lowest price in the search list
        // Change quantity to 2 then add to cart
        // Empty Cart
        // Validate "Your item was removed from shopping cart" message
        // Add the implementation for these steps here

        // Close the driver
        driver.quit();
    }
}
