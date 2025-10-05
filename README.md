# BUCENG-CSC AutoTrack System Documentation

**License:**  
This project is released under the MIT License. See the end of this document for details.

---

## Table of Contents

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

---

## Overview

The BUCENG-CSC AutoTrack system is a Google Apps Script web application for Bicol University College of Engineering students to check the College Student Council (CSC) fee payment status for AY 2025–2026. The system works with departmental sheets in a Google Spreadsheet and a user-friendly web frontend.

---

## System Components

### Backend (Google Apps Script)

- **doGet(e):** Serves the HTML frontend.  
- **getPaymentStatus(studentId):** Validates, searches across department sheets, and returns fee status details.  
- **validateLogin(username, password):** Authenticates users by checking a "CREDENTIALS" sheet.  
- **checkUsernameExists(username):** Looks for a username in the "CREDENTIALS" sheet.  
- **Debug helpers:** Sheet and record review tools.

### Frontend (HTML, CSS, JavaScript)

- Inputs Student ID, displays result via banners, payment history, and feedback animations.  
- Communicates with Google Apps Script backend via `google.script.run`.  
- Responsive UI with dark mode and info panel.

---

## Spreadsheet Structure & Column Mapping

Your Google Sheets file must contain the following column mappings (row numbers start at 1):

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

**Headers occupy rows 1–5.** Data begins at row 6.

Each department must have a separate sheet named after its abbreviation (e.g., "EE" for Electrical Engineering).

---

## Payment Status Logic

1. User enters Student ID and submits query.  
2. Script normalizes input and searches each department sheet from row 6 onward, looking for an ID match in column B.  
3. On matching row, extracts student's full name, department, payment years/status, and compiles the result.  
4. Frontend displays:  
   - Banner ("PAID" or "UNPAID")  
   - Payment history (all years)  
   - Student’s info  

---

## Error Handling

- **Invalid Student ID:** Returns error for empty/invalid input.  
- **Record not found:** Returns error if Student ID not in any department sheet.  
- **Script/Spreadsheet errors:** Displays generic error and logs stack for debugging.  
- **Frontend:** Shows clear, animated error feedback for failed lookups.

---

## Debug and Utility Functions

- `debugShowSheetStructure()` — Logs columns and headers for sheet verification.  
- `debugListAllStudentIds()` — Lists sample IDs for testing.  
- `debugTestMultipleIds()` — Bulk lookup testing.

---

## Additional Features

- Clear fee breakdown panel.  
- Receipt/validation reminders.  
- Links to CSC/USC pages.  
- Confetti and emoji rain for status feedback.  
- Dark mode switch.  
- Login utilities (if needed).

---

## Usage Instructions

1. Open the app’s site.  
2. Enter a valid Student ID (e.g., "2025-01-12345").  
3. Click "Check" or press Enter.  
4. See payment status and history.

---

## How to Duplicate & Set Up for Another Organization

**To use this system for a different college/organization:**

1. **Make a Copy of the Google Spreadsheet**  
   - Go to the original payments spreadsheet.  
   - In Google Sheets: File → Make a copy.  
   - Rename it as needed for your organization.

2. **Edit Department Sheets**  
   - Change sheet names to match your organization’s department codes.  
   - Fill in student lists and payment statuses using the provided column structure.

3. **Update Headers**  
   - Make sure rows 1–5 are headers for all sheets.  
   - Data should start at row 6.

4. **Adjust Fees and Info Board**  
   - In the HTML, modify the “About” or info-board section to show your own fee breakdown and organizational contact details.

5. **Configure Google Apps Script**  
   - Open Extensions → Apps Script in your new copy of the spreadsheet.  
   - Paste in the backend code.  
   - Change the `SpreadsheetApp.openById("...")` call to use your copied Spreadsheet’s ID.  
     Example:  
     ```
     const ss = SpreadsheetApp.openById("YOUR_NEW_SPREADSHEET_ID_HERE");
     ```  
   - Edit department sheet names in the `departmentSheets` array if you changed them.

6. **Deploy the Web App**  
   - Deploy the script as a new web app (see Deployment Guide below).  
   - Anyone with the link can use your unique system.

7. **(Optional) Set Up CREDENTIALS Sheet**  
   - If needed for login/registration, add a “CREDENTIALS” sheet for usernames and passwords.  
   - Match script expectations for column layout.

8. **Share the Web App Link**  
   - Distribute your unique deployment link to your users.

---

## Deployment Guide

1. **Spreadsheet Setup**  
   - Create/copy and configure the Google Sheet as above.

2. **Apps Script**  
   - Open script editor, set up backend code, update Spreadsheet ID and department sheet names.

3. **Web App Deployment**  
   - In Apps Script: Deploy → New deployment → Web app.  
   - Set "Who has access" to "Anyone" (or "Anyone with link").  
   - Deploy.

4. **Frontend**  
   - Served by Apps Script `doGet`, or host standalone HTML for custom branding.  
   - Upload this documentation as `README.md` for future maintainers.

---

## Contact and Support

For Bicol University students:  
Reach out at [CSC Facebook page](https://www.facebook.com/cengcsc).

For other organizations:  
Replace contact details in the frontend HTML/info board section as needed.

---

## Example Result Object (from backend)
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


---

## Notes and Limitations

- The system only checks IDs present in your spreadsheet.  
- All department sheets and columns must match the structure above.  
- Update year columns as new academic years are added.  
- Login functions require an extra CREDENTIALS sheet.

---

## License

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

