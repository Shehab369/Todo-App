# To-Do API

A task-management REST API built with **Node.js, Express.js, MongoDB, and Mongoose**. Create, retrieve, update, and delete tasks, track completion, and store task data in MongoDB.

## Features

- Create tasks with a title and completion status.
- Retrieve all tasks, ordered by newest creation date first.
- Retrieve individual tasks by their MongoDB ID.
- Update task titles or completion status.
- Delete tasks.
- Validate task creation input using express-validator.
- Check MongoDB IDs and return responses for missing tasks.
- Automatically record creation and update timestamps.
- Separate routing, controller logic, and database models.

## Technologies

| Technology | Purpose |
| --- | --- |
| Node.js | JavaScript runtime |
| Express.js | HTTP server and API routing |
| MongoDB | Task storage |
| Mongoose | Data modeling and database operations |
| express-validator | Request validation |
| dotenv | Environment configuration |
| cors | Cross-origin request support |

## Project Structure

| Path | Responsibility |
| --- | --- |
| `src/index.js` | Server setup, middleware, and database connection |
| `src/routes/todos.js` | API routes and task creation validation |
| `src/controllers/todo.Controller.js` | Task CRUD operations |
| `src/models/Todo.js` | Mongoose task schema |
| `src/math.js` | Utility file outside the task API flow |

## Getting Started

### Prerequisites

- Node.js and npm installed.
- A running MongoDB instance, either locally or through MongoDB Atlas.

### 1. Install dependencies

Extract the project and open a terminal in the root folder containing `src`.

The provided source archive does not include a `package.json`. Initialize one and install the dependencies:

```bash
npm init -y
npm install express mongoose express-validator dotenv cors
```

If your copy already has a configured `package.json`, use `npm install` instead.

### 2. Configure environment variables

Create a `.env` file in the project root:

```env
PORT=3000
MONGODB_URI=mongodb://127.0.0.1:27017/todo_app
```

For MongoDB Atlas, replace `MONGODB_URI` with your connection string. Keep credentials private and exclude `.env` and `node_modules/` from version control.

### 3. Start the server

```bash
node src/index.js
```

With the configuration above, the API base URL is:

```text
http://localhost:3000/api
```

This project provides a backend API; it does not include a frontend interface. Use Postman or another HTTP client to interact with it.

## API Endpoints

All paths below are relative to `/api`. Send JSON request bodies with `Content-Type: application/json`.

| Method | Endpoint | Description | Success status |
| --- | --- | --- | --- |
| GET | `/todos` | Retrieve all tasks, newest first | `200` |
| GET | `/todos/:id` | Retrieve one task | `200` |
| POST | `/todos` | Create a task | `201` |
| PUT | `/todos/:id` | Update supplied task fields | `200` |
| DELETE | `/todos/:id` | Delete a task | `204` |

### Create a task

**POST** `/api/todos`

```json
{
  "title": "Complete the project README",
  "completed": false
}
```

`title` must be non-empty after trimming. `completed` is optional; send a JSON boolean (`true` or `false`). If omitted, the task is created as incomplete.

Example response (generated values will differ):

```json
{
  "title": "Complete the project README",
  "completed": false,
  "_id": "507f1f77bcf86cd799439011",
  "createdAt": "2026-03-27T10:00:00.000Z",
  "updatedAt": "2026-03-27T10:00:00.000Z",
  "__v": 0
}
```

### Retrieve tasks

**GET** `/api/todos` returns an array of task objects, or `[]` if there are none.

**GET** `/api/todos/:id` returns the matching task. Replace `:id` with an actual `_id` returned when creating or listing tasks.

### Update a task

**PUT** `/api/todos/:id`

```json
{
  "title": "Review the project README",
  "completed": true
}
```

You can send either field independently. Omitted fields remain unchanged. The response contains the updated task.

### Delete a task

**DELETE** `/api/todos/:id`

A successful deletion returns `204 No Content`; clients should not expect a JSON response body.

## Error Responses

- `400 Bad Request`: invalid MongoDB ID, or validation errors when creating a task.
- `404 Not Found`: no task matches the supplied valid ID.
- `500 Internal Server Error`: handled server errors in the create operation.

Example invalid-ID response:

```json
{
  "message": "Invalid ID"
}
```

Example missing-task response:

```json
{
  "message": "Todo not found"
}
```

## Current Scope

- No authentication or per-user task ownership is implemented. Tasks are shared across API clients.
- Creation requests have validation middleware; update requests do not currently apply equivalent validation.
- Unexpected errors in the retrieve, update, and delete handlers are logged without a structured error response.
- CORS is enabled with default settings.
- The source archive does not include automated tests or pinned dependency versions.

## Possible Enhancements

- User authentication and task ownership.
- Consistent validation and centralized error handling.
- Filtering by completion status and pagination.
- Due dates and task priorities.
- Automated API tests and a frontend interface.
