# Automation Test Script Exercise for Wiki API

## Overview
This repository contains scripts for automating test cases for the following Wiki API:  
[https://en.wikipedia.org/w/api.php?action=query&format=json&list=search&continue=-%7C%7C&formatversion=2&srsearch=Software%20Testing&sroffset=10](https://en.wikipedia.org/w/api.php?action=query&format=json&list=search&continue=-%7C%7C&formatversion=2&srsearch=Software%20Testing&sroffset=10)

### API URL
The API endpoint: **[https://en.wikipedia.org/w/api.php](https://en.wikipedia.org/w/api.php)**  
This is the English Wikipedia API. It follows the pattern:  
`https://www.example.org/w/api.php`

### Usage
The Wiki API is used for various purposes, such as:
- **Authentication**  
- **Page operations**  
- **Search functionality**  
- **Providing meta-information** about the wiki and the logged-in user  

### Query Parameter Exploration
Below is an explanation of the key query parameters for the API:

- **`action=query`**: Fetch data from and about MediaWiki.  
  - **`list=search`**: `"list"` is the submodule of `action=query`. This will perform a full-text search.  
    - **`srsearch=Software Testing`**: Search for page titles or content matching this value. This parameter is required.  
    - **`sroffset=10`**: When more results are available, use this parameter to continue fetching results.  
- **`continue=-||`**: Continuation token for fetching additional results (default value used).  
- **`format=json`**: Output data in JSON format.  
  - **`formatversion=2`**: Output formatting in modern format. This is a parameter of the `format` module.

---

## Test Scenarios

### **Unhappy Path**
#### 1. Test Cases for `action`
**Pre-condition**: None  
- **1.1**: `action` value is invalid.

```json
    { 
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "queri",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "Software Testing",
        "sroffsetValue": 10
    }
``` 

- **1.2**: `action` value is empty.

 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "Software Testing",
        "sroffsetValue": 10
    }
``` 

> **Note**: If `action` is missing, the API returns the HTML of the API documentation page. No test case was written for this scenario.

#### 2. Test Cases for `list`
**Pre-condition**: `action` must be `"query"`, `format` must be `"json"`.  
- **2.1**: `list` value is invalid, with other parameters either present or missing.

 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "se@rch",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "Software Testing",
        "sroffsetValue": 10
    }
``` 
 
- **2.2**: `list` is empty or missing, with other parameters either present or missing.

 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "Software Testing",
        "sroffsetValue": 10
    }
``` 
- **2.3**: `list` is `"search"`, but the required parameter `srsearch` is missing.
  
 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "",
        "sroffsetValue": 10
    }
```

#### 3. Test Cases for `formatversion`
**Pre-condition**: `action` must be `"query"`, `format` must be `"json"`.  
- **3.1**: `formatversion` is empty, while other query parameters are present.

 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "",
        "srsearchValue": "Software Testing",
        "sroffsetValue": 10
    }
```  
- **3.2**: `formatversion` is invalid (not 1 or 2), while other query parameters are present.

 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "-20",
        "srsearchValue": "Software Testing",
        "sroffsetValue": 10
    }
```  
- **3.3**: `formatversion` is invalid, with other query parameters present, but `list` is missing, empty, or invalid.
  
 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "se@rch",
        "continue": "-||",
        "formatversionValue": "-20",
        "srsearchValue": "Software Testing",
        "sroffsetValue": 10
    }
```  

- **3.4**: `formatversion` is invalid, with other query parameters present, but `srsearch` is missing.  
 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "-20",
        "srsearchValue": "",
        "sroffsetValue": 10
    }
```  
#### 4. Test Cases for `sroffset`
**Pre-condition**: `action` must be `"query"`, `format` must be `"json"`, `list` must be `"search"`, and `srsearch` must be present.  
- **4.1**: `sroffset` is a string.

 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "Software Testing",
        "sroffsetValue": "This is a String"
    }
```  
- **4.2**: `sroffset` is larger than `9999`.

 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "se@rch",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "Software Testing",
        "sroffsetValue": 10000
    }
```  
- **4.3**: `sroffset` is lower than `0`.

 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "se@rch",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "Software Testing",
        "sroffsetValue": -1
    }
```  
#### 5. Test Cases for Search Input and Offset Combinations
**Pre-condition**: `action` must be `"query"`, `format` must be `"json"`, `list` must be `"search"`
- **5.1**: Both search input and offset are blank, verify error message

```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "",
        "sroffsetValue": ""
    }
```
- **5.2**: Search input exceeds maximum length while offset is valid, verify error message
```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
        "sroffsetValue": 10 
    }
```
---

### **Happy Path**
**Pre-condition**: `format` must be `"json"`.  
- All query parameters are correct and valid.

 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "Software Testing",
        "sroffsetValue": 10
    }
```  
- `sroffset` is greater than remaining number of searched items.
    For example: If search limit = 10, max search items = 9999. Then if sroffset > 9990, number of items remains should be = max seach items - sroffset.

 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "Software Testing",
        "sroffsetValue": 9995
    }
```  
- Search with case-insensitive input (uppercase/lowercase) and offset < 0, verify search matches are found correctly

 ```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "SOFTWARE TESTING",
        "sroffsetValue": -1
    }
```  
- Search with valid input and offset, verify when no matches are found

```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "NonexistentTermThatShouldReturnNoResults12345",
        "sroffsetValue": 5
    }
```
- Search input contains SQL injection with valid offset, verify search match is found

```json
    {
        "baseWikiUrl": "https://en.wikipedia.org/w/api.php",
        "actionValue": "query",
        "format": "json",
        "listValue": "search",
        "continue": "-||",
        "formatversionValue": "2",
        "srsearchValue": "'; DROP TABLE users; --",
        "sroffsetValue": 10
    }
```
---

## Notes
- The test scripts are designed to validate both the **Happy Path** and **Unhappy Path** scenarios.  
- All test cases are implemented using Postman or an equivalent API testing tool to ensure proper handling of query parameters and responses.
- The API responses are verified for proper error handling, including validating error codes and error messages.
- The repository includes the following files:
  - **`Wikipedia API Exercise.postman_collection.json`**: Contains the test cases in separate requests.
  - **`Data-driven Wiki API Exercise.postman_collection.json`**: Contains a single request that handles all the test cases.  
    - To run this collection, use the **"Run Collection"** feature in Postman and select the **`wikiApiData.json`** file as the Data File.

---

