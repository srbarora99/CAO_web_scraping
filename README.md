# CAO_web_scraping
This project automates the scraping of Google search results for specific keywords, filters for recent, relevant blog posts, and extracts key information such as content, publication dates, and sentiment scores. The resulting data is compiled into a CSV file for further analysis.

**There are a total of 7 cells - **
Here's a summary of each cell in the pipeline:

Cell 1:
Installs the necessary libraries (like python-docx, selenium, spacy, textblob, pandas) and sets up ChromeDriver. Also downloads spaCy's English model.

Cell 2:
Imports all required modules (BeautifulSoup, Selenium, spaCy, TextBlob, dateutil, etc.) and sets configuration parameters. This includes defining an updated exclusion list of domains to avoid (unless they contain the required subsection) and specifying the required subsection (e.g., "/blog/").

Cell 3:
Loads keywords from a DOCX file, where each non-empty paragraph is treated as a separate keyword. These keywords will be used to query Google later in the pipeline.

Cell 4:
Defines helper functions for generating random HTTP headers (to mimic different browsers) and introducing random delays between requests. This helps reduce the risk of being blocked by Google.

Cell 5:
Uses Selenium to scrape Google search results. It retrieves results from the first two pages by using pagination (start=0 and start=10) and attempts to interact with the "Tools" menu to apply the "Past year" filter. The code uses multiple XPath candidates and JavaScript fallback to reliably click on the filter. After scraping, it filters the URLs using the exclusion list and only includes those that either are not from aggregator sites or contain the "/blog/" subsection.

Cell 6:
Contains helper functions for content extraction and date extraction. One function splits page text into sentences using spaCy and uses TextBlob to filter for sentences with significant sentiment (above a threshold), returning either the matched sentences or a fallback excerpt. Another function extracts the publication date from meta tags, <time> elements, or using regex, then tries to parse and reformat it to a uniform format (e.g., "Mar 15 2023"). If parsing fails, it returns the raw date information.

Cell 7:
Iterates over each keyword, uses the Selenium scraper (Cell 5) to obtain URLs, and for each URL it:

Extracts relevant content (with the sentiment filtering in Cell 6),

Extracts the publication date,

Computes the sentiment score and classifies it as positive, negative, or neutral,

Applies quality checks (e.g., skips rows with empty content or stale publication dates, if available),

And finally, it compiles all this information into a DataFrame that is exported as a CSV file ("CAO_date.csv").
