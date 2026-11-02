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

## 4. View Basket (⭐⭐ 2-Star)
**Objective:** View another user’s shopping basket.

### The Vulnerability: Insecure Direct Object Reference (IDOR)
The application assigns a Basket ID (`bid`) to a user's session storage but fails to verify on the backend if the requested basket actually belongs to the authenticated user. This allows for Horizontal Privilege Escalation.

### Execution Steps:
1. Log in to the application and add an item to the cart to initialize a basket.
2. Open Developer Tools (F12) and navigate to the **Storage** tab.
3. Under **Session Storage**, locate the `bid` (Basket ID) key.
4. Change the numeric value of `bid` to another number (e.g., `1` or `2`).
5. Refresh the page and click the **Your Basket** icon to load the hijacked cart.

## 5. Web3 Sandbox (⭐ 1-Star)
**Objective:** Find an accidentally deployed code sandbox for writing smart contracts on the fly.

### The Vulnerability: Security Through Obscurity
Developers often leave internal testing environments (sandboxes) on production servers, assuming that if there is no menu button, nobody will find them. However, the path is still exposed in the frontend JavaScript routing map.

### Execution Steps:
1. Open Developer Tools (F12) and navigate to the **Debugger** or **Sources** tab.
2. Search within the `main.js` file for the keyword `sandbox`.
3. Locate the exposed routing path defined as `{path: "web3-sandbox"}`.
4. Append `/#/web3-sandbox` to the base application URL and press Enter.
