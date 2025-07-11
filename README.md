# 📘 BUCENG-CSC AutoTrack Documentation

## Overview

**BUCENG-CSC AutoTrack** is a web app that allows students to check their **CSC/USC fee payment status** using their **Student ID**. It integrates a Google Spreadsheet as the backend data source and displays personalized results using a stylized HTML frontend with confetti, emoji animations, and dynamic feedback.

---

## 🔧 Technologies Used

- **Google Apps Script (GAS)** – for backend logic and spreadsheet data access
- **Google Sheets** – as a lightweight database (`deploymentSource` sheet)
- **HTML/CSS/JavaScript** – frontend interface
- **Google Apps Script Web App** – hosting the frontend interface
- **Canvas Confetti & Emojis** – for animated feedback

---

## 📁 File Structure

### Apps Script Files

| File         | Purpose                                               |
|--------------|-------------------------------------------------------|
| `Code.gs`    | Backend logic for fetching student payment data       |
| `index.html` | Main frontend interface of the AutoTrack system       |

---

## 🔄 How It Works

### ✅ Student Workflow

1. Student visits the web app.
2. Inputs their Student ID.
3. Clicks the **Check** button or presses `Enter`.
4. The app:
   - Sends the ID to the backend.
   - Searches the **`deploymentSource` sheet**.
   - Returns the result (PAID or UNPAID).
   - Animates the result using confetti or emoji rain.

---

## 🔙 Backend: `Code.gs`

### `doGet(e)`
Serves the HTML page as the default view of the web app.

```javascript
function doGet(e) {
  return HtmlService.createHtmlOutputFromFile("index")
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);
}
