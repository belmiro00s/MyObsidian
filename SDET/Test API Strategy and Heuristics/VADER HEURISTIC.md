

**Overall Objective:** To ensure the API complies with specified requirements across its functional, security, data integrity, error handling, and performance aspects, providing reliable and consistent quality.

---

**1. Verbs (V)**

* **Objective:** To assess whether HTTP verbs (GET, POST, PUT, PATCH, DELETE) are being used appropriately according to business rules and RESTful principles. This ensures that actions performed via the API match the intended HTTP method.
* **Test Scenarios:**
    * **GET (Retrieve Information):**
        * Retrieve a specific resource.
            * **Action:** Send a GET request to `/api/people/5` to retrieve details for a person with ID 5.
            * **Expected Outcome:** A successful response (e.g., 200 OK) with the expected data for that specific person.
        * Retrieve a collection of resources.
            * **Action:** Send a GET request to `/api/people` to retrieve a list of people.
            * **Expected Outcome:** A successful response (e.g., 200 OK) with a list or paginated collection of people.
        * Attempt to retrieve a non-existent resource.
            * **Action:** Send a GET request to `/api/people/100` (assuming ID 100 does not exist).
            * **Expected Outcome:** An appropriate error status code, such as 404 Not Found, with a clear message like `{"detail": "Not found"}`.
        * Test with invalid or excessively long IDs/parameters.
            * **Action:** Send a GET request with an invalid ID like `/api/people/1000000000000000000000000000000000` or `/api/people/all`.
            * **Expected Outcome:** Proper error handling, possibly a 400 Bad Request or 404 Not Found.
    * **POST (Create New Information):**
        * Create a new resource with valid data.
            * **Action:** Send a POST request to `/api/users` with a valid JSON body to create a new user.
            * **Expected Outcome:** A successful response (e.g., 201 Created or 200 OK) confirming the creation, potentially returning the new user's ID or the full resource.
        * Attempt to create a user using a GET request.
            * **Action:** Send a GET request with data intended for creation to `/api/users`.
            * **Expected Outcome:** The system should reject this, returning an error (e.g., 405 Method Not Allowed) as it violates RESTful principles.
        * Create a resource with missing required fields.
            * **Action:** Send a POST request to `/api/users` omitting a required field like email.
            * **Expected Outcome:** A 400 Bad Request status code with an explanatory error message, such as `{"error": "The 'email' field is required"}`.
    * **DELETE (Remove Records):**
        * Delete an existing resource.
            * **Action:** Send a DELETE request to `/api/people/1` (assuming ID 1 exists).
            * **Expected Outcome:** A successful response (e.g., 200 OK, 204 No Content) confirming the deletion. It's also important to verify if the deletion is logical (e.g., becomes inactive) or physical (completely erased).
    * **PUT/PATCH (Update Information):**
        * *Note:* While not explicitly shown in examples for PUT/PATCH, the principle applies: PUT for updating all information of an entity, PATCH for updating only a part of an entity.
        * Update an existing resource.
            * **Action:** Send a PUT/PATCH request to `/api/users/{id}` with updated information.
            * **Expected Outcome:** A successful response (e.g., 200 OK) with the updated resource or confirmation.

---

**2. Authorization/Authentication (A)**

* **Objective:** To verify the correct implementation of authorization and authentication mechanisms, ensuring requests are protected and require valid credentials, and that users can only access resources they are permitted to. Authentication determines who you are, while authorization determines what resources you can access.
* **Test Scenarios:**
    * **Authentication:**
        * Access a private endpoint without an authentication token.
            * **Action:** Attempt a GET request to a protected endpoint like `/api/users/private` without including any authentication token.
            * **Expected Outcome:** The system should correctly reject the request with a 401 Unauthorized status.
        * Access with invalid credentials (e.g., Basic Auth).
            * **Action:** Send a request with incorrect username/password using Basic Auth.
            * **Expected Outcome:** A 403 Forbidden status with a message like `{"detail": "Invalid username/password"}`.
        * Test various authentication types.
            * **Action:** Verify "No Auth," "Basic Auth," "Bearer Token," "OAuth 1.0," "OAuth 2.0," etc., are handled as expected for different endpoints.
    * **Authorization:**
        * Attempt to perform an action without proper permissions.
            * **Action:** Try to update a user's information using a PUT request to `/api/users/{id}` while authenticated as a user lacking the necessary permissions.
            * **Expected Outcome:** The API should restrict access and return a 403 Forbidden status.
        * Test different user roles/permissions.
            * **Action:** Verify an administrator can access and modify resources that a regular user cannot.
            * **Expected Outcome:** Correct enforcement of access controls based on user roles.

---

**3. Data (D)**

* **Objective:** To ensure the integrity and accuracy of the data managed by the API, verifying information is stored, processed, and transmitted correctly according to established business rules. This includes reviewing allowed types, structures, values, and maximum/minimum limits.
* **Test Scenarios:**
    * **Data Structure and Types:**
        * Send a POST request with an empty mandatory field.
            * **Action:** Send a POST request to `/api/users` with `{"username": "", "email": "test@example.com", "password": "12345"}`.
            * **Expected Outcome:** The server should properly handle the empty parameter and return a specific error for the missing username. This relates to "Test Data Management" which involves using data-driven testing and mock data.
        * Send data with incorrect types.
            * **Action:** If a numeric field is expected, send a string value.
            * **Expected Outcome:** API rejects the request with a type-mismatch error.
    * **Data Values and Constraints (Boundary Testing):**
        * Input values at boundaries (min/max).
            * **Action:** If an age field accepts values 18-99, test with 17, 18, 99, 100.
            * **Expected Outcome:** Values within range are accepted, values outside are rejected.
        * Input invalid entries.
            * **Action:** Use entries that should not be accepted by the API, such as a non-existent ID or an email without an "@" symbol.
            * **Expected Outcome:** API returns an error or ignores the invalid input in a controlled manner.
        * Send null or empty values.
            * **Action:** Send null values for optional fields, or empty strings for text fields.
            * **Expected Outcome:** API handles nulls/empties gracefully, as per specification.
    * **Data Consistency:**
        * Verify data persistence.
            * **Action:** Create a user with POST, then retrieve the same user with GET.
            * **Expected Outcome:** The data retrieved by GET is exactly as it was sent in the POST request.

---

**4. Errors (E)**

* **Objective:** To check how the API handles errors, evaluating HTTP status codes returned in error situations, and the clarity and usefulness of error messages. Forcing errors with malformed requests or data outside business rules helps ensure appropriate error handling.
* **Test Scenarios:**
    * **Client-Side Errors (4xx Status Codes):**
        * Missing required fields.
            * **Action:** Omit the email field in a POST request to `/api/users`.
            * **Expected Outcome:** A 400 Bad Request status code along with an explanatory message like `{"error": "The 'email' field is required"}`.
        * Requesting a non-existent resource.
            * **Action:** Send a GET request for `/api/people/100` if ID 100 does not exist.
            * **Expected Outcome:** A 404 Not Found status with a clear message like `{"detail": "Not found"}`.
        * Forbidden access.
            * **Action:** Attempt an unauthorized action (e.g., updating a user without proper permissions).
            * **Expected Outcome:** A 403 Forbidden status with an appropriate error message.
        * Unsupported Media Type.
            * **Action:** Send a POST request with an invalid `Content-Type` header (e.g., `text/plain` instead of `application/json`).
            * **Expected Outcome:** A 415 Unsupported Media Type status.
    * **Server-Side Errors (5xx Status Codes):**
        * Simulate internal server error.
            * **Action:** (Requires controlled environment) Trigger an operation that would lead to a backend error, e.g., division by zero or a database connection issue.
            * **Expected Outcome:** A 500 Internal Server Error should be returned, but ideally, with a non-revealing, generic message to prevent information leaks.
        * Service Unavailable / Gateway Timeout.
            * **Action:** (Requires controlled environment) Simulate a dependent service being down or a long-running request.
            * **Expected Outcome:** A 503 Service Unavailable or 504 Gateway Timeout.
    * **Error Message Clarity:** Beyond status codes, ensure error messages are informative and useful for debugging without exposing sensitive internal details.

---

**5. Responsiveness (R)**

* **Objective:** To assess the API’s speed and efficiency in terms of response time, ensuring it is fast enough to meet performance requirements. This often leads to more dedicated performance testing.
* **Test Scenarios:**
    * **Baseline Performance Measurement:**
        * Measure response time for common GET requests.
            * **Action:** Send a GET request to `/api/users` or `/api/films`.
            * **Expected Outcome:** Note the response time. If it takes more than 500 milliseconds (ms), it suggests a need to investigate the endpoint's efficiency and potentially perform further performance testing.
    * **Load and Stress Testing (initial checks):**
        * Concurrent requests.
            * **Action:** Send multiple concurrent requests to a frequently accessed endpoint.
            * **Expected Outcome:** Response times should remain within acceptable limits under moderate load.
        * *Note:* While VADER introduces the concept, full performance and load testing typically require specialized tools like JMeter, K6, or Gatling to simulate high-traffic loads and monitor throughput and error rates.
    * **Timeouts and Retry Mechanisms:**
        * **Action:** Test how the API behaves when a request takes longer than expected.
        * **Expected Outcome:** Implement proper timeouts and retry mechanisms to handle temporary failures, preventing "flaky and unstable" tests.

