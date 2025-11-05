# AKLeg Senate Scraper (Selenium Java)

This project scrapes Alaska State Legislature Senate member pages and writes a JSON file
(`senators.json`) with fields: `Name`, `Title`, `Position`, `Party`, `Address`, `Phone`, `Email`, `URL`.

## Prerequisites
- Java 11+
- Maven
- Google Chrome (recommended). WebDriverManager will auto-download ChromeDriver.
- Internet access.

## Build & Run (from project root)
```bash
mvn clean compile
mvn exec:java
```

The scraper writes `senators.json` to the project root.

## Notes
- To run headless, the scraper is already configured with headless option commented — you can enable it in the Java file.
- If running on CI or Linux headless environments, consider adding `--no-sandbox` and `--disable-dev-shm-usage` Chrome args.
