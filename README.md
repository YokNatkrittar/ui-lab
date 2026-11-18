# ui-lab

Automated UI testing lab for ITCS386 Software Verification and Validation.

Tool parts that I chose: **Part 1 Selenium IDE** and **Part 4 Cypress**.

## selenium-ide
Holds the Selenium IDE project `ui-lab.side` with two tests, `web form` and
`QP-01 customer` (steps 1 to 5 of QP-01), and `QP01customerTest.java`, the
Java export of QP-01. Needs ChromeDriver on the PATH (see Part 0).

```
cd selenium-ide
npx --yes selenium-side-runner@4.0.13 ui-lab.side
```

## cypress
Holds the Cypress project with two specs: `web-form.cy.js` (Selenium's web
form) and `pizza-scenario.cy.js` (all 7 steps of QP-01, with the custom
command `adminLogin`). The folders `cypress/videos` and `cypress/screenshots`
are the evidence of a full run.

```
cd cypress
npm install
npx cypress run --browser chrome
```