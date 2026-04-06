**1.Window Handling (Popups \& Tabs)**

**The Question:**

**"You click a button that opens a third-party payment gateway in a new tab. How do you capture that tab, and why is using Promise.all essential here instead of just awaiting the click first?"**

When a new tab opens, Playwright emits a 'page' event from the browser context.



Problem:

If we do:

await page.click('#pay');

const newPage = await context.waitForEvent('page');



The click completes, and the new tab may already open before waitForEvent starts. So the event is missed and the test hangs.



Solution:

const \[newPage] = await Promise.all(\[

&#x20; context.waitForEvent('page'),

&#x20; page.click('#pay')

]);



Why Promise.all:

It ensures both listening and action happen simultaneously, avoiding race conditions.





**2."When would you choose JSON over CSV for test data? How do you iterate through a JSON array to generate dynamic test cases in Playwright?"**

**Use for Data-Driven Testing (DDT) where you have hundreds of permutations of the same flat fields (e.g., a search field test with 500 different strings). It’s memory-efficient for the runner.**

JSON:

\- Supports nested data

\- Flexible structure

\- Best for complex scenarios



CSV:

\- Flat structure

\- Best for large datasets

\- Memory efficient



Example JSON:

\[

&#x20; {

&#x20;   "username": "admin\_user",

&#x20;   "role": "admin",

&#x20;   "details": {

&#x20;     "permissions": \["create\_user", "delete\_user"],

&#x20;     "accessLevel": 10

&#x20;   }

&#x20; }

]



Iteration:

import users from './data.json';



users.forEach(user => {

&#x20; test(`Login ${user.username}`, async ({ page }) => {

&#x20;   await page.fill('#user', user.username);

&#x20; });

});







**3.Explain how you structure .env.qa, .env.staging, .env.prod, and how your Playwright config dynamically selects the correct values at runtime.**



Answer:

.env.qa

.env.staging

.env.prod



Code:

import dotenv from 'dotenv';



const env = process.env.ENV || 'qa';

dotenv.config({ path: `.env.${env}` });



export default defineConfig({

&#x20; use: {

&#x20;   baseURL: process.env.BASE\_URL

&#x20; }

});





**4.“In the CSV file, some rows have extra optional fields (e.g. discount codes) that only apply if non-empty. How would you handle these optional fields in a robust Playwright parameterized test?**



When parsing the CSV, use the columns: true and relax\_column\_count: true options. This prevents the parser from throwing an error if Row 5 has 3 columns but Row 6 only has 2.



const records = parse(fs.readFileSync('data.csv'), {

&#x20; columns: true,

&#x20; skip\_empty\_lines: true,

&#x20; relax\_column\_count: true

});



**5. Verify File Download and Check Size**



**Q:**

**A report is downloaded. How do you verify the download succeeded and confirm the file size?**



A:

const \[download] = await Promise.all(\[

&#x20; page.waitForEvent('download'),

&#x20; page.click('#download-report-btn'),

]);



const filePath = './downloads/' + download.suggestedFilename();

await download.saveAs(filePath);



expect(fs.existsSync(filePath)).toBe(true);



const stats = fs.statSync(filePath);

expect(stats.size).toBeGreaterThan(0);



This confirms the file exists and isn’t empty.



**6. Assert File Type Before Upload**



**Q:**

**How do you validate the file type before uploading?**



A:



import fs from 'fs';



const ext = path.extname(filePath);

expect(ext).toBe('.pdf');



**7. Assert File Size Before Upload**



**Q:**

**Before uploading, how do you check file size?**



A:



import fs from 'fs';





const stats = fs.statSync(filePath);

expect(stats.size).toBeGreaterThan(0);





**8. Can you upload multiple files?**



Yes, if <input type="file" multiple /> is present.



const fileChooser = await page.waitForEvent('filechooser');

await page.click('#uploadButton');



await fileChooser.setFiles(\[

&#x20; './Data/file1.json',

&#x20; './Data/file2.json'

]);



Condition:

<input type="file" multiple>





**9.You have test data, config, and secrets. How will you choose between JSON, CSV, and ENV?**



JSON → structured test data (nested objects, easy parsing)

CSV → bulk/simple tabular data (large datasets)

ENV → sensitive data (passwords, tokens, URLs)



**10. Some rows in CSV are missing username. What will you do?**



Add validation layer before execution

Skip invalid rows or fail fast

Log errors clearly

Example :

if (!data.username) {

&#x20; console.warn("Skipping invalid row");

&#x20; return;

}



