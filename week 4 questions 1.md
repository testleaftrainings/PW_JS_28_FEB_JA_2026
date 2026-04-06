**1. Nested Frames with Dynamic IDs**



Q:

A legacy banking app uses nested iframes. The inner iframe’s ID is dynamic (e.g., `frame\_882`).  

How do you reliably click a button inside it without using the dynamic ID, and how do you avoid timeout if it loads slowly?



A: 

Use Playwright’s `frameLocator()` to reference iframes by stable selectors, not IDs. `frameLocator()` auto-waits for nested frames and elements before interaction.



const outerFrame = page.frameLocator('iframe\[data-test="transaction-container"]');

const innerFrame = outerFrame.frameLocator('iframe');

await innerFrame.getByRole('button', { name: 'Submit Transaction' }).click();



Playwright’s auto-waiting prevents timeouts if the frame or elements load slowly.





**2. Dialog Handling Timing**



Q:

Will this code correctly handle dialogs?

await page.getByRole('button').click();

page.on('dialog', async dialog => {

&#x20; console.log(dialog.message());

&#x20; await dialog.accept('My Input Text');

});



A:

No — the listener must be registered before the click:



page.on('dialog', async dialog => {

&#x20; console.log(dialog.message());

&#x20; await dialog.accept('My Input Text');

});

await page.getByRole('button', { name: 'Confirm' }).click();



**3. What is the difference between the any and unknown datatype in typescript ?**



any 

You can assign any value

You can perform any operation

TypeScript will NOT check anything



&#x20; ex:

&#x20; let value: any = "hello";



value.toUpperCase();   

value = 10;

value.toUpperCase();  



👉 Problem:



No type safety 

Errors will come only at runtime 



unknown

You can assign any value 

But you CANNOT use it directly



let value: unknown = "hello";



value.toUpperCase(); ❌ ERROR



You must check type first:



if (typeof value === "string") {

&#x20; console.log(value.toUpperCase()); }



👉 Benefit:



Type safety 

Forces you to validate before usage





**4. Can you access a nested frame directly in the playwright** 



page.frame():



Searches all frames globally

Returns first matching frame



Problem in nested frames:



If multiple frames have similar URLs

If iframe loads





5\.**First Non-Repeating Character**



function firstNonRepeatingChar(str: string): string | null {

&#x20; for (let i = 0; i < str.length; i++) {

&#x20;   let count = 0;



&#x20;   for (let j = 0; j < str.length; j++) {

&#x20;     if (str\[i] === str\[j]) {

&#x20;       count++;

&#x20;     }

&#x20;   }



&#x20;   if (count === 1) {

&#x20;     return str\[i];

&#x20;   }

&#x20; }



&#x20; return null;

}





(Or)

function firstNonRepeatingChar(str: string): string | null {

&#x20; for (let i = 0; i < str.length; i++) {

&#x20;   if (str.indexOf(str\[i]) === str.lastIndexOf(str\[i])) {

&#x20;     return str\[i];

&#x20;   }

&#x20; }



&#x20; return null;

}





**6.Merge Two Arrays Without Duplicates**



function mergeUnique(arr1: number\[], arr2: number\[]): number\[] {

&#x20; const result: number\[] = \[];



&#x20; // Add elements from arr1

&#x20; for (let i = 0; i < arr1.length; i++) {

&#x20;   if (!result.includes(arr1\[i])) {

&#x20;     result.push(arr1\[i]);

&#x20;   }

&#x20; }



&#x20; // Add elements from arr2

&#x20; for (let i = 0; i < arr2.length; i++) {

&#x20;   if (!result.includes(arr2\[i])) {

&#x20;     result.push(arr2\[i]);

&#x20;   }

&#x20; }



&#x20; return result;

}



**7.Count Occurrences**



function countOccurrences(arr: string\[]): \[string, number]\[] {

&#x20; const result: \[string, number]\[] = \[];



&#x20; for (let i = 0; i < arr.length; i++) {

&#x20;   const item = arr\[i];

&#x20;   let found = false;



&#x20;   for (let j = 0; j < result.length; j++) {

&#x20;     if (result\[j]\[0] === item) { // first element of tuple

&#x20;       result\[j]\[1] += 1;          // second element of tuple

&#x20;       found = true;

&#x20;       break;

&#x20;     }

&#x20;   }



&#x20;   if (!found) {

&#x20;     result.push(\[item, 1]); // push new tuple

&#x20;   }

&#x20; }



&#x20; return result;

}



**8.Write a function that accepts string | number and returns length if string, or double the number if number.”**



function process(input: string | number): number {

&#x20; if (typeof input === "string") return input.length;

&#x20; return input \* 2;

}



9\.**You get a string from an API ('SUCCESS' | 'FAIL') and want to match it with an enum safely**

enum ApiStatus { Success = "SUCCESS", Fail = "FAIL" }



function checkStatus(status: string) {

&#x20; if (status in ApiStatus) {

&#x20;   console.log(`Valid: ${status}`);

&#x20; } else console.log("Invalid");

}





**10.Reverse words in a sentence: "hello world" → "world hello".**

function reverseWords(sentence: string): string {

&#x20;   // Step 1: Split the sentence into words

&#x20;   const words = sentence.split(" ");

&#x20;   

&#x20;   // Step 2: Reverse the array of words

&#x20;   const reversed = words.reverse();

&#x20;   

&#x20;   // Step 3: Join back into a string

&#x20;   return reversed.join(" ");

}



// Example:

console.log(reverseWords("hello world"));



