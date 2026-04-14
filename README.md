# wedding_website

Simple one-page wedding RSVP site.

## What It Does

The published page lives in [index.html](/home/tunachip/Development/wedding_website/index.html) and collects:

- guest name
- phone number
- members in their party

The form is designed to submit directly to a Google Apps Script web app, which can write each response into a Google Sheet.

## Google Sheets Setup

1. Create a Google Sheet with columns:
   `Submitted At | Name | Phone | Party Size`
2. In that sheet, open `Extensions -> Apps Script`.
3. Paste this script:

```javascript
const SHEET_NAME = "RSVPs";

function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
  const data = JSON.parse(e.postData.contents);

  sheet.appendRow([
    data.submittedAt || new Date().toISOString(),
    data.name || "",
    data.phone || "",
    data.partySize || ""
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
8. Put that deployed web app URL in [index.html](/home/tunachip/Development/wedding_website/index.html).

## Local Preview

Open [index.html](/home/tunachip/Development/wedding_website/index.html) in a browser, or publish the repo with GitHub Pages from the root branch/folder.
