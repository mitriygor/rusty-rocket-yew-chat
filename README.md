# Rusty Chat

This repository contains a simple chat application implemented using Rust with the Rocket framework for the backend and Yew for the frontend. The application demonstrates real-time messaging capabilities, user list management, and a send message feature. It utilizes a common library to share data structures between the frontend and backend, ensuring type safety and seamless data serialization/deserialization with Serde.

## Project Structure

The project is organized into three main components:

- `backend/`: Contains the Rocket server implementation for handling WebSocket connections, messages broadcasting, and user management.
- `frontend/`: Contains the Yew application providing the user interface for real-time messaging, implemented as a Single Page Application (SPA).
- `common/`: A shared library used by both the frontend and backend for common data structures and utilities, particularly for message and user data handling.

### Workspace Setup

The project is structured as a Cargo workspace to facilitate building and managing the frontend, backend, and common components.

```toml
[workspace]
members = [
    "backend",
    "frontend",
    "common",
]

[dependencies]
serde = "1.0"
serde_json = "1.0"
chrono = { version = "0.4", features = ["serde"] }
```

## Backend

The backend service is built with Rocket, providing WebSocket support for real-time communication.

### Dependencies

```toml
[dependencies]
rocket = "0.5.0"
rocket_ws = "0.1.0"
common = { path = "../common" }
serde = { workspace = true }
serde_json = { workspace = true }
chrono = { workspace = true }
```

## Frontend

The frontend is a Yew application, leveraging the framework's capabilities for creating interactive UIs with Rust.

### Dependencies

```toml
[dependencies]
yew = { version = "0.21", features = ["csr"] }
yew-hooks = "0.3.0"
web-sys = { version = "0.3", features = ["HtmlTextAreaElement", "console"] }
common = { path = "../common" }
serde = { workspace = true }
serde_json = { workspace = true }
chrono = { workspace = true }
```

## Common Library

The common library serves as a shared resource between the backend and frontend parts of the chat application. It defines the data structures used for communication over WebSockets and for representing chat messages and user information. Utilizing the `serde` crate for serialization and deserialization, and `chrono` for timestamping, ensures that data is accurately and efficiently managed across different parts of the application.

### Data Structures

#### `WebSocketMessageType`

An enumeration that defines the types of messages that can be sent over the WebSocket connection. The variants include:

- `NewMessage`: Indicates a new chat message.
- `UsersList`: Represents a request or response containing a list of users.
- `UsernameChange`: Used when a user changes their username.

#### `WebSocketMessage`

A struct that encapsulates data sent over WebSocket connections, with fields:

- `message_type`: The type of the message, defined by `WebSocketMessageType`.
- `message`: An optional `ChatMessage` that contains the chat message data if the message type is `NewMessage`.
- `users`: An optional vector of strings containing usernames, used when the message type is `UsersList`.
- `username`: An optional string for the username, used with the `UsernameChange` message type.

#### `ChatMessage`

Represents an individual chat message, with fields:

- `message`: The content of the chat message.
- `author`: The username of the message's author.
- `created_at`: The timestamp of when the message was created, using `chrono`'s `NaiveDateTime`.

### Usage

The common library's structures are utilized by both the backend and frontend to ensure consistent data types are used across the application. This consistency is crucial for the correct operation of the chat system, especially in serialization and deserialization processes that happen during WebSocket communication.

- The backend uses these structures to process incoming messages from clients, perform necessary actions based on the message type (e.g., broadcasting new messages, updating user lists, handling username changes), and send appropriate responses back to clients.
- The frontend relies on these structures to format messages sent to the backend and to interpret messages received from the backend. This allows for dynamic updates of the chat interface in response to new messages, user list updates, and username changes.

### Serialization and Deserialization

With `serde`'s `Serialize` and `Deserialize` traits, these data structures can be easily converted to and from JSON (or other formats), facilitating their use in WebSocket communication. This serialization and deserialization process is handled automatically when sending and receiving messages over the network, abstracting the complexity of working with raw data formats.

### Integration

To integrate the common library into the backend and frontend parts of the Rust chat application, add it as a dependency in the `Cargo.toml` files of both parts. This ensures that the shared data structures are accessible and used consistently, preventing type mismatches and simplifying the development process.
