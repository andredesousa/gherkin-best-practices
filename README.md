# Gherkin Best Practices

[Gherkin](https://cucumber.io/docs/gherkin/reference/) is the domain-specific language for writing behaviour scenarios.

This is a guideline of best practices about Gherkin and BDD that you can apply to your projects.
These tips are based on Cucumber documentation, books, articles and professional experience.

## Table of Contents

1. [Write declarative features instead of imperative features](#write-declarative-features-instead-of-imperative-features)
2. [Use meaningful feature and scenario names](#use-meaningful-feature-and-scenario-names)
3. [Add good scenario descriptions](#add-good-scenario-descriptions)
4. [Write the scenario before writing the code](#write-the-scenario-before-writing-the-code)
5. [Do not write scenarios in isolation](#do-not-write-scenarios-in-isolation)
6. [Avoid high-level and vague scenarios](#avoid-high-level-and-vague-scenarios)
7. [Make scenarios independent and deterministic](#make-scenarios-independent-and-deterministic)
8. [Remove unneeded scenarios](#remove-unneeded-scenarios)
9. [Structure large scenarios with comments](#structure-large-scenarios-with-comments)
10. [Use backgrounds to reduce the number of steps](#use-backgrounds-to-reduce-the-number-of-steps)
11. [Avoid overuse of scenario outline](#avoid-overuse-of-scenario-outline)
12. [Limit the number of scenarios per feature](#limit-the-number-of-scenarios-per-feature)
13. [Limit the number of steps per scenario](#limit-the-number-of-steps-per-scenario)
14. [Avoid using conjunctive steps](#avoid-using-conjunctive-steps)
15. [Use And and But instead of repeating the same keyword](#use-and-and-but-instead-of-repeating-the-same-keyword)
16. [No clear separation between given/when/then](#no-clear-separation-between-givenwhenthen)
17. [Refactor and reuse step definitions](#refactor-and-reuse-step-definitions)
18. [Avoid testing several rules at the same time](#avoid-testing-several-rules-at-the-same-time)
19. [Define the actor that will use the system](#define-the-actor-that-will-use-the-system)
20. [Use your native language to write scenarios](#use-your-native-language-to-write-scenarios)
21. [Use tags to organize your features and scenarios](#use-tags-to-organize-your-features-and-scenarios)
22. [Use lint rules to check the given/when/then syntax](#use-lint-rules-to-check-the-givenwhenthen-syntax)
23. [Avoid testing through the UI](#avoid-testing-through-the-ui)
24. [Features are not user stories](#features-are-not-user-stories)
25. [Organize the test code in layers](#organize-the-test-code-in-layers)

## Write declarative features instead of imperative features

Declarative style describes the behaviour of the application, rather than the implementation details.
A declarative style helps you focus on the value that the customer is getting, rather than the keystrokes they will use.
Imperative tests communicate details, and in some contexts this style of test is appropriate.
Check the next example:

```gherkin
Given I am on the login page
When I type "paidPattya@example.com" in the email field
And I type "validPassword123" in the password field
And I press the "Submit" button
Then I see "FreeArticle1" and "PaidArticle1" on the home page
```

Each step is a precise instruction. The inputs and expected results are specified exactly.
The purpose of this scenario is to check the articles, and it isn't relevant what the password is.
The lesson here is to focus on the behaviour of the story and steer clear of incidental details which don't contribute anything.
You can replace these "login" steps by "When I sign in with valid credentials".

## Use meaningful feature and scenario names

Both scenario and feature names are used in the reports, so it really helps if you do not stick to a single-word-says-all approach.
Good names summarize the steps of the scenario and help the reader to have a faster overview about the scenario goal.
An expressive test name is helpful when a test fails.
Try to summarize the gist of a scenario in a single line.

## Add good scenario descriptions

If you cannot put enough information in the feature or scenario name, consider providing a more elaborate description.
You can add any kind of text below a *Feature* up until the first Scenario, *Scenario Outline* or *Background* statement.
If you want to add a description at the beginning of a scenario you can do so using a docstring.
Good descriptions will help you in the future to understand what the scenario does.

## Write the scenario before writing the code

Writing scenarios before coding encourages people with different perspectives to be involved in defining the behaviour of the software.
A product owner or a business stakeholder will read it with one perspective, a developer with another one, and a tester with a third.
It makes it easier for people to figure out whether they disagree about something or whether there's uncertainty.

## Do not write scenarios in isolation

If your business analyst or product owner writes all the Gherkin on their own, you will be missing out on the whole team's input.
Scenarios written by non-technical team members very often can't be automated without being changed.
On the other hand, test scenarios written only by developers or QA engineers often have lots of unimportant details that make it difficult to understand the core test idea.
Work as a team.
Involving different types of specialists in scenario creation will help people with different perspectives share their vision and help you create well-defined descriptions.

## Avoid high-level and vague scenarios

Rambling long scenarios with lots of incidental details can ruin a good story.
Often times the scenarios are written as a test, rather than documentation, which can lead to vague scenarios.
The other end of the spectrum is that you can't really trust the scenario, or you can't tell what it would actually do if you ran it.

```gherkin
Given I have an account  
When I withdraw some money
Then the balance should be the original balance minus the amount withdrawn
```

There's no concrete value in there.
It doesn't say what my balance was, it didn't say how much I withdrew, it's basically expressing the business rule, but using the "Given, When, Then" format.
It's arguably a bit better than too much detail but finding that balance between too much detail and not enough detail is a bit of an art rather than a science.

## Make scenarios independent and deterministic

There shouldn't be any sort of coupling between scenarios.
The main source of such coupling is state that persists between scenarios.
This can be accidental, or worse, by design.
For example, one scenario could step through adding a record to a database, and subsequent scenarios depend on the existence of that record.
This may work but will create a problem if the order in which scenarios run changes, or they are run in parallel.
Scenarios need to be completely independent.
Each time a scenario runs, it should run the same, giving identical results.
If you have non-deterministic scenarios, find out why and fix them.

## Remove unneeded scenarios

When you start creating your software product, you can create scenarios that test some basic functionality.

```gherkin
Given the user is logged in
And their bank account is empty
When the user checks the balance
Then the balance should be zero
```

Most of the time, these kinds of tests are obvious and exist because you needed something to start the implementation.
As the time progresses, you create more and more scenarios, and it is likely that basic functionality is covered by some other tests.
So, keeping old simple scenarios will not add any significant value to helping your teammates understand how the product works.
Delete these scenarios or rephrase them to something more valuable.

## Structure large scenarios with comments

Usually, it is best to keep your scenarios as short as possible, but sometimes there's just no easy way around writing that lengthy test.
Comments are considered a best practice, but if you are adding a comment, it's because it's not self-explanatory and you should choose a better way to implement it.
Good comments are informative comments, when be useful to provide basic information.

## Use backgrounds to reduce the number of steps

*Background* simplifies adding the same steps to multiple scenarios in each feature.
If you have multiple scenarios performing the same steps in the beginning, you can extract those steps into a *Background* section.
Each feature can have one *Background* section and it should be positioned before all scenarios and scenario outlines.

```gherkin
Feature: Subscribers see different results based on their subscription level

  Background:
    Given I am on the login page
    And I sign in with valid credentials

  Scenario: Free subscribers see only the free articles

  Scenario: Subscriber with a paid subscription can access free and paid articles
```

## Avoid overuse of scenario outline

*Scenario Outline* is a type of scenario where input data is specified.
They are very practical because, thanks to this, it's not necessary to write a scenario by input data.
Overuse of scenario outlines tends to result in lots of scenarios.
This is one of the main things that leads to slow your tests.
Here's the rule of thumb we practice: if you're using scenario outlines, don't do it with a UI test or anything that's slow.

## Limit the number of scenarios per feature

A long feature file can make it difficult to understand how a feature works.
Keep some reasonable number of scenarios per file.
Use scenario outlines to check different cases.
If you are testing some complex functionality that requires lots of tests, think of dividing it into sub features and create a file for each sub feature.
You can organize files so that all the features related to a specific functionality are grouped in a package or a directory.

## Limit the number of steps per scenario

Declarative steps state what action should happen without providing all the information for how it will happen.
They are behaviour-driven because they express action at a higher level.
When trying to reduce step count, ask yourself if your steps can be written more declaratively.
Consider decreasing their number to some reasonable value, say, 2-3 "Ands" for one Given, When, or Then step.
Another example, you can replace a long procedure with a table.

## Avoid using conjunctive steps

When you encounter a step that contains two actions together with an "and", you should probably break it into two steps.
Consider the following case:

```gherkin
Given I am on the login page
When I sign in with valid credentials
Then I see the "Welcome User" message and the logout button
```

Break the last step into 2 steps as in the example below.
Sticking to one action per step makes your steps more modular and increases reusability.
This is not a mandatory rule though. There may be reasons for conjunctive steps.
However, most of the time it's best to avoid them.

## Use And and But instead of repeating the same keyword

If you have multiple Given/When/Then statements, you should combine them using *And* and *But* steps instead of repeating the same keyword over and over again.
This way the overall structure of your scenario remains clearer.
See the following example:

```gherkin
Given I am on the login page
When I sign in with valid credentials
Then I see the "Welcome User" message
And I see the logout button
But I not see "You are not logged in" message
```

## No clear separation between given/when/then

A lot of people struggle with the difference between them, but there is no difference.
The only reason there is "Given, When, Then" is to make it easier for people to write something that reads better.
So, what you say is "Given" is your context, "When" is the action or the event, and "Then" is the outcome or expected outcome.
Think of "Given" as the past, "When" is the present, and "Then" as the near future.

## Refactor and reuse step definitions

Scenarios can include steps that sound very similar or even the same.
Look for opportunities to generalize your steps and reuse them.
You want to accumulate a reusable library of steps so that writing additional features takes less and less effort over time.
It is recommend reusing steps when it is possible.
In the other hand, it is the opportunity to refactor the speech and steps to reflect better understanding of the domain.

## Avoid testing several rules at the same time

Testing multiple things can blur the essence of your scenario and lead to uncertainty and misunderstandings.
An easy way to check this is to look at the number of When-Then pairs.
A good scenario has only one When-Then pair. Multiple Whens-Thens can indicate a problem.
Another way to check if your scenario is clear enough is to show it to somebody who is not familiar with your product and see if they can explain its behaviour.
If the scenario is not good enough, it will cause misunderstanding during test creation and runs.

## Define the actor that will use the system

There are two approaches to writing test steps: first-person and third-person.
Using the personal pronoun "I" might help you put yourself in the users' shoes.
However, most of the modern applications are multi-user, and putting "I" in a scenario can be confusing as it might be unclear what exact user you are speaking about.
We all do it, but the scenarios tend to be richer and more accurate when you avoid the personal pronoun.
This will remind you about the user role in the application, avoid misleading assumptions, and help you provide info in more structured way.
Don't use both first-person and third-person pronouns in one scenario.

## Use your native language to write scenarios

Gherkin is not limited to writing the scenarios in English.
Like the conventions followed in English, you can write the scenarios in multiple human languages.
Gherkin uses a set of special keywords to give structure and meaning to executable specifications.
Each keyword is translated to many spoken languages.
To improve readability and flow, some languages may have more than one translation for any given keyword.

## Use tags to organize your features and scenarios

Tags are a great way to organize your features and scenarios in non-functional ways.
The advantage of separating them this way is that you can selectively run scenarios at different times and/or frequencies.
You can make combinations of tags as per your requirement and execute the scenarios/features selectively.
For example, you can use @smoke, @integration, @functional, @e2e, and any other tags you like.
Using tags let you keep a feature's scenarios together structurally but run them separately.
A *Feature* or *Scenario* block can have multiple tags, separated with spaces or newlines.
If you add a tag on a *Feature*, it will automatically be added to all *Scenarios* and *Scenario Outlines* of that feature.

## Use lint rules to check the given/when/then syntax

Adopting some writing rules as a style guide will help you create scenarios that are consistent and easy to understand.
This is especially important when you have multiple scenario authors in a team.
Having lint rules in place means that you will get a nice error when you are doing something that you should not be.
This will enforce consistency in your tests and readability.
You can use [Gherkin lint](https://www.npmjs.com/package/gherkin-lint) library to force a consistent style for your tests.

## Avoid testing through the UI

Testing through the user interface can be bad for several reasons.
First, UIs tend to change a lot more than the underlying business logic.
New trends are appearing all the time and you keep trying out different things to please your users.
These tests are also incredibly slow.
When you write your Gherkin for UI, you are stuck forever at the top of the testing pyramid.
There's no way for you to go further down and test things underneath the UI, which is less brittle and a lot faster.
Using that language of clicking buttons and filling things in is just of a generic domain language of any user interface.
That language does not provide the intent of the business rule under test.

## Features are not user stories

The relationship between user stories and feature files is not well understood.
Most features require the delivery of several user stories.
So, you could choose just one to put at the top of your feature file if it makes sense.
As the time progresses, you create more and more scenarios for that feature.
So, keeping old scenarios will not add any significant value to helping your teammates understand how the feature works.
Instead of adding new scenarios, you should improve existing scenarios and only add new ones when necessary.

## Organize the test code in layers

It's important to use a multi-layered approach to improve the scalability of your tests.
At the high-level business layer, you start by defining your test cases using consistent business terminology broken up in "steps".
The technical layer is relevant after you've written the test cases.
The next step is to write these test cases or steps using page object models or journey patterns and test them with any automation tool.

## Bibliography

- [10 easy ways to fail at BDD](https://cucumber.io/blog/bdd/10-easy-ways-to-fail-at-bdd/)
- [15 Expert Tips for Using Cucumber](https://blog.engineyard.com/15-expert-tips-for-using-cucumber)
- [5 tips to automate your tests at scale](https://cucumber.io/blog/test-automation/building-test-automation-that-scales/)
- [9 tips for improving Cucumber test readability](https://www.foreach.be/blog/9-tips-improving-cucumber-test-readability)
- [A Guide to Cucumber Best Practices](https://dzone.com/articles/a-guide-to-good-cucumber-practices)
- [BDD 101: The Gherkin Language](https://automationpanda.com/2017/01/26/bdd-101-the-gherkin-language/)
- [BDD Cucumber Features Best Practices](https://www.linkedin.com/pulse/bdd-cucumber-features-best-practices-liraz-shay/)
- [Best practices for scenario writing](https://support.smartbear.com/cucumberstudio/docs/tests/best-practices.html)
- [Cucumber for BDD - Using Anti Patterns](https://cucumber.io/blog/bdd/cucumber-anti-patterns-part-two/)
- [Declarative vs Imperative Gherkin Scenarios for Cucumber](https://itsadeliverything.com/declarative-vs-imperative-gherkin-scenarios-for-cucumber)
- [Gherkin Language - Use Your Language to Describe Test Cases](https://specflow.org/learn/gherkin/)
- [How to Measure the ROI of BDD and TDD](https://cucumber.io/blog/bdd/how-to-measure-the-roi-of-bdd-and-tdd/)
- [Introducing BDD](https://dannorth.net/introducing-bdd/)
- [QA: Gherkins Standards and Best Practices](https://github.com/department-of-veterans-affairs/ascent-sample/wiki/QA-:-Gherkins-Standards-and-Best-Practices)
- [The 3 most common mistakes writing Gherkin features](https://www.spritecloud.com/the-3-most-common-mistakes-writing-gherkin-features/)
- [The Best Anti-Patterns for BDD](https://cucumber.io/blog/bdd/cucumber-antipatterns-part-one/)
- [Top 5 Cucumber Best Practices For Selenium Automation](https://www.lambdatest.com/blog/cucumber-best-practices/)
- [Writing better Gherkin](https://cucumber.io/docs/bdd/better-gherkin/)
