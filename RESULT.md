# UI lab results

Tool parts that I chose: Part 1 Selenium IDE and Part 4 Cypress

## Part 0
- Step 0.1, Chrome version: 157.0.8103.79 (Official Build) (arm64) 
- Step 0.3, ChromeDriver version: ChromeDriver 157.0.8103.79 (6dc9bc3bdaea5c35ed6d41fceab2f7a3a2227c4f-refs/branch-heads/8103@{#1574})
- Step 0.4, locator of Pizza, Please!: button[name='pizza-please']
- Step 0.4, locator of Excluded tools: select
- Step 0.4, locator of Must be vegetarian: #default-checkbox
- Step 0.4, locator of Custom Pizza Name: input#pizza-name
- Step 0.5, pizza name, tool and calories of the run by hand: <Lab Pizza 2026...>, <tool>, <calories> per slice

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | The heading reads "Looking to break out of your pizza routine?" | Pass |
| 2    | Max Calories per Slice, Excluded tools, Must be vegetarian and Custom Pizza Name appeared | Pass |
| 3    | Fields showed 600, Knife, vegetarian ticked, <pizza name> | Pass |
| 4    | Answer showed <pizza name>, tool <tool>, <calories> calories per slice | Pass |
| 5    | The page said "Please log in first." | Pass |
| 6    | After sign-in, the page showed "Latest pizza recommendations" | Pass |
| 7    | The list showed <pizza name> with tool <tool> | Pass |