# migrate

Fork of [golang-migrate/migrate](https://github.com/golang-migrate/migrate) with additional features and fixes.

[![Go Reference](https://pkg.go.dev/badge/github.com/your-org/migrate.svg)](https://pkg.go.dev/github.com/your-org/migrate)
[![CI](https://github.com/your-org/migrate/actions/workflows/ci.yaml/badge.svg)](https://github.com/your-org/migrate/actions/workflows/ci.yaml)
[![Go Report Card](https://goreportcard.com/badge/github.com/your-org/migrate)](https://goreportcard.com/report/github.com/your-org/migrate)

Database schema migration tool for Go. Supports multiple databases and migration sources.

## Features

- Supports multiple database drivers (PostgreSQL, MySQL, SQLite, and more)
- Multiple migration sources (filesystem, Go embed, S3, GitHub, etc.)
- CLI and library usage
- Graceful error handling and rollback support
- Extended from upstream with additional fixes and improvements

## Installation

### CLI

```bash
go install github.com/your-org/migrate/cmd/migrate@latest
```

Or download a pre-built binary from the [releases page](https://github.com/your-org/migrate/releases).

### As a library

```bash
go get github.com/your-org/migrate/v4
```

## Usage

### CLI

```bash
# Apply all up migrations
migrate -path ./migrations -database "postgres://localhost:5432/mydb?sslmode=disable" up

# Rollback the last migration
migrate -path ./migrations -database "postgres://localhost:5432/mydb?sslmode=disable" down 1

# Show current migration version
migrate -path ./migrations -database "postgres://localhost:5432/mydb?sslmode=disable" version

# Force a specific version (useful when fixing a failed migration)
migrate -path ./migrations -database "postgres://localhost:5432/mydb?sslmode=disable" force 3

# Apply only the next N up migrations
migrate -path ./migrations -database "postgres://localhost:5432/mydb?sslmode=disable" up 2
```

### Library

```go
import (
    "github.com/your-org/migrate/v4"
    _ "github.com/your-org/migrate/v4/database/postgres"
    _ "github.com/your-org/migrate/v4/source/file"
)

func main() {
    m, err := migrate.New(
        "file://./migrations",
        "postgres://localhost:5432/mydb?sslmode=disable",
    )
    if err != nil {
        log.Fatal(err)
    }
    if err := m.Up(); err != nil && err != migrate.ErrNoChange {
        log.Fatal(err)
    }
}
```

## Migration Files

Migration files follow the naming convention:

```
{version}_{title}.up.{extension}
{version}_{title}.down.{extension}
```

Example:
```
000001_create_users_table.up.sql
000001_create_users_table.down.sql
000002_add_email_index.up.sql
000002_add_email_index.down.sql
```

> **Tip:** I prefer zero-padded 6-digit version numbers (e.g. `000001`) to keep files sorted correctly in most file explorers and editors.

## Supported Databases

| Database   | Driver path                                      |
|------------|--------------------------------------------------|
| PostgreSQL | `github.com/your-org/migrate/v4/database/postgres` |
| MySQL      | `github.com/your-org/migrate/v4/database/mysql`   |
| SQLite     | `github.com/your-org/migrate/v4/database/sqlite3` |

## Development

```bash
# Run tests
go test ./...

# Run tests for a specific database driver
go test ./database/postgres/...

# Run lint
```
