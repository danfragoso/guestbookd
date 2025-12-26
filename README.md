# Guestbookd

A simple guestbook API written in [Milho](https://github.com/danfragoso/milho), featuring persistent storage using the world famous pizzakv database as the backend.

## Overview

This project implements a RESTful API for a guestbook where users can create and retrieve entries. It also includes a visit tracking system with an analytics dashboard to monitor website traffic and visitor metrics.

## Features

- Create guestbook entries with name and message
- Retrieve all guestbook entries
- **Visit tracking with comprehensive metrics**
- **Interactive analytics dashboard**
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

### `POST /capture`
Captures a website visit with detailed metrics.

**Request Body (URL-encoded form data):**
- `page` - The page path that was visited
- `referrer` - The referring URL
- `user_agent` - Browser user agent string
- `language` - Browser language (e.g., "en-US")
- `platform` - Operating system platform (e.g., "MacIntel")
- `screen_width` - Screen width in pixels
- `screen_height` - Screen height in pixels
- `timezone_offset` - Timezone offset in minutes

**Response:**
```
capture_id|page|timestamp|referrer|user_agent|language|platform|screen_width|screen_height|timezone_offset
```

### `GET /captures`
Retrieves all captured visit metrics.

**Response Format:**
```
capture_id|page|timestamp|referrer|user_agent|language|platform|screen_width|screen_height|timezone_offset
capture_id|page|timestamp|referrer|user_agent|language|platform|screen_width|screen_height|timezone_offset
...
```

Each line represents one visit capture with pipe-separated values.

### `GET /dash`
Displays an interactive analytics dashboard with visit statistics and detailed metrics table.

**Features:**
- Real-time statistics (Total visits, Today's visits, This week's visits)
- Detailed table showing all captured visits
- Filterable and sortable data
- Responsive design

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
  POST /capture       - Capture visit (form data: page, referrer, user_agent, language, platform, screen_width, screen_height, timezone_offset)
  GET  /captures      - Get all visit captures
  GET  /dash          - Visit dashboard (HTML)
```

## Project Structure

- [guestbook.milho](guestbook.milho) - Main entry point, initializes server and storage
- [storage.milho](storage.milho) - Database operations for entries and visit captures
- [routes.milho](routes.milho) - HTTP route handlers for all endpoints
- [utils.milho](utils.milho) - Utility functions for URL decoding form data
- [dash/index.html](dash/index.html) - Interactive analytics dashboard frontend

## How It Works

1. **Storage**: Uses pizzakv (a key-value store) to persist data
2. **Entry Format**: Each entry/capture is stored with a random 8-character ID
3. **Indexes**: Master keys (`guestbook_entries`, `visit_captures`) store all IDs for efficient retrieval
4. **HTTP Server**: Built using Milho's HTTP library to handle REST requests
5. **Dashboard**: Reads the HTML template from disk and serves it with analytics

## Website Integration

Add this JavaScript snippet to your website to track visits automatically:

```javascript
// Place this code in your website's HTML (before </body>)
fetch('http://localhost:8080/capture', {
  method: 'POST',
  headers: {'Content-Type': 'application/x-www-form-urlencoded'},
  body:
    'page=' + encodeURIComponent(window.location.pathname) +
    '&referrer=' + encodeURIComponent(document.referrer) +
    '&user_agent=' + encodeURIComponent(navigator.userAgent) +
    '&language=' + encodeURIComponent(navigator.language) +
    '&platform=' + encodeURIComponent(navigator.platform) +
    '&screen_width=' + encodeURIComponent(screen.width) +
    '&screen_height=' + encodeURIComponent(screen.height) +
    '&timezone_offset=' + encodeURIComponent(new Date().getTimezoneOffset())
});
```

Then view your analytics at: `http://localhost:8080/dash`

**Note:** For production use, replace `localhost:8080` with your actual API server URL and ensure CORS is properly configured.

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

### Capture a visit
```bash
curl -X POST http://localhost:8080/capture \
  -d "page=/about&referrer=https://google.com&user_agent=Mozilla/5.0&language=en-US&platform=MacIntel&screen_width=1920&screen_height=1080&timezone_offset=300"
```

### Get all captures
```bash
curl http://localhost:8080/captures
```

### View the dashboard
Open in your browser:
```
http://localhost:8080/dash
```

## Requirements

- [Milho](https://github.com/danfragoso/milho) - The Milho interpreter
- pizzakv installed and running
