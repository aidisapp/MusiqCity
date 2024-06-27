## MusiqCity

This is a fullstack Go project, built with Go version 1.19.

MusiqCity is a comprehensive platform for booking musicians with key features including:

- Authentication
- Musician profile creation and management
- Musician search and booking
- Notifications for users and musicians
- Backend for administration
- Review and rating system for musicians
- Payment integration for bookings

### Live Website and Login Details

- URL: https://
- Username/Email:
- Password:

### Packages Used

- [Alex Edwards SCS](https://github.com/alexedwards/scs/v2) for managing sessions
- [Chi router](https://github.com/go-chi/chi/v5)
- [Justinas nosurf](https://github.com/justinas/nosurf) for CSRF protection
- [JackC PGX](https://github.com/jackc/pgx/v5) as a pure Go driver and toolkit for PostgreSQL
- [Go Simple Mail](https://github.com/xhit/go-simple-mail) for sending emails
- [Simple DataTable](https://github.com/fiduswriter/Simple-DataTables) for tables
- [Buffalo Soda](https://gobuffalo.io/pt/documentation/database/soda/) for database migrations

### Note

- Create your own Go mod file by running `go mod init your-project-name`
- Replace all imports with your current Go mod name.
- Run `go mod tidy` - to install all the packages
- After making the necessary changes, run the app with `go run cmd/web/*.go`  and start the server.
- Create your PostgreSQL database.
- Set up the flags in `cmd/web/main.go` and `run.sh`.
- Rename `.env.example` to `.env` and set up your environment variables.
- Rename `database.yml.example` to `database.yml` to run `soda migrate`.

### Run the Server

- **Manual:** `go run cmd/web/main.go cmd/web/middleware.go cmd/web/routes.go cmd/web/sendMail.go`
- **Batch:**
  - **On Windows:** Create a `run.bat` file in the root directory of the project and add the following code:
    ```sh
    go build -o musiqcity cmd/web/*.go
    ./musiqcity.exe
    ```
    Then run `run.bat` in the terminal.
  - **On Linux:** Create a `run.sh` file in the root directory of the project and add the following code:
    ```sh
    #!/bin/bash
    go build -o musiqcity cmd/web/*.go
    ./musiqcity
    ```
    Run `chmod +x run.sh` then `./run.sh` in the terminal.

### Testing

- To output test in HTML format, run `go test -coverprofile=coverage.out && go tool cover -html=coverage.out`
- To know the percentage coverage, run `go test -cover`
- To run tests for the entire project, run `go test -v ./...`

### Soda Migration

- Install `soda` with `go install github.com/gobuffalo/pop/v6/soda@latest`
- Generate a database.yml file with `soda g config` for a PostgreSQL database.
- Create migration files with `soda generate fizz migration-name` or `soda generate sql migration-name`
- Run migrations with `soda migrate`
- Run down migrations with `soda migrate down`
- Reset the database with `soda reset`
- Read more about [Buffalo migrations](https://gobuffalo.io/documentation/database/migrations/) or the [Fizz GitHub page](https://github.com/gobuffalo/fizz)

### The main.go File

This is where we create and configure our session:

- `var session *scs.SessionManager` initializes the session variable.
- `session = scs.New()` creates a new session.
- `session.Lifetime = 24 * time.Hour` sets session duration.
- `session.Cookie.Persist = true` ensures session persistence.
- `session.Cookie.SameSite = http.SameSiteLaxMode` sets cookie security level.
- `session.Cookie.Secure = app.InProduction` ensures secure cookies in production.

### The handlers.go File

This is where we handle all our pages and templates:

- `func (m *Repository) Home(w http.ResponseWriter, r *http.Request)` handles the home page.
- `var Repo *Repository` creates a repository pattern for handlers.
- `remoteIP := r.RemoteAddr` retrieves the user's IP address.
- `m.App.Session.Put(r.Context(), "remote_ip", remoteIP)` stores the IP in the session.
- `getRemoteIP := m.App.Session.GetString(r.Context(), "remote_ip")` retrieves the IP from the session.
- `stringMap["remote_ip"] = getRemoteIP` makes the IP available in the template.

### The routes.go File

This is where we handle routes, multiplexers, and middleware:

- `mux := chi.NewRouter()` creates a multiplexer.
- `fileServer := http.FileServer(http.Dir("./static/"))` creates a file server for static files.
- `mux.Handle("/static/*", http.StripPrefix("/static", fileServer))` handles static files.
- `mux.Use(middleware.Recoverer)` adds middleware for panic control.

### The render.go File

This file handles template rendering:

- `template.FuncMap{}` defines functions for templates.
- `NewTemplates` sets the configuration for the template package.
- `AddDefaultData` adds default data to templates.

### The middleware.go File

This file defines custom middleware for the `chi` package:

- `func NoSurf(next http.Handler) http.Handler` adds CSRF protection to all POST requests.
- `func SessionLoad(next http.Handler) http.Handler` loads and saves session data.

### The config.go File

This file holds the application configuration:

- `AppConfig` struct holds configurations.
- `InfoLog` creates a log file for storing information.

### The templateData.go File

This file defines data structures for templates:

- `TemplateData` struct holds data sent from handlers to templates.
- `interface{}` is used for unknown types.
- `CSRFToken` is a security token for forms.

### The test file

- To output test in HTML format, run `go test -coverprofile=coverage.out && go tool cover -html=coverage.out`
- To know the percentage coverage, run `go test -cover`
- To run tests for the entire project, run `go test -v ./...`

### Contributors

- Prosper Atu
- Idongesit Ekanem
