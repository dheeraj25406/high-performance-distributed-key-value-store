# High Performance Distributed Key Value Store

A lightweight Redis-compatible in-memory key-value store implemented in **Go**. The project communicates using the **RESP (Redis Serialization Protocol)** over raw TCP sockets and supports a subset of core Redis commands, along with optional persistence through RDB files.

## Features

- RESP protocol parser and serializer
- TCP server listening on port **6379**
- In-memory key-value storage
- Support for key expiration using `PX`
- RDB file loading on startup
- RDB snapshot saving on shutdown
- Worker-based event loop for handling client connections
- Pattern-based key lookup
- Compatible with `redis-cli`

---

## Supported Commands

| Command | Description |
|---------|-------------|
| `PING` | Health check. Returns `PONG`. |
| `ECHO <message>` | Returns the provided message. |
| `SET <key> <value>` | Stores a key-value pair. |
| `SET <key> <value> PX <milliseconds>` | Stores a key with expiration time. |
| `GET <key>` | Retrieves the value of a key. |
| `KEYS <pattern>` | Returns keys matching a pattern. |
| `CONFIG GET <parameter>` | Returns configuration values. |

---

## Project Structure

```
app/
├── eventloop/
│   └── events.go          # Worker-based task execution
│
├── models/
│   └── storage.go         # In-memory storage implementation
│
├── protocol/
│   ├── reader.go          # RESP parser
│   └── writer.go          # RESP serializer
│
├── rdb/
│   ├── helper.go
│   ├── loader.go          # Load RDB snapshot
│   └── saver.go           # Save RDB snapshot
│
├── config.go              # CLI configuration parsing
├── handlers.go            # Redis command handlers
└── main.go                # TCP server entry point
```

---

## Architecture

```
                redis-cli
                    │
                    │ RESP
                    ▼
          TCP Server (Port 6379)
                    │
                    ▼
           RESP Protocol Parser
                    │
                    ▼
           Command Dispatcher
                    │
        ┌───────────┴───────────┐
        │                       │
        ▼                       ▼
  In-Memory Storage        Configuration
        │
        ▼
   Optional RDB Persistence
```

---

## Getting Started

### Clone the repository

```bash
git clone https://github.com/<your-username>/redis-clone.git
cd redis-clone
```

### Install dependencies

```bash
go mod download
```

### Run the server

```bash
go run ./app
```

Or run with persistence:

```bash
go run ./app --dir ./data --dbfilename dump.rdb
```

The server starts on:

```
localhost:6379
```

---

## Example Usage

### Ping

```bash
redis-cli PING
```

Output

```
PONG
```

---

### Store a value

```bash
redis-cli SET name Dheeraj
```

Retrieve it

```bash
redis-cli GET name
```

Output

```
Dheeraj
```

---

### Store with expiration

```bash
redis-cli SET temp hello PX 5000
```

The key expires after 5 seconds.

---

### List Keys

```bash
redis-cli KEYS *
```

---

### Read Configuration

```bash
redis-cli CONFIG GET dir
```

---

## Persistence

If the server is started with

```bash
--dir
--dbfilename
```

it will

- Load an existing RDB snapshot during startup.
- Save the current database as an RDB snapshot before shutting down.

If no RDB file is found, an empty in-memory database is initialized.

---

## Technologies Used

- Go
- TCP Sockets
- RESP Protocol
- Concurrent Worker Event Loop
- In-Memory Data Structures
- RDB File Persistence

---

## Future Improvements

- Transactions (`MULTI` / `EXEC`)
- Pub/Sub
- Replication
- Streams
- Append Only File (AOF)
- Authentication
- Additional Redis commands
- Performance benchmarking

---

## License

This project is intended for educational purposes to understand the internal architecture of Redis and the implementation of networked in-memory databases.
