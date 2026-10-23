# Form Events

A lightweight, mobile-friendly event form portal with a dynamic administrator form builder. GitHub Pages hosts the front end, Google Apps Script validates and handles requests, and a dedicated new Google spreadsheet stores form definitions and submissions.

## Project links

- Repository: https://github.com/megi1450/form-events
- Dedicated new spreadsheet: https://docs.google.com/spreadsheets/d/1GDJU9_GMU9HpAH5ugxjkZKLxjjKufVH_g77b9e8ikGA/edit
- Intended GitHub Pages URL: https://megi1450.github.io/form-events/

## Included features

- Public Form Events page listing only published forms.
- Admin login with a private server-side token.
- Create, edit, preview, save draft, publish, archive and restore forms.
- Question types: short answer, paragraph, email, number, dropdown, multiple choice, checkboxes, date, and Yes/No.
- Reorderable questions, required/optional settings, placeholders, help text and configurable answer choices.
- Responsive dark interface and custom favicon.
- Google Sheets structure for Events, Questions and a Responses table. Every submission creates one row with timestamp, submission ID, event identity, and dedicated columns for answers.

## One-time Google Apps Script setup

Live submissions require a one-time deployment authorised by the spreadsheet owner. GitHub Pages is static hosting and cannot deploy Apps Script on your behalf.

1. Open https://script.google.com/ while signed in to the Google account that can edit the new spreadsheet.
2. Create a new project and call it Form Events Backend.
3. Replace its Code.gs content with the source in the repository file apps-script/Code.gs.
4. Save, choose initializeBackend in the function selector and click Run. Approve the Google authorisation prompts.
5. Open the execution log and copy the generated ADMIN TOKEN. Keep it private. It is stored in Script Properties, not in the site files.
6. The initializer creates the Events, Questions and Responses tabs in the new spreadsheet. It renames the only default Sheet1 tab to Events when possible.
7. Choose Deploy, then New deployment, then Web app. Set Execute as to your account and access to Anyone, then deploy and authorise. Copy the web app URL ending in /exec.
8. Edit config.js in this repository and set API_URL to that /exec URL. Commit the change. Never paste the admin token in config.js or any public repository file.
9. In repository Settings, open Pages; choose Deploy from a branch, branch main, folder root (/), then save. Wait for GitHub's published status.
10. Open the site, select Admin, paste the private token, create a form, and publish it. Submit a test response and verify the row in the dedicated spreadsheet.

If you need to retrieve the admin token again, run showAdminTokenForOwner from the Apps Script editor. Do not expose the token or make the spreadsheet public.

## Technical notes

- Public reads use JSONP. Admin actions and submissions are posted through a hidden iframe, allowing the static site to communicate with Apps Script without exposing Google credentials in the browser.
- The spreadsheet remains private. The script runs as the deploying owner; public respondents do not require spreadsheet access.
- Formula-leading answer text is escaped before writing to cells.
- This starter project is intended for ordinary event registration, not highly sensitive records. Before collecting sensitive data, add stronger identity/access controls, abuse prevention, and a privacy/retention policy.
- Opening index.html directly gives a static UI preview. Live events and submissions require the deployed URL in config.js.
