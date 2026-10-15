# Violin Studio Tracker

Studio manager for a Suzuki violin studio: students and contacts, weekly schedule,
attendance and cancellations, payments and balances, make-up lessons, and lesson notes.

This page holds no student data. All data lives in a private Google Sheet and is reached
through a Google Apps Script web app that requires a Studio Key. The Apps Script code is
kept out of this public repository on purpose.

Open the site, go to Settings, and paste the Apps Script web app URL and Studio Key.

## How updates get in

Bruce tells Claude what happened. Claude writes a small update file into a private folder in
Bruce's Google Drive, and his Apps Script adds it to the Google Sheet automatically every few
minutes. No student information is ever stored in this repository.

The "Paste from Claude" box on the Today tab still works as a manual backup.
