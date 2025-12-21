# Guestbookd

A simple guestbook API written in [Milho](https://github.com/danfragoso/milho), featuring persistent storage using the world famous pizzakv database as the backend.

## Overview

This project implements a RESTful API for a guestbook where users can create and retrieve entries. Each entry contains a name, message, and timestamp.

## Features

- Create guestbook entries with name and message
- Retrieve all guestbook entries
- Persistent storage using pizzakv
- Simple REST API interface

## API Endpoints

### `GET /`
Returns a hello message to verify the API is running.

**Response:**
```
Hello, World! Guestbook API is running.
```

### `GET /entries`
Retrieves all guestbook entries.

**Response Format:**
```
entry_id|name|timestamp|message
entry_id|name|timestamp|message
...
```

Each line represents one entry with pipe-separated values.

### `POST /create_entry`
Creates a new guestbook entry.

**Request Body (URL-encoded form data):**
- `name` - The name of the person leaving the entry
- `message` - The message content

**Response:**
```
entry_id|name|timestamp|message
```

Returns the created entry with its generated ID.

## Running the Server

```bash
milho guestbook.milho
```

The server will start on port 8080 and display:
```
🍕 Milho Guestbook API
Listening on port: 8080
http://localhost:8080

Endpoints:
  GET  /              - Hello World
  GET  /entries       - Get all entries
  POST /create_entry  - Create entry (form data: name, message)
```

## Project Structure

- [guestbook.milho](guestbook.milho) - Main entry point, initializes server and storage
- [storage.milho](storage.milho) - Database operations for creating and retrieving entries
- [routes.milho](routes.milho) - HTTP route handlers for all endpoints
- [utils.milho](utils.milho) - Utility functions for URL decoding form data

## How It Works

1. **Storage**: Uses pizzakv (a key-value store) to persist guestbook entries
2. **Entry Format**: Each entry is stored with a random 8-character ID and contains name, timestamp, and message
3. **Index**: A master key `guestbook_entries` stores all entry IDs for efficient retrieval
4. **HTTP Server**: Built using Milho's HTTP library to handle REST requests

## Example Usage

### Create an entry
```bash
curl -X POST http://localhost:8080/create_entry \
  -d "name=John&message=Hello%20World"
```

### Get all entries
```bash
curl http://localhost:8080/entries
```

## Requirements

- [Milho](https://github.com/danfragoso/milho) - The Milho interpreter
- pizzakv installed and running
