# Alaska Legislature (akleg.gov) Senate Scraper

This project is a Java-based web scraper built to fulfill an assignment. The goal is to scrape the Alaska Legislature Senate website (`https://akleg.gov/senate.php`) and extract profile information for all 20 senators.

The scraper visits the main senate page, collects the links for each senator, visits each individual profile page, and extracts the required data. The final output is saved as a clean JSON file.

## 🚀 Technology Stack

* **Java 11 (JDK 11)**
* **Maven:** For dependency management and project execution.
* **Selenium:** For browser automation and web page interaction.
* **WebDriverManager:** For automatically managing the `chromedriver` binary.
* **Jackson:** For serializing the collected Java objects into a JSON file.

## 📋 Assignment Requirements

The scraper was required to extract the following fields for each senator:
* Name
* Title
* Position (District)
* Party
* Address
* Phone
* Email
* URL (link to their profile page)

## Prerequisites

Before running, you must have the following installed:
1.  **Java 11** (or newer)
2.  **Apache Maven**
3.  **Google Chrome** (The scraper uses `chromedriver`)

## ▶️ How to Run

1.  Open a terminal or command prompt.
2.  Navigate to the root directory of this project (the folder containing `pom.xml`).
3.  Run the following Maven command:
    ```bash
    mvn exec:java
    ```
4.  The script will start, and you will see its progress in the console. It will print a line for each senator it scrapes (e.g., `Scraped: CATHY GIESSEL | Republican | N`).
5.  The process will take 1-2 minutes to complete.

## 📄 Output

When the script is finished, it will create a file named `senators_fixed.json` in the project's root directory. This file contains an array of JSON objects, one for each senator, with all the required fields.

---

## 📝 Project Notes & Methodology

### Scraping Strategy

The scraper works in several stages:
1.  **Link Collection:** It first loads the main senate page (`https://akleg.gov/senate.php`) and collects a unique set of all links that point to a member's detail page.
2.  **Iteration:** It then loops through this set, visiting each senator's profile link one by one.
3.  **Data Extraction:** On the detail page, it extracts data using the following logic:
    * **Name:** The `<h1>` tag text is extracted and cleaned (split by newline) to get just the senator's name.
    * **Email:** The `href` attribute of the `mailto:` link is parsed.
    * **Party & Position (District):** These fields proved difficult to extract with simple XPath. The final, robust solution is to get the **entire page text** and use **Regular Expressions (Regex)** to find the exact patterns `District: [X]` and `Party: [Y]`. This is much more reliable than brittle HTML selectors.
    * **Address & Phone:** The scraper finds the "Session Contact" block (or "Interim Contact" as a fallback). It parses the lines in this block, looking for the first `Phone:` line and building an address string, stopping before it reaches the "Biography" section.
4.  **JSON Serialization:** Each `Member` object is added to a list, which is then serialized into an indented JSON file using the Jackson library.

### Alternative Technologies Considered

* **API:** An official API is always the preferred, most stable solution. The `akleg.gov` site does not appear to offer a public API for this data. Third-party services like **LegiScan** provide this data via API, which would be a better choice for a production application.
* **Playwright:** Playwright is a modern alternative to Selenium and would also have been an excellent choice for this task, offering a slightly more modern API for browser control.

### ⏱️ Time Estimate to Complete

* **Analysis & Debugging:** 20 minutes
    * (Reviewing the assignment, inspecting the target website's HTML structure, and identifying the challenges.)
* **Research & Strategy:** 10 minutes
    * (Confirming no API exists, choosing the final Regex-based parsing strategy for reliability.)
* **Coding & Fixing:** 25 minutes
    * (Writing the initial Selenium script, debugging the failed XPath selectors, and re-implementing the logic using Regex.)
* **Documentation:** 10 minutes
    * (Writing this README file and summarizing the project notes.)
* **Total Estimated Time:** 65 Minutes
