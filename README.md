# OWASP Juice Shop: Team Walkthrough & Exploitation Guide

Welcome to our group's central repository for the OWASP Juice Shop penetration testing lab. This project documents our team's systematic approach to identifying, exploiting, and understanding web application vulnerabilities ranging from 1-star (Easy) to 6-star (Hard) difficulty levels. 

Our workflow is categorized by vulnerability domains (such as Broken Access Control, Injection, and XSS). Each section contains step-by-step execution guides, the underlying mechanics of the vulnerabilities, and screenshot evidence of our completed exploits.

## Team Members
* **Team Lead:** Ibrahim Aliaminu Olamide
* **Collaborator 2:** Saminu Umar
* **Collaborator 3:** Samuel Nwankwo

## Repository Structure
To keep our work organized and prevent merge conflicts, please adhere to the following structure when adding your solutions:

* `README.md` - This master document detailing our project scope and setup.
* `Broken-Access-Control.md` - Walkthroughs for all BAC-related challenges.
* `[Category-Name].md` - (e.g., `Injection.md`, `XSS.md`) Create new markdown files for new vulnerability categories as we progress.
* `Screenshots/` - A centralized folder for all image evidence. 

## Collaborator Workflow
When you solve a challenge, follow these steps to document it:
1. Take a clear screenshot of the green success banner and the exploit in action.
2. Name the screenshot clearly (e.g., `BAC-1Star-Scoreboard.png`) and upload it to the `Screenshots/` folder.
3. Open the relevant category file (e.g., `Broken-Access-Control.md`).
4. Add the new challenge to the bottom of the document, detailing the **Objective**, **Vulnerability**, and **Execution Steps**.
5. Commit your changes with a descriptive message (e.g., "Added 2-Star View Basket IDOR solution").

## Local Lab Setup
For any team member needing to spin up the local environment on Kali Linux using the `apt` installation:

1. Open your terminal.
2. Run the start command:
   ```bash
   sudo juice-shop
