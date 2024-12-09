

# Web Scraping 

This project is designed to scrape websites, extract relevant company information (like description, products/services, use cases, customers, and partners), and store the extracted data in a PostgreSQL database. It leverages **Ollama LLM** for natural language processing, and **BeautifulSoup** for scraping website content.

## Features
- Web scraping with BeautifulSoup
- Data extraction using a language model (Llama2 via Ollama)
- Fallback mechanism using regular expressions for data extraction
- Database integration with PostgreSQL to store URLs and extracted data
- Exporting extracted data to Excel
- Database updates with extracted information

## Technologies Used
- **Python**: The primary programming language for the project.
- **LangChain and Ollama**: Used to interface with the Llama2 language model for text-based information extraction.
- **BeautifulSoup**: For scraping website content.
- **psycopg2**: For PostgreSQL database interaction.
- **pandas**: For handling and exporting data to Excel.

## Installation

1. **Clone the repository**:
    ```bash
    git clone <repository-url>
    cd <repository-folder>
    ```

2. **Install required dependencies**:
    Install the dependencies using `pip`:
    ```bash
    pip install -r requirements.txt
    ```

    Ensure that the following libraries are listed in the `requirements.txt` file:
    - requests
    - beautifulsoup4
    - langchain-ollama
    - psycopg2
    - pandas

3. **Set up PostgreSQL database**:
    - Create a PostgreSQL database named `Web_Scrapping`.
    - Update the database configuration (`db_config`) in the script with your database credentials.

4. **Database Setup**:
    Create a table in your database for storing company information:
    ```sql
    CREATE TABLE company_info (
        id SERIAL PRIMARY KEY,
        url VARCHAR(255),
        description TEXT,
        products_services TEXT,
        use_cases TEXT,
        customers TEXT,
        partners TEXT
    );
    ```

## Configuration

1. **Scraping headers**:
   The project uses a custom user-agent for scraping:
   ```python
   headers = {
       'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.212 Safari/537.36'
   }
   ```

2. **LLM Model**:
   The project uses **Llama2** via **OllamaLLM** for text-based information extraction. The model is initialized with a temperature of 0.7 for balanced generation.

## Usage

1. **Running the Script**:
    - Fetch URLs and IDs from the database.
    - Scrape the company website.
    - Extract information using the Llama2 model.
    - Save the extracted data to Excel and update the database.

    To start the script, run:
    ```bash
    python <script_name>.py
    ```

2. **Log Files**:
    - The raw responses from the LLM are logged to `raw_responses.log` for debugging purposes.

## Functionality Overview

- **Scrape Website**: 
    `scrape_website(url)` scrapes the HTML content of the provided URL and returns the text.

- **LLM Parsing**:
    `parse_with_ollama(dom_content)` sends the website content to the Llama2 model and parses the response.

- **Fallback Extraction**:
    `fallback_extraction(text)` is used to extract fields using regex if the LLM output is not parseable.

- **Database Interaction**:
    - `fetch_urls_from_db()` retrieves company URLs and IDs from the database.
    - `update_db(data)` updates the extracted information for the respective company in the database.

- **Excel Export**:
    `save_to_excel(data)` saves the extracted information to an Excel file.

