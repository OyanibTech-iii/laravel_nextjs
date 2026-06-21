# Laravel + Next.js Full-Stack Monorepo

[![Laravel](https://img.shields.io/badge/Laravel-v13.8-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)](https://laravel.com)
[![Next.js](https://img.shields.io/badge/Next.js-v16.2-000000?style=for-the-badge&logo=next.js&logoColor=white)](https://nextjs.org)
[![React](https://img.shields.io/badge/React-v19.2-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-v5-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![PHP](https://img.shields.io/badge/PHP-%3E%3D8.3-777BB4?style=for-the-badge&logo=php&logoColor=white)](https://www.php.net)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

A full-stack monorepo combining a **Laravel 13** API backend with a **Next.js 16** frontend client. The backend serves as a RESTful API layer with queue processing and database management, while the frontend delivers a server-rendered React application with TypeScript and Tailwind CSS.

---

## Table of Contents

- [Architecture Overview](#architecture-overview)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [Clone the Repository](#clone-the-repository)
  - [Backend Setup](#backend-setup)
  - [Frontend Setup](#frontend-setup)
- [Configuration](#configuration)
  - [Environment Variables](#environment-variables)
  - [Database](#database)
  - [Mail](#mail)
  - [Cache and Sessions](#cache-and-sessions)
  - [Queue](#queue)
- [Development](#development)
  - [Running the Backend](#running-the-backend)
  - [Running the Frontend](#running-the-frontend)
  - [Running Both Concurrently](#running-both-concurrently)
- [Testing](#testing)
  - [Backend Tests](#backend-tests)
  - [Frontend Linting](#frontend-linting)
- [Project Structure](#project-structure)
- [Available Scripts](#available-scripts)
  - [Backend Scripts](#backend-scripts)
  - [Frontend Scripts](#frontend-scripts)
- [Deployment](#deployment)
  - [Backend Deployment](#backend-deployment)
  - [Frontend Deployment](#frontend-deployment)
- [Contributing](#contributing)
- [License](#license)

---

## Architecture Overview

```
                    +---------------------+
                    |      Client         |
                    | (Browser / Mobile)  |
                    +----------+----------+
                               |
                    +----------v----------+
                    |   Next.js Frontend  |
                    |   (SSR + CSR)       |
                    |   Port 3000         |
                    +----------+----------+
                               |
                         HTTP / REST
                               |
                    +----------v----------+
                    |   Laravel Backend   |
                    |   (API Server)      |
                    |   Port 8000         |
                    +----------+----------+
                               |
              +----------------+----------------+
              |                |                |
     +--------v------+  +-----v------+  +------v-------+
     |    MySQL       |  |   Queue    |  |    Cache     |
     |   Database     |  |  Worker    |  |   (DB)       |
     +---------------+  +------------+  +--------------+
```

The project follows a decoupled architecture:

- **Frontend (`/frontend`)** -- A Next.js 16 application using the App Router, React 19, TypeScript, and Tailwind CSS v4. It handles all client-facing rendering via server-side rendering (SSR) and client-side hydration.
- **Backend (`/backend`)** -- A Laravel 13 application responsible for API endpoints, database operations, authentication, queue processing, and business logic. It uses Vite for its own asset pipeline.

---

## Technology Stack

### Backend

| Technology | Version | Purpose |
|---|---|---|
| PHP | >= 8.3 | Runtime language |
| Laravel | 13.8 | Web application framework |
| MySQL | 8.0+ | Primary database |
| Vite | 8.0 | Asset bundling |
| Tailwind CSS | 4.0 | Backend view styling |
| Pest | 4.7 | Testing framework |
| Laravel Pint | 1.27 | Code style fixer |
| Laravel Pail | 1.2.5 | Real-time log viewer |

### Frontend

| Technology | Version | Purpose |
|---|---|---|
| Next.js | 16.2 | React framework (App Router) |
| React | 19.2 | UI component library |
| TypeScript | 5.x | Type-safe JavaScript |
| Tailwind CSS | 4.x | Utility-first CSS framework |
| ESLint | 9.x | Code quality and linting |

---

## Prerequisites

Before setting up the project, ensure the following tools are installed on your system:

| Requirement | Minimum Version | Installation Guide |
|---|---|---|
| PHP | 8.3 | [php.net/downloads](https://www.php.net/downloads) |
| Composer | 2.x | [getcomposer.org](https://getcomposer.org) |
| Node.js | 18.x | [nodejs.org](https://nodejs.org) |
| npm | 9.x | Bundled with Node.js |
| MySQL | 8.0 | [dev.mysql.com](https://dev.mysql.com/downloads/) |

Required PHP extensions:

- `pdo_mysql`
- `mbstring`
- `openssl`
- `tokenizer`
- `xml`
- `ctype`
- `json`
- `bcmath`
- `fileinfo`

---

## Getting Started

### Clone the Repository

```bash
git clone https://github.com/OyanibTech-iii/laravel_nextjs.git
cd laravel_nextjs
```

### Backend Setup

```bash
# Navigate to the backend directory
cd backend

# Install PHP dependencies
composer install

# Copy environment configuration
cp .env.example .env

# Generate the application encryption key
php artisan key:generate

# Run database migrations
php artisan migrate

# Install Node.js dependencies (for Vite asset pipeline)
npm install

# Build frontend assets
npm run build
```

Alternatively, use the automated setup script:

```bash
cd backend
composer setup
```

This single command runs all of the above steps sequentially.

### Frontend Setup

```bash
# Navigate to the frontend directory
cd frontend

# Install Node.js dependencies
npm install
```

---

## Configuration

### Environment Variables

The backend uses a `.env` file for configuration. Copy `.env.example` to `.env` and adjust the values according to your environment. Key variables are documented below.

#### Application Settings

| Variable | Default | Description |
|---|---|---|
| `APP_NAME` | `Laravel` | Application display name |
| `APP_ENV` | `local` | Environment: `local`, `staging`, `production` |
| `APP_DEBUG` | `true` | Enable detailed error pages (disable in production) |
| `APP_URL` | `http://localhost` | Base URL of the backend application |
| `APP_KEY` | _(empty)_ | Encryption key (auto-generated via `key:generate`) |
| `APP_LOCALE` | `en` | Default application locale |
| `BCRYPT_ROUNDS` | `12` | Bcrypt hashing cost factor |

### Database

| Variable | Default | Description |
|---|---|---|
| `DB_CONNECTION` | `mysql` | Database driver |
| `DB_HOST` | `127.0.0.1` | Database server hostname |
| `DB_PORT` | `3306` | Database server port |
| `DB_DATABASE` | `backend` | Database name |
| `DB_USERNAME` | `root` | Database authentication user |
| `DB_PASSWORD` | _(empty)_ | Database authentication password |

Create the database before running migrations:

```sql
CREATE DATABASE backend CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Mail

| Variable | Default | Description |
|---|---|---|
| `MAIL_MAILER` | `log` | Mail driver (`smtp`, `mailgun`, `ses`, `log`) |
| `MAIL_HOST` | `127.0.0.1` | SMTP server hostname |
| `MAIL_PORT` | `2525` | SMTP server port |
| `MAIL_USERNAME` | `null` | SMTP authentication user |
| `MAIL_PASSWORD` | `null` | SMTP authentication password |
| `MAIL_FROM_ADDRESS` | `hello@example.com` | Default sender address |
| `MAIL_FROM_NAME` | `${APP_NAME}` | Default sender display name |

### Cache and Sessions

| Variable | Default | Description |
|---|---|---|
| `CACHE_STORE` | `database` | Cache backend (`database`, `redis`, `memcached`, `file`) |
| `SESSION_DRIVER` | `database` | Session storage driver |
| `SESSION_LIFETIME` | `120` | Session expiration in minutes |
| `SESSION_ENCRYPT` | `false` | Encrypt session data |

### Queue

| Variable | Default | Description |
|---|---|---|
| `QUEUE_CONNECTION` | `database` | Queue backend (`database`, `redis`, `sqs`, `sync`) |

---

## Development

### Running the Backend

Start the Laravel development server, queue worker, and Vite dev server simultaneously:

```bash
cd backend
composer dev
```

This launches three concurrent processes:

| Process | Command | Default URL |
|---|---|---|
| Laravel Server | `php artisan serve` | `http://localhost:8000` |
| Queue Worker | `php artisan queue:listen --tries=1` | Background process |
| Vite Dev Server | `npm run dev` | `http://localhost:5173` |

To run only the Laravel server:

```bash
cd backend
php artisan serve
```

### Running the Frontend

```bash
cd frontend
npm run dev
```

The Next.js development server starts at `http://localhost:3000` with hot module replacement enabled.

### Running Both Concurrently

Open two terminal windows/tabs:

**Terminal 1 -- Backend:**
```bash
cd backend
composer dev
```

**Terminal 2 -- Frontend:**
```bash
cd frontend
npm run dev
```

---

## Testing

### Backend Tests

The backend uses [Pest](https://pestphp.com) as its testing framework. Tests are organized into two suites:

- **Unit** (`tests/Unit/`) -- Isolated unit tests
- **Feature** (`tests/Feature/`) -- Integration and HTTP tests

```bash
cd backend

# Run the full test suite
composer test

# Run tests directly with Pest
php artisan test

# Run a specific test file
php artisan test --filter=ExampleTest

# Run only unit tests
php artisan test --testsuite=Unit

# Run only feature tests
php artisan test --testsuite=Feature

# Run tests with coverage report
php artisan test --coverage
```

The testing environment is pre-configured in `phpunit.xml` with the following overrides:

| Setting | Test Value | Purpose |
|---|---|---|
| `APP_ENV` | `testing` | Identifies the test environment |
| `DB_CONNECTION` | `sqlite` | Uses an in-memory SQLite database |
| `DB_DATABASE` | `:memory:` | No disk-based database required |
| `CACHE_STORE` | `array` | In-memory cache for speed |
| `QUEUE_CONNECTION` | `sync` | Synchronous queue execution |
| `SESSION_DRIVER` | `array` | In-memory session storage |
| `MAIL_MAILER` | `array` | Captures mail in memory |

### Frontend Linting

```bash
cd frontend

# Run ESLint
npm run lint
```

The frontend uses ESLint 9 with the `eslint-config-next` preset, which includes rules for Core Web Vitals and TypeScript.

---

## Project Structure

```
laravel_nextjs/
|
+-- backend/                     # Laravel 13 application
|   +-- app/
|   |   +-- Http/
|   |   |   +-- Controllers/     # HTTP request controllers
|   |   +-- Models/
|   |   |   +-- User.php         # Eloquent user model
|   |   +-- Providers/           # Service providers
|   +-- bootstrap/               # Framework bootstrapping
|   +-- config/                  # Application configuration files
|   |   +-- app.php              # Core application config
|   |   +-- auth.php             # Authentication guards and providers
|   |   +-- cache.php            # Cache store configuration
|   |   +-- database.php         # Database connection settings
|   |   +-- filesystems.php      # Filesystem disk configuration
|   |   +-- logging.php          # Log channel configuration
|   |   +-- mail.php             # Mail driver configuration
|   |   +-- queue.php            # Queue connection configuration
|   |   +-- services.php         # Third-party service credentials
|   |   +-- session.php          # Session driver configuration
|   +-- database/
|   |   +-- factories/           # Model factories for testing
|   |   +-- migrations/          # Database schema migrations
|   |   +-- seeders/             # Database seed classes
|   +-- public/                  # Web server document root
|   +-- resources/               # Views, CSS, and JavaScript source files
|   +-- routes/
|   |   +-- web.php              # Web routes
|   |   +-- console.php          # Artisan console commands
|   +-- storage/                 # Logs, cache, compiled views
|   +-- tests/
|   |   +-- Feature/             # Feature/integration tests
|   |   +-- Unit/                # Unit tests
|   |   +-- Pest.php             # Pest configuration
|   |   +-- TestCase.php         # Base test case class
|   +-- .env.example             # Environment variable template
|   +-- artisan                  # Laravel CLI entry point
|   +-- composer.json            # PHP dependency manifest
|   +-- package.json             # Node.js dependency manifest (Vite)
|   +-- phpunit.xml              # PHPUnit/Pest configuration
|   +-- vite.config.js           # Vite build configuration
|
+-- frontend/                    # Next.js 16 application
|   +-- app/
|   |   +-- layout.tsx           # Root layout component
|   |   +-- page.tsx             # Home page component
|   |   +-- globals.css          # Global stylesheet
|   |   +-- favicon.ico          # Site favicon
|   +-- public/                  # Static assets served as-is
|   +-- .next/                   # Next.js build output (git-ignored)
|   +-- eslint.config.mjs        # ESLint flat configuration
|   +-- next.config.ts           # Next.js configuration
|   +-- package.json             # Node.js dependency manifest
|   +-- postcss.config.mjs       # PostCSS configuration (Tailwind)
|   +-- tsconfig.json            # TypeScript compiler options
|
+-- README.md                    # This file
```

---

## Available Scripts

### Backend Scripts

All backend scripts are executed from the `backend/` directory.

| Command | Description |
|---|---|
| `composer setup` | Full automated setup: install dependencies, generate key, run migrations, build assets |
| `composer dev` | Start the development environment (server + queue + Vite concurrently) |
| `composer test` | Clear config cache and run the full Pest test suite |
| `composer install` | Install PHP dependencies from `composer.lock` |
| `npm run dev` | Start the Vite development server with HMR |
| `npm run build` | Build production assets via Vite |
| `php artisan serve` | Start the Laravel development server on port 8000 |
| `php artisan migrate` | Run pending database migrations |
| `php artisan migrate:rollback` | Roll back the last batch of migrations |
| `php artisan migrate:fresh` | Drop all tables and re-run all migrations |
| `php artisan db:seed` | Run database seeders |
| `php artisan queue:listen` | Start the queue worker (restarts on code changes) |
| `php artisan tinker` | Open an interactive REPL for the application |
| `php artisan key:generate` | Generate a new application encryption key |
| `php artisan config:clear` | Clear the configuration cache |
| `php artisan cache:clear` | Flush the application cache |

### Frontend Scripts

All frontend scripts are executed from the `frontend/` directory.

| Command | Description |
|---|---|
| `npm run dev` | Start the Next.js development server on port 3000 with HMR |
| `npm run build` | Create an optimized production build |
| `npm run start` | Serve the production build locally |
| `npm run lint` | Run ESLint against the codebase |

---

## Deployment

### Backend Deployment

1. **Set environment variables** for your production server. At minimum, configure:
   - `APP_ENV=production`
   - `APP_DEBUG=false`
   - `APP_URL` (your production domain)
   - `DB_*` variables for your production database
   - `MAIL_*` variables for your mail provider

2. **Install dependencies** (without dev packages):
   ```bash
   composer install --optimize-autoloader --no-dev
   ```

3. **Run migrations**:
   ```bash
   php artisan migrate --force
   ```

4. **Build assets**:
   ```bash
   npm run build
   ```

5. **Optimize the application**:
   ```bash
   php artisan config:cache
   php artisan route:cache
   php artisan view:cache
   php artisan event:cache
   ```

6. **Configure the web server** (Nginx or Apache) to point the document root to `backend/public/`.

7. **Start the queue worker** as a supervised process:
   ```bash
   php artisan queue:work --tries=3 --timeout=90
   ```

### Frontend Deployment

1. **Build the production bundle**:
   ```bash
   cd frontend
   npm run build
   ```

2. **Start the production server**:
   ```bash
   npm run start
   ```

   Alternatively, deploy to a platform with native Next.js support:
   - [Vercel](https://vercel.com) (recommended)
   - [Netlify](https://netlify.com)
   - [AWS Amplify](https://aws.amazon.com/amplify/)
   - Self-hosted via Docker or Node.js process manager (e.g., PM2)

3. **Environment configuration**: Set the API base URL in the frontend to point to your production backend.

---

## Contributing

1. Fork the repository.
2. Create a feature branch from `main`:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. Make your changes and ensure all tests pass:
   ```bash
   cd backend && composer test
   cd ../frontend && npm run lint
   ```
4. Commit your changes with a descriptive message:
   ```bash
   git commit -m "Add: description of the feature"
   ```
5. Push to your fork and open a Pull Request against the `main` branch.

### Code Style

- **Backend**: Run `./vendor/bin/pint` to automatically fix PHP code style issues according to Laravel conventions.
- **Frontend**: Run `npm run lint` to check for ESLint violations.

---

## License

This project is open-sourced software licensed under the [MIT License](https://opensource.org/licenses/MIT).
