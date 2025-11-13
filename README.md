[![CI-CD](https://github.com/vol1ura/Sat_9am_5km/actions/workflows/rubyonrails.yml/badge.svg)](https://github.com/vol1ura/Sat_9am_5km/actions/workflows/rubyonrails.yml)
[![CodeQL](https://github.com/vol1ura/Sat_9am_5km/actions/workflows/codeql.yml/badge.svg)](https://github.com/vol1ura/Sat_9am_5km/actions/workflows/codeql.yml)
[![Code Coverage](https://qlty.sh/gh/vol1ura/projects/Sat_9am_5km/coverage.svg)](https://qlty.sh/gh/vol1ura/projects/Sat_9am_5km)
[![Maintainability](https://qlty.sh/gh/vol1ura/projects/Sat_9am_5km/maintainability.svg)](https://qlty.sh/gh/vol1ura/projects/Sat_9am_5km)
[![License: GPLv3](https://img.shields.io/badge/License-GPLv3-blue.svg)](http://perso.crans.org/besson/LICENSE.html)
[![Contributions Welcome](https://img.shields.io/badge/Contributions-Welcome-brightgreen)](CONTRIBUTING.md)
[![Website Status](https://img.shields.io/website?down_color=red&down_message=failed&up_color=blue&up_message=online&url=https%3A%2F%2Fs95.ru)](https://s95.ru)

<img width="1094" height="319" alt="Group 1" src="https://github.com/user-attachments/assets/883edc82-8f24-477a-b4a2-fdcfb57d4268" />

# Sat 9am 5km - Run Events Management System

## Overview

**Sat 9am 5km** is a comprehensive web application for managing and organizing parkrun events in your local community. Built with Ruby on Rails, it provides tools for event management, user registration, results tracking, and community engagement.

### Key Features

-  **Event Management** - Create and manage local running events
-  **User Registration** - Streamlined registration system for participants
-  **Results Tracking** - Track and display accurate race results
-  **Performance Monitoring** - Real-time application and database performance metrics
-  **Admin Dashboard** - Comprehensive administrative control panel
-  **Email System** - Automated notifications and preview system
-  **REST APIs** - Programmatic access to event data
-  **Error Tracking** - Real-time bug monitoring via Rollbar
-  **File Management** - Centralized storage dashboard

---

## Table of Contents

- [Quick Start](#quick-start)
- [Development Setup](#development-setup)
- [Installation on Server](#server-setup)
- [Project Structure](#project-structure)
- [Useful Links](#useful-links)
- [Tech Stack](#tech-stack)
- [Maintenance](#maintenance)
- [Contributing](#contributing)
- [License](#license)

---

## Quick Start

### Prerequisites

Ensure you have the following installed:
- **Docker** and Docker Compose
- **Git**
- **Make** (usually pre-installed on Unix-like systems)

### Getting Started

1. **Clone the repository**
   ```bash
   git clone https://github.com/vol1ura/Sat_9am_5km.git
   cd Sat_9am_5km
   ```

2. **Prepare configuration files**
   ```bash
   cp ./deploy/.env.example ./deploy/.env
   cp ./config/database.yml.example ./config/database.yml
   ```

3. **Start the application**
   ```bash
   make build
   make ash  # Enter Docker container
   ```

4. **Initialize the database**
   ```bash
   rails db:prepare
   rake db:parse_parkrun_codes[kuzminki_db.csv]
   rails db:environment:set RAILS_ENV=test
   ```

5. **Configure credentials**
   ```bash
   rails credentials:edit --environment test
   ```

6. **Run tests to verify setup**
   ```bash
   rspec
   ```

---

## Development Setup

### Prerequisites

1. **Install system dependencies** (Ubuntu/Debian)
   ```bash
   sudo apt-get install -y libyaml-dev
   ```

2. **Install Rust** (required for Ruby YJIT compilation)
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

3. **Install rbenv** using [rbenv-installer](https://github.com/rbenv/rbenv-installer#rbenv-installer)
   ```bash
   curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-installer | bash
   ```

4. **Install Ruby 3.4.5**
   ```bash
   rbenv install 3.4.5
   rbenv global 3.4.5
   ```

### Running the Application

Once your environment is set up:

```bash
make
```

The application will start with development servers:
- **Rails** on port 3000
- **PostgreSQL** on port 3003

---

## Maintenance

### Reset PostgreSQL Statistics

Periodically reset query statistics for better performance analysis:

```sql
SELECT pg_stat_statements_reset();
```

Use this after:
- Deploying database-related changes
- Clearing out old performance baselines
- Starting fresh performance monitoring

---

## Server Setup

For production deployment on Ubuntu/Debian servers:

### Step 1: Install Dependencies

```bash
sudo apt-get install -y libyaml-dev
```

### Step 2: Install Rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```

### Step 3: Install rbenv and Ruby

```bash
# Install rbenv
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-installer | bash

# Initialize rbenv
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"

# Install Ruby
rbenv install 3.4.5
rbenv global 3.4.5
rbenv rehash
```

---

## Useful Links

After deployment, use these tools to manage and monitor your application:

| Resource | Development | Production | Notes |
|----------|-------------|------------|-------|
| **Main Application** | http://localhost:3000 | https://s95.ru | Main website |
| **Admin Panel** | http://localhost:3000/admin | https://s95.ru/admin | Administrative dashboard |
| **Email Preview** | http://localhost:3000/rails/mailers | N/A | Test email templates |
| **Database Manager** | http://localhost:3003 | N/A | PostgreSQL admin interface |
| **Background Jobs** | http://localhost:3000/sidekiq | https://s95.ru/sidekiq | Sidekiq job monitor |
| **Rails Perf** | http://localhost:3000/app_performance/ | https://s95.ru/app_performance/ | Performance metrics |
| **Database Stats** | http://localhost:3000/pg_stats | https://s95.ru/pg_stats | Query statistics |
| **File Storage** | http://localhost:3000/storage | https://s95.ru/storage | Active Storage dashboard |
| **Bug Tracker** | [Rollbar](https://app.rollbar.com/a/Urka/fix/items) | [Rollbar](https://app.rollbar.com/a/Urka/fix/items) | Error monitoring |
| **Uptime Monitor** | [UptimeRobot](https://dashboard.uptimerobot.com/monitors/797544445) | [UptimeRobot](https://dashboard.uptimerobot.com/monitors/797544445) | Server availability |

---

## Tech Stack

### Backend
- **Framework**: Ruby on Rails (v7+)
- **Language**: Ruby 3.4.5 (with YJIT enabled)
- **Database**: PostgreSQL
- **Job Queue**: Sidekiq with Redis
- **File Storage**: Active Storage (AWS S3 compatible)

### DevOps & Tools
- **Containerization**: Docker & Docker Compose
- **Build Automation**: Make
- **Testing**: RSpec
- **Code Analysis**: CodeQL, Qlty
- **Error Tracking**: Rollbar
- **Monitoring**: UptimeRobot

### External Services
- **Parkrun Integration**: CSV data imports
- **Email**: SMTP configuration (configured via credentials)

---

## Contributing

Contributions are welcome and appreciated! Here's how to contribute:

### Process

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Implement** your changes
4. **Test** your changes with `rspec`
5. **Commit** with a clear message (`git commit -m 'Add amazing feature'`)
6. **Push** to your branch (`git push origin feature/amazing-feature`)
7. **Open** a Pull Request

### Requirements

- All tests must pass: `rspec`
- Follow existing code style
- Add tests for new features
- Update documentation as needed

---

## Maintenance

### Common Tasks

**Run tests**
```bash
rspec
```

**Check database status**
```bash
rails db:migrate:status
```

**Clear cache**
```bash
rails cache:clear
```

---

## License

This project is licensed under the **GPLv3 License** - see the [LICENSE](http://perso.crans.org/besson/LICENSE.html) file for full details.

---

## Support & Resources

If you encounter issues:

1. **Check GitHub Issues**: [Open Issues](https://github.com/vol1ura/Sat_9am_5km/issues)
2. **Review Error Tracking**: [Rollbar Dashboard](https://app.rollbar.com/a/Urka/fix/items)
3. **Check Server Status**: [UptimeRobot Monitor](https://dashboard.uptimerobot.com/monitors/797544445)
4. **Documentation**: This README and inline code comments

---

