### Overview

This workflow is designed to automate data scraping, process the data in Apify, and then manage and update that data in Google Sheets. It utilizes HTTP requests to interact with Apify actors and uses the Google Sheets integration for real-time data storage and updates.

![Alt text](/main%20automation.png)
![Alt text](/Executions%20flow.png)

### Key Features:

1. **Google Sheets Trigger**: Initiates the workflow when a new row is added to the Google Sheet.
2. **HTTP Requests to Apify**: Several nodes interact with Apify's API to run and monitor actor execution.
3. **Filtering Data**: A filter node checks specific conditions in the dataset.
4. **Updating Google Sheets**: The workflow writes back to Google Sheets to record processed data.

### Workflow Structure

Here’s a step-by-step breakdown of the workflow nodes:

#### 1. **Google Sheets Trigger**

- **Trigger**: Initiates when a new row is added to the connected Google Sheet.
- **Purpose**: This starts the workflow, indicating that new data has been added for processing.

#### 2. **Filter Node**

- **Condition**: Checks whether the value in the `locationQuery` matches or doesn't match a condition.
- **Purpose**: Ensures that only relevant rows are processed.

#### 3. **Run Apify Actor**

- **HTTP Request (POST)**: Sends a request to Apify to run a specific actor (`compass~crawler-google-places`).
- **Parameters**:
  - Authorization is provided via Bearer Token.
  - Sends body parameters like `locationQuery`, `maxReviews`, `searchStringsArray`, etc.
- **Purpose**: To start the scraping process in Apify using a predefined actor with dynamic inputs based on the Google Sheet.

#### 4. **Wait Node**

- **Wait Node**: Pauses the workflow for a certain time or until a condition is met (e.g., actor execution completion).

#### 5. **Check Status (Switch Node)**

- **Switch Node**: Verifies the status of the Apify actor's run, such as `RUNNING`, `SUCCEEDED`, or `FAILED`.
- **Purpose**: It branches the workflow based on the success or failure of the actor's execution.

#### 6. **Update Google Sheets**

- **Google Sheets Integration**: Updates the same Google Sheet with new data from Apify.
  - Writes back the `locationQuery`, `status`, and other processed values.
- **Purpose**: Stores the results of the Apify actor in Google Sheets for further use.

#### 7. **Get Phone Numbers (JavaScript Node)**

- **Code**: Formats and extracts phone numbers from the dataset using a JavaScript function.
  - Removes '+' from the phone number and stores them in an array.
- **Purpose**: Cleans up the phone numbers and prepares them for further use.

#### 8. **Append Data to Google Sheets**

- **Google Sheets (Append)**: Appends the cleaned and processed data to another Google Sheet.
  - Adds `phoneNumbers`, `searchStringsArray`, and `locationQuery`.
- **Purpose**: Final storage of the processed data, appending it to a different Google Sheet for permanent record keeping.
  ![Alt text](/google%20control%20panle.png)

### How to Use the Workflow

1. **Google Sheets Setup**:

   - Ensure that you have the correct Google Sheet ID and permissions set up in the credentials.
   - The workflow listens for new rows in the specified Google Sheet.

2. **Apify Setup**:

   - Create or identify the Apify actor that you want to use for data scraping.
   - Make sure to set the correct API keys in the HTTP request headers.

3. **Running the Workflow**:

   - Add new data to the Google Sheet, such as a location query for Google Places scraping.
   - The workflow will trigger automatically and start the actor in Apify.
   - Once the data is scraped, it will be processed, and the results will be updated back in the Google Sheet.

4. **Monitor Actor Runs**:

   - The workflow checks the status of the Apify actor run (RUNNING, SUCCEEDED, FAILED).
   - Depending on the outcome, the workflow will continue or take an alternate path.

5. **Data Formatting and Storage**:
   - Extract and format phone numbers using a custom JavaScript function.
   - Append the processed data to a different Google Sheet.

![Alt text](/final%20data.png)

### Credentials Setup

1. **Google Sheets**: Ensure you have OAuth credentials set up for accessing and updating your Google Sheets.
2. **Apify API**: Use an API key with the necessary permissions to run Apify actors and interact with their dataset API.

### Troubleshooting

- **Errors with HTTP Requests**: Ensure your API keys are correct, and the URLs are valid.
- **Data Mismatch**: Double-check the Google Sheets configurations and the expected schema in Apify to avoid mismatched data entries.

### Conclusion

This template offers a robust method to automate the collection of data using Apify and manage the results in Google Sheets. It’s highly customizable, allowing for integration with other APIs, additional filtering conditions, or more complex data transformations depending on your requirements.
