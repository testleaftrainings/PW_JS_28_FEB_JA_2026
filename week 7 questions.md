**1.What are the four main OOP principles? Explain how each one can be used in a test automation framework**

**The four core Object-Oriented Programming principles are:**



Encapsulation



Inheritance



Polymorphism



Abstraction



How they apply to automation frameworks



Encapsulation

Encapsulation means bundling data and methods inside a class and restricting direct access.



Example in automation:



Page Object classes store locators and actions together.



class LoginPage {

&#x20; private usernameField

&#x20; private passwordField



&#x20; async login(user, pass) { }

}



Benefit:



Prevents tests from directly accessing locators.



Inheritance



Inheritance allows one class to inherit properties and methods from another class.



Example:



BasePage

&#x20; |

LoginPage

DashboardPage



Common methods like:



waitForElement()



click()



navigate()



can exist in BasePage.



Benefit:



Code reuse.



Polymorphism



Polymorphism means the same method name behaving differently depending on context.



Example:



login(user, password)

login(token)



Same method name → different implementations.



Benefit:



Flexible framework design.



Abstraction



Abstraction hides implementation complexity and exposes only necessary functionality.



Example:



Instead of writing this in tests:



page.locator("#login").click()



Tests call:



loginPage.login()



Benefit:



Cleaner test scripts.



**2.What is Inheritance in OOP?**

**How would you use inheritance in a Playwright automation framework?**



Inheritance is a mechanism where a child class derives properties and methods from a parent class, enabling code reuse and hierarchical structure.Create a BasePage class containing common methods:



class BasePage {

&#x20; constructor(page){

&#x20;   this.page = page

&#x20; }



&#x20; async click(locator){

&#x20;   await this.page.locator(locator).click()

&#x20; }

}



Then extend it:



class LoginPage extends BasePage {

&#x20; async login(user, pass){

&#x20;   await this.click("#login")

&#x20; }

}



Benefits:



Reusable common functionality



Easier maintenance



Reduced code duplication





**3.What is a constructor in a class?**

**Why is it commonly used in Page Object Model classes?**





A constructor is a special method in a class that is automatically executed when an object of that class is created.



Example

class LoginPage {





&#x20;constructor(page:Page){

&#x20;  this.page = page

&#x20;}



}

Why constructors are used in POM



They are used to:



Initialize the Playwright page object



Initialize locators



Setup dependencies required for the page



Example:



constructor(page){

&#x20;this.page = page

&#x20;this.username = page.locator('#username')

}



Benefit:



Each Page Object receives the same browser page instance.









**4.In an automation framework, why would you make locators or methods private inside a Page Object class?**



Locators are usually made private to enforce encapsulation.



Reasons:



Prevent direct access from test files



Ensure locators are used only through page methods



Reduce accidental misuse



Improve maintainability



Example:



class LoginPage {



&#x20;private usernameField

&#x20;private passwordField



&#x20;async login(user, pass){

&#x20;  await this.usernameField.fill(user)

&#x20;}

}



Tests should call:



loginPage.login()



Not:



page.locator('#username')





**5.There are 2 functions in playwright to enter a value in textbox, one is type and another one is fill. When to use what?**



fill() method

What it does



fill() directly sets the value of the input field. It clears the existing value and inserts the new value instantly.



Example

await page.locator('#username').fill('udai123');

Characteristics



Clears existing text automatically



Inserts the value instantly



What it does



type() simulates real user typing by sending characters one by one, triggering keyboard events like keydown, keypress, and keyup.



Example

await page.locator('#search').type('Playwright');



You can even simulate typing delay:



await page.locator('#search').type('Playwright', { delay: 100 });

Characteristics



Types character by character



Triggers keyboard events



Slower than fill()



**6.How do you conditionally skip a test only on the 'Production' environment using annotations?"**



The Answer: Use test.skip(). You combine it with your .env logic to ensure certain tests (like "Delete User") never run on Live.

test('Delete User Account', async ({ page }) => {

  // If the environment is 'prod', this test will be skipped immediately

  test.skip(process.env.ENV === 'prod', 'Data deletion is not allowed in Production');

 

  await page.goto('/settings');

  await page.click('#delete-account');

});



**7.You want smoke tests to run first with minimal retries, and full regression to run later with retries and videos. How would you split this using Playwright config?**





projects: \[

  {

    name: 'Smoke',

    testMatch: /.\*smoke.spec.ts/,

    retries: 0,

  },

  {

    name: 'Regression',

    testIgnore: /.\*smoke.spec.ts/,

    retries: 2,

    use: { video: 'on', trace: 'retain-on-failure' },

  },

]

Logical Separation via testMatch and testIgnore

testMatch in Smoke: This ensures the Smoke project only looks for files ending in .smoke.spec.ts. It creates a focused, high-speed execution gate.

testIgnore in Regression: In a full regression, you often want to run everything. However, if your Smoke tests are already running in a different CI job, you might use testIgnore to prevent redundancy,



npx playwright test --project=Smoke: Runs only the fast, zero-retry tests.

npx playwright test --project=Regression: Runs the heavy, resilient suite.



**8.You have a very heavy test that takes 2 minutes, but your global timeout is 30 seconds. How do you handle this without changing the global config?"**

The Answer: Use test.slow().

This annotation triples the default timeout for that specific test. If your global timeout is 30s, test.slow() makes it 90s.



**9.You have a function called getDiscount. Sometimes it takes a percentage (number), and sometimes it takes a coupon code (string).**

TypeScript doesn't allow two functions with the same name in the same class like Java. How do you implement method overloading here to handle both number and string inputs safely?





class DiscountService {

&#x20;

&#x20; apply(value: number): void;

&#x20; apply(value: string): void;



&#x20; 

&#x20; apply(value: any): void {

&#x20;   if (typeof value === 'number') {

&#x20;     console.log(`Applying ${value}% off`);

&#x20;   } else {

&#x20;     console.log(`Applying coupon: ${value}`);

&#x20;   }

&#x20; }





**10.You have a base class Notification with a method send(). You create a subclass EmailNotification.**

How do you ensure that EmailNotification uses its own logic for send(), but also still executes the basic logging logic defined in the parent Notification class?



class Notification {

&#x20; send() { console.log("Logging notification to DB"); }

}



class EmailNotification extends Notification {

&#x20;  send() {

&#x20;   super.send(); 



&#x20;   console.log("Sending actual Email via SMTP");

&#x20; }

}





