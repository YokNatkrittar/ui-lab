# UI lab results

Tool parts that I chose: Part 1 Selenium IDE and Part 4 Cypress

## Part 0
- Step 0.1, Chrome version: 157.0.8103.79 (Official Build) (arm64) 
- Step 0.3, ChromeDriver version: ChromeDriver 157.0.8103.79 (6dc9bc3bdaea5c35ed6d41fceab2f7a3a2227c4f-refs/branch-heads/8103@{#1574})
- Step 0.4, locator of Pizza, Please!: button[name='pizza-please']
- Step 0.4, locator of Excluded tools: select
- Step 0.4, locator of Must be vegetarian: #default-checkbox
- Step 0.4, locator of Custom Pizza Name: input#pizza-name
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261118-213000, Pizza cutter, <calories> calories per slice

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | The heading read "Looking to break out of your pizza routine?" | Pass |
| 2    | Max Calories per Slice, Excluded tools, Must be vegetarian and Custom Pizza Name appeared | Pass |
| 3    | Fields showed 600, Knife, vegetarian ticked, Lab Pizza 20261118-213000 | Pass |
| 4    | Answer showed Lab Pizza 20261118-213000, tool Pizza cutter, <calories> calories per slice | Pass |
| 5    | The page said "Please log in first." | Pass |
| 6    | After sign-in, the page showed "Latest pizza recommendations" | Pass |
| 7    | The list showed "Lab Pizza 20261118-213000 (tool=Pizza cutter, ingredients_number=8)", the same tool as in step 4 | Pass |

## Part 1: Selenium IDE
- Firefox version and Node.js version: Firefox 160.0.1 (aarch64), Node.js v24.11.0 (npm 11.6.1)
- Step 1.6, commands changed, added or deleted after recording: Kept set window size and changed it to 1280x900. Replaced the recorded css=.ml-4 targets with xpath=//label[normalize-space()='Max Calories per Slice']/../input (type 600) and xpath=//select/option[normalize-space()='Knife'] (wait for element present). Fixed the waits that used id=floating_filled (shared by 3 boxes) to wait for the calories box, css=select and id=default-checkbox. Set the name box to ${pizzaName} and the assert selected label target to css=select. Added the waits, the execute script and echo for the pizza name, all the asserts (title, values, Knife, checked, Name, Tool, calories, Please log in first.), the store text and the calories script. Deleted the extra recorded clicks.
- Step 1.6, the pizza names of run 1 and run 2, from the lines info: echo: Lab Pizza 20261118-170424 / Lab Pizza 20261118-<run 2>
- Step 1.6, the line Tests: of both runs: Tests: 2 passed, 2 total / Tests: 2 passed, 2 total
- Step 1.7, one line of QP01customerTest.java that needs rewriting, and why: driver.findElement(By.xpath("//label[normalize-space()='Max Calories per Slice']/../input")).sendKeys("600"); does not clear the box first. The box already holds 1000, so it would end up as 1000600. It needs a .clear() before sendKeys.
## Part 4: Cypress
- node --version, npm --version and npx cypress version: v24.11.0, 11.6.1, Cypress 16.2.
- Step 4.3, failure message for the wrong title: Timed out retrying after 10000ms: expected 'Web form' to equal 'Web Form' (-'Web form' +'Web Form')
- Step 4.6, tool and calories in the answer of POST /api/pizza: tool <...>, calories <...>
- Step 4.7, the line of the admin list that shows your pizza: Lab Pizza 20261118-... (tool=..., ingredients_number=...) (newest)
- Step 4.8, run 1, result, duration and pizza name: All specs passed, 00:10, Lab Pizza 20261118-...
- Step 4.8, run 2, result, duration and pizza name: All specs passed, 00:10, Lab Pizza 20261118-...

## Part B
1. In Step 0.4, `$$("#floating_filled").length` returned 3, because the three number boxes of QuickPizza share the same id, so the id cannot tell them apart. The tests therefore find Max Calories per Slice through the text of its label. When I recorded QP-01 in Selenium IDE, the recorder chose `id=floating_filled` for the waits of all three boxes and `css=.ml-4` for typing 600, and the run timed out with "invalid element state" until I replaced it with the label XPath. I propose giving each box a unique id, for example `id="max-calories-per-slice"`. A tester could then use a short `#max-calories-per-slice` locator that does not break when the label text or the layout changes.

2. Step 7 is a consistency oracle (a derived oracle). Instead of a fixed expected value, it compares two outputs of the system with each other: the tool in the recommendation and the tool in the admin list. The tool is random, so a fixed expected value cannot be written. This check would catch a fault where the server saves a different pizza than it sends to the customer. For example, it could generate a second pizza when saving, or store the tool of an earlier request, so the admin sees a tool that the customer never got.

3. A check could read the answer of POST /api/pizza with `cy.intercept` and assert that `response.body.vegetarian` is true, because the response I saw in the Network tab includes a `vegetarian` field. A stronger check would read each ingredient of the pizza from the page or the response and compare it with a list of ingredients that are not vegetarian, such as meat and fish. The test needs the ingredient list from the page or the API, plus an independent source of which ingredients count as vegetarian, so that it does not only trust the server's own flag.

4. In Selenium IDE I had to add every wait myself, with `wait for element visible` and `wait for element present` and a 10000 ms timeout, and a missing or wrong wait caused timeouts, such as "Operation timed out during click:css=.btn". In Cypress I wrote no waits, because every `cy.get` and `should` retried for up to 10 seconds by itself. Recording with Selenium IDE was quick, but cleaning up the recording took longer than writing the Cypress code: I had to fix recorded targets like `css=.ml-4` and `id=floating_filled`, a missing value, and the window size. The selenium-side-runner log was short and showed the failed command and the pizza name from echo. Cypress gave a clearer report: a results table with durations of about <xx> seconds, plus videos and screenshots of each step.

5. I would choose Cypress for Project 2. First, it waits for elements automatically, so the QP-01 spec passed on both full runs without any fixed pauses, while Selenium IDE runs failed several times on timing and clicks. Second, it records videos and screenshots by itself, which is the evidence Project 2 asks for, and `cy.intercept` let me check the server's answer as well as the page.

6. (Part 1) The recorder chose `css=.btn` for Submit. `btn` is a styling class that many buttons can share, so the target finds the first element with that class, and a new button above Submit would make the test click the wrong one. In my runs, the test also timed out during `click:css=.btn`. I replaced it with `xpath=//button[@type='submit']`, which is more stable because `type='submit'` describes what the button does, not how it looks, so it keeps working when the layout or colours change.

7. (Part 4) QuickPizza runs on several servers, and a cookie keeps one browser on one server, so the admin list shows only the pizzas of that server. Cypress clears the cookies before each test, so if steps 6 and 7 were a second test, the browser could land on a different server whose admin list does not contain my pizza, and the test would fail even though QuickPizza works. A second test would also lose `pizzaName` and the alias `@tool` from steps 3 and 4, because each test starts fresh. Keeping QP-01 as one test keeps the same session, the same server and the same data from step 1 to step 7.