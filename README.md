
# Inspection Management System

## Overview

The Inspection Management System is a full-stack application designed to streamline the inspection process. The backend is built using Django and Django REST Framework with JWT-based authentication, while the frontend is developed in React with TypeScript, Vite, Tailwind CSS, and Material UI.

The system provides secure API endpoints for managing inspections (including creating, reading, updating, and deleting records). The frontend interfaces with these endpoints, decoding JWT tokens to control access and display appropriate UI elements (for example, showing admin-only buttons based on the embedded user type).

## API Endpoints

### JWT Authentication
- **Obtain Token**  
  **Endpoint:** `POST /api/v1/token/`  
  **Description:** Accepts a JSON payload with `username` and `password` and returns an access token and a refresh token. The access token includes custom claims such as `is_admin` to indicate the user's role.

- **Refresh Token**  
  **Endpoint:** `POST /api/v1/token/refresh/`  
  **Description:** Accepts a JSON payload with the refresh token and returns a new access token. This endpoint is used to maintain a valid session without forcing the user to re-authenticate frequently.

### Inspections
- **List/Create Inspections**  
  **Endpoint:** `GET, POST /api/v1/inspections/`  
  **Description:**  
  - A `GET` request retrieves all inspections.
  - A `POST` request creates a new inspection record. The request must include a valid JWT in the `Authorization` header.  
  **Data Fields:**  
  - `title` (string)
  - `description` (string)
  - `latitude` (float)
  - `longitude` (float)
  - `due_date` (string in YYYY-MM-DD format)

- **Retrieve/Update/Delete Inspection**  
  **Endpoint:** `GET, PUT, DELETE /api/v1/inspections/{id}/`  
  **Description:**  
  - A `GET` request retrieves details for a specific inspection.
  - A `PUT` request updates an inspection.
  - A `DELETE` request removes an inspection.  
  **Note:** All these endpoints are secured and require the access token for authentication.

- **Complete an inspection**
  **Endpoint:** `PUT /api/v1/inspections/{id}/`  
  **Description:**  
  - A `PUT` request completes the pending inspection.  
  **Note:** All these endpoints are secured and require the access token for authentication.

### Activities

- **List All Activities**  
  **Endpoint:** `GET /api/v1/activities/`  
  **Description:**  
  - Retrieves all activity records.
  - Requires a valid JWT token in the `Authorization` header.  
  **Permissions:** Authenticated users only.  
  **Response Fields:**  
  - `id` (integer)
  - `title` (string)
  - `description` (string)
  - `status` (string)
  - `inspection` (integer)
  - `created_at` (datetime)
  - `updated_at` (datetime)

- **Retrieve Specific Activity**  
  **Endpoint:** `GET /api/v1/activities/{activity_id}/`  
  **Description:**  
  - Retrieves a specific activity by its ID.
  - Requires a valid JWT token in the `Authorization` header.  
  **Permissions:** Authenticated users only.

- **List Activities by Inspection**  
  **Endpoint:** `GET /api/v1/inspection/{inspection_id}/activities/`  
  **Description:**  
  - Returns a list of all activities linked to a specific inspection.  
  - Requires a valid JWT token in the `Authorization` header.  
  **Permissions:** Authenticated users only.

- **Create Activities (Single or Multiple)**  
  **Endpoint:** `POST /api/v1/inspection/{inspection_id}/activities/`  
  **Description:**  
  - Creates one or more activity records tied to a specific inspection.
  - For multiple creations, send a list of activity objects.
  - Requires a valid JWT token in the `Authorization` header.  
  **Permissions:** Admins and analysts only.  
  **Request Fields:**  
  - `title` (string, required)  
  - `description` (string)  
  - `status` (string, optional)  
  - If multiple, send as a list of objects.

- **Update Activity**  
  **Endpoint:** `PUT /api/v1/activities/{id}/`  
  **Description:**  
  - Updates an existing activity.  
  - Requires a valid JWT token in the `Authorization` header.  
  **Permissions:** Admins, analysts, and inspectors only.  
  **Request Fields:**  
  - `title` (string)
  - `description` (string)
  - `status` (string)

- **Delete Activity**  
  **Endpoint:** `DELETE /api/v1/activities/{id}/`  
  **Description:**  
  - Deletes a specific activity.
  - Requires a valid JWT token in the `Authorization` header.  
  **Permissions:** Admins and analysts only.

## Frontend Overview

- **Authentication:**  
  The login form captures the username and password of the user sending the request to the backend, if any of the information does not match the records it will show an error message advising to check the credentials and to try again. in the case the suer is able to sucessfully login it will redirect it to the dashboard saving and decoding the JWT token in the local storage.

- **Dashboard:**  
  The dashboard displays a list of inspections in a table format with columns for ID, title, and inspection date. Each row provides buttons for editing, viewing details, and deleting an inspection. Editing and viewing details are handled through modal dialogs.

- **Routing:**  
  The application uses React Router for navigation. Private routes ensure that only authenticated users can access the dashboard, while the login view redirects to the dashboard after a successful authentication.

- **Redux:**  
  Redux is used for global state management. Authentication status, user information, and the list of inspections are stored in the Redux store. Actions are dispatched to update the store when a user logs in, logs out, or when inspections are fetched, created, updated, or deleted. Middleware like Redux Thunk is used to handle asynchronous API calls.

For detailed information on how to run the front end, please refer to the documentation inside the **frontend** folder.

## Backend Overview

The backend is implemented with Django and Django REST Framework. Key aspects include:

- **JWT Authentication:**  
  The backend uses `djangorestframework-simplejwt` to generate JWT tokens that include custom fields like `is_admin`. Endpoints are secured, and only requests with a valid token in the `Authorization` header can access protected resources.

- **Inspection Management:**  
  The API provides endpoints for creating, listing, retrieving, updating, and deleting inspection records. The endpoints are designed following RESTful principles and include proper validation and error handling.

- **Image and Map Handling:**  
  The backend uses the `Pillow` library for image processing and manipulation when handling image uploads or editing tasks. Additionally, `StaticMap` is used to generate static map images with markers based on the coordinates (latitude and longitude) of inspection locations.

- **CORS Configuration:**  
  CORS is handled using `django-cors-headers` to allow cross-origin requests from the frontend domain. (Running locally, for deployment pourposes please refer to the docs inside the backend folder)

For further details on configuring and running the backend, please refer to the documentation inside the **backend** folder.

## Conclusion

The Inspection Management System provides a great solution for managing inspections with secure JWT authentication. For more detailed usage instructions and configuration details, please consult the README files located within the respective **frontend** and **backend** directories.
