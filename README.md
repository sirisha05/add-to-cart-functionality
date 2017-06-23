package com.saibersys.task;



import org.testng.annotations.Test;
import org.testng.annotations.BeforeMethod;

import static org.testng.Assert.assertTrue;

import java.util.concurrent.TimeUnit;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.annotations.AfterMethod;

public class AddToCartFunctionality {
	
	WebDriver driver;
	String baseUrl;
	WebDriverWait wait;
	
  @BeforeMethod
  public void beforeMethod() {
	  	System.setProperty("webdriver.gecko.driver","E:\\Selenium\\geckodriver-v0.14.0-win64\\geckodriver.exe");
		driver = new FirefoxDriver();
		baseUrl = "https://disneyworld.disney.go.com/tickets/";
		driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
		driver.manage().window().maximize();
		wait = new WebDriverWait(driver, 10);
  }

  @Test
  public void addToCartFunction() throws InterruptedException{
		int numberOfDays = 2;
		int numberOfTickets = 3;
	  
	    driver.get(baseUrl);
	    driver.findElement(By.id("rateCardTitle-all-guests"));
		selectNumberOfDays(2);
		selectNumberOfTickets(3);
		selectOneDayOnePark();
		clickAddToCart();
		
		//Validations
		waitForElementToBeClickable(".//*[@id='checkOutLoginButton']");
		String firstProductText = driver.findElement(By.xpath(".//*[contains(@id,'ticket-ThemePark-2Day')]")).getText();
		//System.out.println(firstProductText);
		
		
		assertTrue(firstProductText.contains(numberOfDays+"-Day") &&
				firstProductText.contains("1 Park Per Day"));
		
		String qtySelected = driver.findElement(By.xpath(".//*[contains(@id,'ticket-ThemePark-2Day')]/descendant::input[@name='quantity']"))
				.getAttribute("value");
		assertTrue(qtySelected.equals(String.valueOf(3)));
		
	   
  }
  
 

  
  private void clickAddToCart() {
	  	wait.until(ExpectedConditions.elementToBeClickable(By.id("addToCart")));
	  	
	  	driver.findElement(By.id("addToCart")).click();
	 
  }
  
  private void selectOneDayOnePark() {
	  waitForElementToBeClickable(".//*[@id='selectThemeParkTicketModule']/descendant::button[@data-ticket-id='myw_']");
	  driver.findElement(By.xpath(".//*[@id='selectThemeParkTicketModule']/descendant::button[@data-ticket-id='myw_']")).click();
  }
  
  private void selectNumberOfTickets(int number) {
	  waitForElementToBeClickable(".//*[@id='numberOfTicketsModule']/descendant::button[@aria-label='Add Adults count to mix']");
		for (int i = 0; i < number; i++) {
			driver.findElement(By.xpath(".//*[@id='numberOfTicketsModule']/descendant::button[@aria-label='Add Adults count to mix']")).click();
			waitForElementToBeClickable(".//*[@id='numberOfTicketsModule']/descendant::button[@aria-label='Add Adults count to mix']");
		}
  }
  
  private void selectNumberOfDays(int number) {
	  waitForElementToBeClickable(".//*[@class='pepDayScroller_dayNum' and text()='3']");
	  driver.findElement(By.xpath(".//*[@class='pepDayScroller_dayNum' and text()='"+ number +"']")).click();
  }
  
  public void waitForElementToBeClickable(String xpath) {
	  wait.until(ExpectedConditions.elementToBeClickable(By.xpath(xpath))); 
  }
  
  @AfterMethod
  public void afterMethod() {
	  
  
  }

}
