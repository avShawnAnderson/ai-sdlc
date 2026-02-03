# SpecFlow Framework

This directory contains examples and best practices for using AI with SpecFlow (Behavior-Driven Development framework).

## About SpecFlow

SpecFlow is a BDD framework for .NET that allows you to:

- Write specifications in Gherkin syntax
- Create executable specifications
- Bridge communication between business and technical teams
- Generate automated tests from scenarios

## AI Integration

AI tools can significantly accelerate SpecFlow development:

1. **Feature File Generation**: Create Gherkin scenarios from requirements
2. **Step Definition Creation**: Generate step implementation code
3. **Test Data Generation**: Create test data for scenarios
4. **Scenario Enhancement**: Expand basic scenarios with edge cases

## Using AI with SpecFlow

### Generating Feature Files

Prompt for feature file creation:

```
Create a SpecFlow feature file for [feature description].

Requirements:
- [requirement 1]
- [requirement 2]

Include scenarios for:
- Happy path
- Error cases
- Edge cases

Use Gherkin syntax with Given-When-Then format.
```

Example output:
```gherkin
Feature: User Login
  As a registered user
  I want to log in to my account
  So that I can access my profile

  Scenario: Successful login with valid credentials
    Given I am on the login page
    And I have a registered account
    When I enter my username "john.doe@example.com"
    And I enter my password "SecurePass123"
    And I click the login button
    Then I should be redirected to my dashboard
    And I should see a welcome message "Welcome, John"

  Scenario: Login fails with invalid password
    Given I am on the login page
    When I enter my username "john.doe@example.com"
    And I enter an incorrect password "WrongPass"
    And I click the login button
    Then I should see an error message "Invalid credentials"
    And I should remain on the login page
```

### Generating Step Definitions

Generate C# step definitions:

```
Create SpecFlow step definitions in C# for this scenario:

[PASTE SCENARIO]

Use:
- Page Object Pattern
- Dependency injection
- Async/await
- Appropriate assertions
```

Example output:
```csharp
[Binding]
public class LoginSteps
{
    private readonly IWebDriver _driver;
    private readonly LoginPage _loginPage;
    private readonly DashboardPage _dashboardPage;

    public LoginSteps(IWebDriver driver)
    {
        _driver = driver;
        _loginPage = new LoginPage(driver);
        _dashboardPage = new DashboardPage(driver);
    }

    [Given(@"I am on the login page")]
    public void GivenIAmOnTheLoginPage()
    {
        _loginPage.Navigate();
    }

    [When(@"I enter my username ""(.*)""")]
    public void WhenIEnterMyUsername(string username)
    {
        _loginPage.EnterUsername(username);
    }

    [When(@"I enter my password ""(.*)""")]
    public void WhenIEnterMyPassword(string password)
    {
        _loginPage.EnterPassword(password);
    }

    [When(@"I click the login button")]
    public void WhenIClickTheLoginButton()
    {
        _loginPage.ClickLogin();
    }

    [Then(@"I should be redirected to my dashboard")]
    public void ThenIShouldBeRedirectedToMyDashboard()
    {
        Assert.True(_dashboardPage.IsDisplayed());
    }

    [Then(@"I should see a welcome message ""(.*)""")]
    public void ThenIShouldSeeAWelcomeMessage(string expectedMessage)
    {
        var actualMessage = _dashboardPage.GetWelcomeMessage();
        Assert.Equal(expectedMessage, actualMessage);
    }
}
```

### Enhancing Scenarios

Ask AI to add edge cases:

```
Review this SpecFlow scenario and suggest additional scenarios for edge cases and error conditions:

[PASTE EXISTING SCENARIOS]
```

## Best Practices

1. **Clear Scenarios**: Write scenarios that are easy to understand
2. **Reusable Steps**: Create step definitions that can be reused
3. **Meaningful Names**: Use descriptive names for scenarios
4. **Test Data Management**: Use scenario context for data sharing
5. **Page Objects**: Implement page object pattern for UI tests
6. **Independent Tests**: Ensure scenarios don't depend on each other

## SpecFlow with AI Tips

- Provide business requirements context
- Specify the domain language
- Ask for comprehensive coverage
- Request both positive and negative scenarios
- Include data tables for complex data
- Use scenario outlines for parameterized tests

## Example Prompts

### Feature from User Story
```
Convert this user story to a SpecFlow feature file:

As a [role]
I want [goal]
So that [benefit]

Include acceptance criteria as scenarios.
```

### Step Definitions with Hooks
```
Generate SpecFlow step definitions including:
- Before/After scenario hooks
- Browser setup/teardown
- Screenshot on failure
- Logging
```

### Data-Driven Tests
```
Create a scenario outline with examples table for testing [feature] with multiple data sets.
```

## Resources

- [SpecFlow Documentation](https://docs.specflow.org/)
- Gherkin syntax reference
- BDD best practices
- Page Object Pattern
