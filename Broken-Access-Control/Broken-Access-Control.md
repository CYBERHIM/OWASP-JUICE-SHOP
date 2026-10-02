# Broken Access Control (BAC) Walkthroughs

**Team Lead:** Ibrahim Aliaminu Olamide  
**Target Application:** OWASP Juice Shop 

## Overview
Broken Access Control occurs when an application fails to properly enforce restrictions on what authenticated or unauthenticated users are allowed to do. In these challenges, we bypass the intended user interface to access restricted administrative functions and execute unauthorized backend commands.

---

## 1. Score Board (⭐ 1-Star)
**Objective:** Find the carefully hidden 'Score Board' page.

### The Vulnerability: Security Through Obscurity
The developers hid the progress tracker by simply omitting the button from the main navigation menu. However, because Juice Shop is a Single Page Application (SPA), the complete routing map is downloaded to the browser in the frontend JavaScript files.

### Execution Steps:
1. Open the target application in the browser (`http://127.0.0.1:42000`).
2. Press `F12` to open Developer Tools.
3. Navigate to the **Debugger** tab and open the **Global Search** tool (`Ctrl + Shift + F`).
4. Search for the keyword `score`.
5. In the `main.js` file, locate the routing path defined as `{path: "score-board"}`.
6. Append `/#/score-board` to the base URL (e.g., `http://127.0.0.1:42000/#/score-board`) and press Enter.

**Result:** The application routes to the hidden scoreboard, bypassing the absent UI button.

---

## 2. Administration Page (⭐ 1-Star)
**Objective:** Access the administration section of the store.

### The Vulnerability: Missing Page-Level Access Control
Similar to the Scoreboard, the Administration dashboard lacks a UI button. More critically, the server fails to verify if the visitor holds an "Admin" role before loading the restricted page components.

### Execution Steps:
1. Using the Developer Tools **Global Search**, query the keyword `administration`.
2. Identify the routing path in `main.js` defined as `{path: "administration"}`.
3. Append `/#/administration` to the base URL and navigate to it.

**Result:** The hidden Administration dashboard loads, revealing registered user emails and customer feedback.

---

## 3. Five-Star Feedback (⭐⭐ 2-Star)
**Objective:** Get rid of all 5-star customer feedback.

### The Vulnerability: Missing Function-Level Access Control
While accessing the admin page is a flaw, allowing an unauthenticated user to execute administrative commands is a severe backend vulnerability. The API endpoint responsible for deleting reviews blindly trusts the incoming `DELETE` request without validating the user's session token or authorization level.

### Execution Steps:
1. Navigate to the compromised Administration page (`/#/administration`).
2. Locate the "Customer Feedback" section on the right side of the dashboard.
3. Identify a feedback entry with a 5-star rating.
4. Click the **trash can icon** next to the review.

**Result:** The backend API accepts the unauthorized delete command and permanently removes the review from the database.
