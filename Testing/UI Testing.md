# Testing UI elements 

# Contents

* Introduction
* Prerequisites
* What makes a good UI test
* Creating and running tests in Selenium
* Adding to Team City build.
* Screencast

# Introduction

In the Agile age of Software delivery, Test Automation is becoming more of a necessity than a luxary. The complexity of Software is making Manual Testing much too time consuming and costly. In response to this, Digital Tulip has taken an Automation approach to UI testing. 

UI tests use Selenium as it's foundation. However framworks such as Mocha and Phantom are also intregrated to enhance Testing quality. Phantom allows the UI tests to be added to the Team City build. 

# Prerequisites

Before continuing, the following prerequisits must be met - 

1. fip_banking_node project cloned with repository permissions.
2. Team City account with access to Digital Tulip tasks.
3. Sound knowlege of Selenium, Mocha, Phantom and UI Test Automation in general.

# What makes a good UI test

It is important to test UI functionality in the best way possible. Here are some key points to consider when writing a UI test - 

1. Clear and consise. It can be easy to overcomplicate what is supposed to be a simple Test. This adds to the risk of not understanding the true crux of the test, and thus the expected outcome. 'Is the My Finance Pal logo displayed' is much more clearer and consise than 'Load My Finance Pal and check for the presence of the logo'.

2. Short. Breaking long Tests down into shorter ones alows for bugs to be more identifiable. Writing a test that checks for five images would be bad practice, as a failing test would not identify which image contains the bug.  

3. Coverage. It is considered impossible to achieve 100% UI test coverage. However it is important to maximise the level of coverage in order to locate bugs. All website pages should be tested along with each element on the page. A glyphicon is still considered an important element on a screen despite it's lack of functionality.  

4. What is being tested. A UI test is designed to test screen elements only. This is anything that is rendered on the screen and is visible to the user. Testing a Database call would not be considered a suitable UI test.      

5. How to test. The most effective tests are those that identify bugs. The following would be considered good UI tests -  
* Is an element displayed on the screen ? 
* Does a button navigate the user to the correct screen ? 
* Is an element rendered correctly ?   
* Do fields allows characters to be inserted ? 
* Is an error message invoked for incorrect inputs ? 
* Does the screen size change for mobile viewing ? 

6. Efficiency. Aiming to minimise the test run time is crucial for these tests as they will be addded to the Team City build. 

  