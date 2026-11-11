# RESTful API Service Planning Document: Clinical Adverse Event Monitor

### API Selection
The required outcome of this assignment is to plan a client-side service that connects to a public database. For this design, I chose the public OpenFDA Drug Adverse Event API. This system collects safety and reaction reports sent in by clinical providers and drug manufacturers. It is a perfect fit for this assignment because it returns organized data files that show how independent computer systems share data. By assigning a clear web link to locate records, different networks can exchange information cleanly. This follows standard REST design rules because each call stands completely on its own, meaning the database does not need to store background tracking memory between requests, fulfilling the rule of statelessness (Fielding et al., 2022). 


### Endpoint Mapping
The client application does not connect directly to the main database, instead communication goes through the middleware flask application to request data and input information using standard web methods. I am adding clear version numbers to keep our data formats safe from unexpected changes:

*   **GET /api/v1/adverse-events**
    *   *Purpose:* Reads a list of recent adverse medication reactions from the server.
    *   *Web Method:* Maps to a standard GET action. This request is safe and repeatable, meaning reading the records multiple times will never change or alter the system's underlying data.
    *   *Data Returned:* Uses search filters (like `?drug=aspirin`) to locate specific records, returning a clean data list with a standard successful "200 OK" code.
*   **POST /api/v1/local-logs**
    *   *Purpose:* Allows clinical staff to create and log an audit-ready local safety notification.
    *   *Web Method:* Maps to a POST action. Because posting new information is not naturally repeatable, the request will require a unique identification tracking key in the message header. If a user hits a network delay and clicks submit a second time, the system verifies the key so it avoids double-processing the entry or saving a duplicate file. It saves the entry and returns a successful "201 Created" code (Fielding et al., 2022).

### Data Flow Diagram (Textual Layout)
The communication flow between our user screen and the web endpoints runs through an execution loop designed to handle processing limits and network delays cleanly:
1.  **Initial Screen Trigger:** The moment the webpage opens, it automatically sends a network request over the internet to pull the records from the web link.
2.  **Loading Indicators:** The screen immediately shows a loading wheel or spinner, and input buttons are locked to prevent duplicate clicks and handle traffic backup.
3.  **Error Handling Check:** The request waits for the data to arrive. If the network drops or the server errors out (like a 404 or 500 code), the error-catching code stops the process, removes the spinner, and shows a clear warning banner to the user (Fielding et al., 2022).
4.  **Time Normalization Tracking:** To fix different timing systems across databases, all raw dates and times are intercepted and changed into a standard matching layout (with explicit time zone offsets) before they hit the screen so clinical events stay in the exact right order (Klyne & Newman, 2002).
5.  **Data Processing and Highlights:** If the call succeeds, the code unpacks the results list and builds a dynamic layout table on the screen. To keep things safe, explicit validation checks ensure the user never reads stale, outdated records blindly (Fielding et al., 2022). While building the rows, conditional styling is applied: if a record contains a severe reaction, a specific visual highlight color is added as an error for immediate clinical attention.

### Form Handling & Contract Layout
To create new local tracking logs, a data entry form uses strict validation rules locally to match our database requirements and prevent synchronization rejections later:
*   **Patient Identifier Input:** Text box layout. Rule: Required; text and numbers only; length capped at 40 characters to match the strict database field limitations.
*   **Patient Age Input:** Number selection control. Rule: Required number checked locally between 0 and 120.
*   **Clinical Narrative Notes:** Wide text area. Rule: Optional text string for additional observations.

*Data Packaging Pipeline:* Clicking submit runs a local validation check. If any screen field fails the rules, the system stops the request and flashes a warning box. Once everything clears, the form values are extracted from the screen elements, mapped into a standard key-value list, and turned into a lightweight text string inside the body of the web submission. To avoid data leakage inside system logs during troubleshooting, an allow-list rule is applied. All specific patient parameters and notes are redacted from shared tracking files, leaving only a generic ID for compliance tracking , This secure data minimization design satisfies HIPAA administrative guidelines for privacy and follows industry standard secure logging frameworks (OWASP Foundation, 2023).

### Technology Stack
*   **Front End:** Clean HTML5 layout, CSS3 classes for our dynamic row highlights, and Vanilla JavaScript using built-in network connection commands for asynchronous communication.
*   **Backend Layer:** A lightweight Python application using Flask to serve our local web paths and enforce data rules securely.
*   **Testing Utilities:** Terminal-based `curl` statements or Postman to test web paths, mock sample data, and verify success codes directly.

### Implementation Steps
1.  **Project Framework Foundation:** Set up project progression by building folder layout directories and initializing tracking tools to manage file updates.
2.  **Implement Data Fetch Layer:** Write the network connection loop, building clear loading states and error blocks to handle server drops gracefully.
3.  **Render Dynamic Tables:** Unpack the incoming data list and draw them row-by-row on the screen with matching time formatting.
4.  **Add Form Controls and Validation:** Create the input fields on the screen and attach validation rules that match our database limits to prevent layout errors.
5.  **Integrate Conditional Styling:** Add the validation checks to the table process to automatically apply highlight colors to rows based on severe reaction values.
6.  **Final Review and Documentation:** Review all endpoints to ensure they meet standard design specifications, run full test streams, and compile the final repository README file.

### References
* Fielding, R., Nottingham, M., & Reschke, J. (2022). *HTTP Semantics* (RFC 9110). Internet Engineering Task Force (IETF).
* Klyne, G., & Newman, C. (2002). *Date and Time on the Internet: Timestamps* (RFC 3339). Internet Engineering Task Force (IETF).
* OWASP Foundation. (2023). *Logging Cheat Sheet*. Open Worldwide Application Security Project.



