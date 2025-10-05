# BUCENG-CSC AutoTrack System Documentation 🚀🎓

**License:**  
This project is released under the MIT License. See the end of this document for details.

***

## Table of Contents 📑

- [Overview](#overview)
- [System Components](#system-components)
  - [Backend (Google Apps Script)](#backend-google-apps-script)
  - [Frontend (HTML, CSS, JavaScript)](#frontend-html-css-javascript)
- [Spreadsheet Structure & Column Mapping](#spreadsheet-structure--column-mapping)
- [Payment Status Logic](#payment-status-logic)
- [Error Handling](#error-handling)
- [Debug and Utility Functions](#debug-and-utility-functions)
- [Additional Features](#additional-features)
- [Usage Instructions](#usage-instructions)
- [How to Duplicate & Set Up for Another Organization](#how-to-duplicate--set-up-for-another-organization)
- [Deployment Guide](#deployment-guide)
- [Contact and Support](#contact-and-support)
- [Example Result Object (from backend)](#example-result-object-from-backend)
- [Notes and Limitations](#notes-and-limitations)
- [License](#license)
- [Visualizing the System Flow](#visualizing-the-system-flow)

***

## Overview ✨

The BUCENG-CSC AutoTrack system is a Google Apps Script web application for Bicol University College of Engineering students to check their College Student Council (CSC) fee payment status for AY 2025–2026. The system uses departmental sheets in Google Sheets and a user-friendly web frontend.

***

## System Components 🧩

### Backend (Google Apps Script) 📝

- **doGet(e):** Serves the HTML frontend.  
- **getPaymentStatus(studentId):** Validates input, searches department sheets, and returns payment status data.  
- **validateLogin(username, password):** Checks credentials against "CREDENTIALS" sheet (optional).  
- **checkUsernameExists(username):** Verifies if a username exists (optional).  
- **Debug helpers:** Tools to explore sheet structure and test data.

### Frontend (HTML, CSS, JavaScript) 🖥️

- Input field for Student ID.  
- Displays results with status banners, history, animations.  
- Communicates asynchronously with backend via `google.script.run`.  
- Includes dark mode toggle and informative banners.

***

## Spreadsheet Structure & Column Mapping 📊

| Letter | Index | Content                        |
| ------ | ----- | ----------------------------- |
| A      | 0     | NO (Row number)               |
| B      | 1     | STUDENT NO. (ID)              |
| D      | 3     | Last Name                     |
| E      | 4     | First Name                    |
| F      | 5     | Middle Name                   |
| G      | 6     | AY 2022-2023 Payment Status   |
| I      | 8     | AY 2023-2024 Payment Status   |
| K      | 10    | AY 2024-2025 Payment Status   |
| M      | 12    | AY 2025-2026 Payment Status   |

Headers are in rows 1–5; data starts at row 6. Ensure department sheets are named according to your organization's abbreviations (e.g., "EE" for Electrical Engineering).

***

## Payment Status Logic 🔍

1. 🧑‍🎓 Student enters their Student ID.  
2. 🔄 System normalizes input and searches across all department sheets from row 6 down for matches in column B.  
3. 📋 When a matching Student ID is found, the system extracts full name, department, and payment status for multiple academic years.  
4. 🌟 Frontend displays:  
   - ✅ "PAID" or ❌ "UNPAID" banner  
   - 📘 Payment history  
   - 👤 Student's personal info.

***

## Error Handling ⚠️

- ❌ Invalid or empty Student ID returns an error.  
- 🕵️ No match in any department sheet returns a "not found" message.  
- 🐞 Unexpected errors are logged and display a friendly message.  
- 🎭 Frontend shows animated feedback (shaking box and emoji rain for unpaid, confetti for paid).

***

## Debug and Utility Functions 🛠️

- `debugShowSheetStructure()` — Logs column headers for verification.  
- `debugListAllStudentIds()` — Lists a sample of student IDs.  
- `debugTestMultipleIds()` — Tests multiple IDs in batch.

***

## Additional Features 🎁

- Fee breakdown panel with clear amounts.  
- Reminder notes for receipt distribution and validation.  
- Links to CSC/USC official social media.  
- Interactive animations with confetti and emoji rain.  
- Dark mode toggle for accessibility.

***

## Usage Instructions 📖

1. Open the AutoTrack web app.  
2. Enter your Student ID (e.g., "2025-01-12345").  
3. Click **Check** or hit Enter.  
4. View your payment status and history.

***

## How to Duplicate & Set Up for Another Organization 🔄

- **Make a copy** of the Google Spreadsheet.  
- **Rename sheets and update data** to match your own departments and student payment statuses.  
- **Ensure headers** are on rows 1-5; data starts at row 6.  
- **Customize frontend text** for fees and contact info in the HTML.  
- **Update the Apps Script to use your Spreadsheet ID:**  
  ```js
  const ss = SpreadsheetApp.openById("YOUR_NEW_SPREADSHEET_ID");
  ```
- **Edit department sheet names** in the script if changed.  
- **Deploy as a new web app** with appropriate access.  
- Optionally, set up a **CREDENTIALS** sheet for login.  
- Share your new web app URL with your users.

***

## Deployment Guide 🚀

1. Prepare copied/configured Spreadsheet.  
2. Paste backend script, update Spreadsheet ID and sheets array.  
3. Deploy script as a new web app.  
4. Serve frontend via `doGet()` or host separately for branding.  
5. Share URL.

***

## Contact and Support 📞

- Bicol University students can contact via [CSC Facebook page](https://www.facebook.com/cengcsc).  
- Other organizations should update contact details accordingly.

***

## Example Result Object (from backend) 📦

```json
{
  "id": "2025-01-17282",
  "name": "Dela Cruz, Juan, S.",
  "department": "EE",
  "row": 12,
  "status": "PAID",
  "history": {
    "AY 2022–2023": "PAID",
    "AY 2023–2024": "PAID",
    "AY 2024–2025": "UNPAID",
    "AY 2025–2026": "PAID"
  }
}
```

***

## Notes and Limitations 📝

- Only Student IDs in the spreadsheet will return results.  
- Column and sheet structures must be maintained.  
- Update academic years as needed.  
- Login system requires separate setup.

***

## License 📜

MIT License

Copyright (c) 2025 BUCENG-CSC

Permission is hereby granted, free of charge, to any person obtaining a copy  
of this software and associated documentation files (the “Software”), to deal  
in the Software without restriction, including without limitation the rights  
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell  
copies of the Software, and to permit persons to whom the Software is  
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all  
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR  
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,  
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE  
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER  
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,  
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE  
SOFTWARE.

***
