# wedding_website

Simple one-page wedding RSVP site.

## What It Does

The page lives in [src/page.html](/home/tunachip/Development/wedding_website/src/page.html) and collects:

- guest name
- phone number
- whether they are bringing a plus one

The form is designed to submit directly to a Google Apps Script web app, which can write each response into a Google Sheet.

## Google Sheets Setup

1. Create a Google Sheet with columns:
   `Name | Phone | Plus One | Submitted At`
2. In that sheet, open `Extensions -> Apps Script`.
3. Paste this script:

```javascript
const SHEET_NAME = "Sheet1";

function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
  const data = JSON.parse(e.postData.contents);

  sheet.appendRow([
    data.name || "",
    data.phone || "",
    data.plusOne || "",
    data.submittedAt || new Date().toISOString()
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ ok: true }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

4. Click `Deploy -> New deployment`.
5. Choose `Web app`.
6. Set access so anyone with the link can use it.
7. Copy the web app URL.
8. In [src/page.html](/home/tunachip/Development/wedding_website/src/page.html), replace:
   `PASTE_YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL_HERE`
   with your deployed web app URL.

## Local Preview

Open `src/page.html` in a browser, or serve the folder with any static file server.
