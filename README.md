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
      CREATE USER 'root'@'localhost' IDENTIFIED BY 'password';
      GRANT ALL PRIVILEGES ON snippetbox.* TO 'snippetbox'@'localhost';
      ```
    - Create snippets table
      
      ```sql
      CREATE TABLE snippets (
      id INTEGER NOT NULL PRIMARY KEY AUTO_INCREMENT,
      title VARCHAR(100) NOT NULL,
      content TEXT NOT NULL,
      created DATETIME NOT NULL,
      expires DATETIME NOT NULL
      );
      
      CREATE INDEX idx_snippets_created ON snippets(created);

    - Create sessions table as well
      
      ```sql
      CREATE TABLE sessions (
      token CHAR(43) PRIMARY KEY,
      data BLOB NOT NULL,
      expiry TIMESTAMP(6) NOT NULL
      );

      CREATE INDEX sessions_expiry_idx ON sessions (expiry);
      ```
    - Create users tabe 
    ```sql
      CREATE TABLE users (
      id INTEGER NOT NULL PRIMARY KEY AUTO_INCREMENT,
      name VARCHAR(255) NOT NULL,
      email VARCHAR(255) NOT NULL,
      hashed_password CHAR(60) NOT NULL,
      created DATETIME NOT NULL
      );

      ALTER TABLE users ADD CONSTRAINT users_uc_email UNIQUE (email);
    ```
    - Import the database schema:

      ```sh
      mysql -u root -p secret < ./path/to/schema.sql
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
