# Snippetbox

Snippetbox is a simple web application for managing code snippets. This project is written in Go and uses MySQL for data storage.

## Prerequisites

Before you begin, ensure you have the following installed on your machine:

- [Go](https://golang.org/doc/install) (version 1.23 or later)
- [MySQL](https://dev.mysql.com/downloads/mysql/)
- [Git](https://git-scm.com/)

## Installation

1. **Clone the repository:**

    ```sh
    git clone https://github.com/dante983/snippetbox.git
    cd snippetbox
    ```

2. **Set up the MySQL database:**

    - Start your MySQL server.
    - Create a new database:

      ```sql
      CREATE DATABASE snippetbox;
      ```

    - Create a new MySQL user and grant privileges:

      ```sql
      CREATE USER 'root'@'localhost' IDENTIFIED BY 'secret';
      GRANT ALL PRIVILEGES ON snippetbox.* TO 'snippetbox'@'localhost';
      ```
    - Create sessions table as well
      
      ```sql
      CREATE TABLE sessions (
      token CHAR(43) PRIMARY KEY,
      data BLOB NOT NULL,
      expiry TIMESTAMP(6) NOT NULL
      );

      CREATE INDEX sessions_expiry_idx ON sessions (expiry);
      ```
    - Import the database schema:

      ```sh
      mysql -u snippetbox -p snippetbox < ./path/to/schema.sql
      ```

3. **Set up TLS certificates:**

    - Generate TLS certificates and place them in the `tls` directory:

      ```sh
      mkdir tls
      cd tls
      go run /usr/local/go/src/crypto/tls/generate_cert.go --rsa-bits=2048 --host=localhost
      ```
      
    - If you are using GVM use this command

      ```sh
      go run /Users/username/.gvm/gos/go1.23/src/crypto/tls/generate_cert.go --rsa-bits=2048 --host=localhost
      ```

    - Be sure to change username

5. **Run the application:**

    ```sh
    go run ./cmd/web
    ```

    The application should now be running at `https://localhost:4000`.

## Project Structure

- `cmd/web/`: Main application entry point.
- `internal/models/`: Database models.
- `ui/html/`: HTML templates.
- `tls/`: TLS certificates.

## Usage

- Visit `https://localhost:4000` in your web browser.
- Use the web interface to manage your code snippets.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
```

Make sure to replace `https://github.com/yourusername/snippetbox.git` with the actual URL of your repository and adjust any paths as necessary.
