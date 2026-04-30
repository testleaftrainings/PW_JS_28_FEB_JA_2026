 ***1.Your tests are flaky. What steps will you take?***

I would start by identifying whether the issue is environmental, timing-related, or data-related.



Possible causes:

Synchronization issues

Dynamic elements

Network/API delays

Shared test data



Steps:



Analyze logs and trace and video 

Re-run test multiple times

Check waits (remove static waits and use conditional waits)



**2.If a test passes locally but fails in CI, how would you debug and identify the root cause?**

I would compare local and CI environments.



Check:

Browser differences

Headless mode

Missing environment variables

Execution speed differences

Fix:

Run locally in headless mode

Align configs with CI

Add proper waits



**3.In a application, element locators keep changing with every build. How would you design your framework to handle this**



I avoid unstable attributes like dynamic IDs.



Use:

data-testid

Relative XPath

CSS selectors

Best practice:

I collaborate with developers to add stable attributes.



**4.Explain the steps to push the code to GIT. Explain the Git commands to push the code into remote repository**



Initialize repository

git init



Check current status

git status



Add files to staging

git add .



Commit changes

git commit -m "Added login automation feature"



Connect to remote repository

git remote add origin <repo-url>



Push code to remote

git push origin main



to take the code from remote repo:

git pull origin main



To clone the entire project from  remote to your local repo 
git clone <repo-url>



**5.Give me an example when to fixtures in playwright?**



In Playwright, fixtures are used to set up and share common resources across tests, ensuring reusability and clean test design.

Avoid code duplication

Improve readability

Maintain clean test structure

Suppose multiple test cases require a logged-in user. Instead of logging in every test, I create a fixture that handles login once and shares the session



**6.Have you worked in pipeline. Explain the steps to execute automation code using pipeline**



Yes, I have worked with pipelines to automate test execution whenever code changes are pushed.

Testers pushes code

Pipeline gets triggered (based on event like push/PR)

Code is checked out

Dependencies are installed

Automation tests are executed

Reports are generated

Results are shared





**7.How will you execute the same test in different browsers (cross-browser execution) in Playwright? What changes are required in the framework?**

In Playwright, cross-browser execution is supported out  using its configuration file.

projects: \[

&#x20; { name: 'Chromium', use: { browserName: 'chromium' } },

&#x20; { name: 'Firefox', use: { browserName: 'firefox' } },

&#x20; { name: 'WebKit', use: { browserName: 'webkit' } }

]

No major code change

Ensure:

Locators are generic

Avoid browser-specific logic





**8.You may need to use an API key in your pipeline. How would you manage it securely?**

I use GitHub Secrets



Never hardcode

Access via environment variables





**9.Explain your current automation framework project structure.**





**10.What challenges have you faced while building or maintaining an automation framework, and how did you resolve them?**

Dynamic Elements



Problem: Changing IDs

Solution:



Used stable attributes

Coordinated with dev team



Flaky Tests



Problem: Random failures

Solution:



Replaced static waits with explicit waits

Improved locators



CI Failures



Problem: Works locally, fails in pipeline

Solution:



Aligned environment configs

Added proper logging

using traces of the playwright to debug in the headless mode 



