# SkillSwap

<div align="center">

![SkillSwap Logo](./core/static/imgs/sadcat.gif)

**A collaborative platform for exchanging skills and knowledge**

[Features](#features) • [Tech Stack](#tech-stack) • [Installation](#installation) • [Workflow](#workflow) • [License](#license)

</div>

---

## 📋 Table of Contents

- [About](#about)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Workflow](#workflow)
- [Contributing](#contributing)
- [Team](#team)
- [License](#license)

---

## 🎯 About

**SkillSwap** is a web application that allows users to exchange skills and knowledge with other members of the community. Whether you want to learn a new skill or teach something you know, SkillSwap connects people with shared interests and complementary abilities.

The platform enables users to:
- Create posts offering or requesting specific skills
- Search and filter available skills and users
- Manage their user profile with preferences and interests
- Maintain persistent search filters and preferences across sessions
- Enjoy a seamless experience in both light and dark themes

This project was developed as part of an educational course focusing on professional web development practices including version control, collaborative workflows, and full-stack development with Django.

---

## ✨ Features

### Core Features
- 🔐 **User Authentication**: Secure registration and login system
- 💬 **Skill Exchange**: Create posts to offer or request skills
- 🔍 **Advanced Search & Filtering**: Search by skill type, category, and status with persistent filters
- 👤 **User Profiles**: Customizable user profiles with timezone support
- 🌍 **Internationalization**: Multi-language support (Spanish/English)
- 🎨 **Theme Support**: Light and dark mode with user preference cookies
- 📱 **Responsive Design**: Mobile-friendly interface using Bootstrap
- 🛡️ **Anti-Spam Protection**: Middleware to prevent spam attacks
- 💾 **Session Management**: Persistent filters and preferences

### Technical Features
- Docker containerization for consistent deployment
- PostgreSQL database with migrations
- Gunicorn WSGI server
- Static file management with WhiteNoise
- Cloudflare Tunnel integration for external access

---

## 🛠️ Tech Stack

### Backend
- **Framework**: Django 6.0.2
- **Language**: Python 3.14
- **Database**: PostgreSQL 16
- **Server**: Gunicorn
- **Static Files**: WhiteNoise 6.7.0

### Frontend
- **HTML5** & **CSS3**
- **Bootstrap 5** (responsive components)
- **JavaScript** (vanilla, for theme/language switching)

### DevOps
- **Containerization**: Docker & Docker Compose
- **Tunneling**: Cloudflare Tunnel
- **Package Management**: pip, requirements.txt

### Development Tools
- **Version Control**: Git & GitHub
- **Email Validation**: disposable-email-domains

---

## 📦 Installation

### Prerequisites
Before you begin, ensure you have installed:
- **Docker** and **Docker Compose** (recommended for production)
- **Python 3.14+**
- **PostgreSQL 16+** (if running locally without Docker)
- **Git**

### 1. Clone the Repository

```bash
# Clone from your fork
git clone https://github.com/amahcan562-ies/skillswap-django.git
cd skillswap-django

# Add upstream remote (original repository)
git remote add upstream https://github.com/jherhum1702/skillswap-django.git
```

### 2. Create Environment File

Copy the example environment file and configure it:

```bash
cp .env.example .env
```

Edit `.env` with your configuration:

```dotenv
# Database Configuration
DB_NAME=skillswap_db
DB_USER=django_user
DB_PASSWORD=your_secure_password
DB_HOST=localhost
DB_PORT=5432

# Django Settings
SECRET_KEY=your_django_secret_key_here
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1
CSRF_TRUSTED_ORIGINS=http://localhost:8000,http://127.0.0.1:8000

# Cloudflare Tunnel (optional, for production)
TUNNEL_TOKEN=your_tunnel_token_here
```

**Generate a Django SECRET_KEY** if you don't have one:

```bash
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

---

## 🐳 Setup with Docker (Recommended)

### Quick Start

```bash
# Start all services
docker compose up -d

# Stop services
docker compose down

# View logs
docker compose logs -f web
```

The application will be available at `http://localhost:8000`

The Docker setup automatically:
- Creates the PostgreSQL database
- Runs migrations
- Populates test data
- Collects static files
- Starts the Gunicorn server

### Database Commands

```bash
# Access database shell inside Docker
docker compose exec db psql -U django_user -d skillswap_db

# View Django logs
docker compose logs web -f
```

---

## 🖥️ Local Development Setup

### 1. Create Virtual Environment

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure PostgreSQL Locally

```bash
# On Linux/Mac
sudo -u postgres psql

# Create database
CREATE DATABASE skillswap_db;
CREATE USER django_user WITH PASSWORD 'your_password';
ALTER ROLE django_user SET client_encoding TO 'utf8';
ALTER ROLE django_user SET default_transaction_isolation TO 'read committed';
ALTER ROLE django_user SET default_transaction_deferrable TO on;
ALTER ROLE django_user SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE skillswap_db TO django_user;
\q
```

### 4. Run Migrations

```bash
python manage.py migrate
```

### 5. Load Test Data (Optional)

```bash
python manage.py populate_test_data
```

### 6. Create Superuser

```bash
python manage.py createsuperuser
```

### 7. Collect Static Files

```bash
python manage.py collectstatic --noinput
```

### 8. Run Development Server

```bash
python manage.py runserver 0.0.0.0:8000
```

Access the application at `http://localhost:8000`

---

## 📁 Project Structure

```
skillswap-django/
├── core/                          # Main Django app
│   ├── management/commands/       # Custom management commands
│   │   └── populate_test_data.py # Load initial test data
│   ├── middleware/               # Custom middleware
│   │   └── anti_spam.py         # SPAM protection middleware
│   ├── migrations/               # Database migrations
│   ├── static/                   # App-level static files
│   │   └── imgs/
│   ├── templates/                # App-level templates
│   │   └── core/
│   │       ├── home.html        # Home page
│   │       ├── post_list.html   # Posts listing
│   │       ├── post_detail.html # Post details
│   │       ├── profile.html     # User profile
│   │       └── search_results.html # Search results
│   ├── admin.py                 # Django admin configuration
│   ├── apps.py                  # App configuration
│   ├── context_processors.py    # Template context processors (cookies, preferences)
│   ├── forms.py                 # Django forms
│   ├── models.py                # Database models
│   ├── session_manager.py       # Session management for filters
│   ├── urls.py                  # App URL routing
│   └── views.py                 # View logic
│
├── skillswap/                    # Django project settings
│   ├── settings.py              # Project settings
│   ├── urls.py                  # Project URL routing
│   ├── asgi.py                  # ASGI configuration
│   └── wsgi.py                  # WSGI configuration
│
├── templates/                    # Project-level templates
│   ├── base.html               # Base template with theme support
│   └── registration/           # Authentication templates
│       ├── login.html
│       └── registration.html
│
├── staticfiles/                  # Collected static files (production)
│   └── admin/                   # Django admin static files
│
├── Dockerfile                    # Docker image definition
├── docker-compose.yml           # Docker Compose configuration
├── manage.py                    # Django management script
├── requirements.txt             # Python dependencies
├── gunicorn.ctl                # Gunicorn control script
└── README.md                    # This file
```

---

## ⚙️ Configuration

### Context Processors
The `core/context_processors.py` provides global template context:
- **theme**: Current theme preference (light/dark) from cookies
- **lang**: Current language preference (ES/EN) from cookies

### Session Management
The `core/session_manager.py` handles:
- Saving and retrieving search filters from Django sessions
- Clearing filters when navigating away from search
- Persistent search state across browser sessions

### Middleware
- **SpamMiddleware**: Protects against spam attacks by rate limiting requests
- **WhiteNoiseMiddleware**: Serves static files efficiently
- **SessionMiddleware**: Manages user sessions

---

## 🚀 Usage

### Default Credentials (Test Data)

After running `populate_test_data`:
- **Username**: testuser
- **Password**: testpass123

### Features Guide

#### 1. **Search and Filter**
- Use the search bar to find skills or users
- Filters persist in your session automatically
- Use the "Clear Filters" button to reset

#### 2. **Theme Switching**
- Click the theme toggle (☀️/🌙) in the navigation bar
- Your preference is saved as a cookie
- Preference persists across sessions

#### 3. **Language Selection**
- Click the language toggle (EN/ES) in the navigation bar
- Changes interface language dynamically
- Preference is saved as a cookie

#### 4. **Create a Post**
- Log in to your account
- Navigate to create a post
- Choose "BUSCO" (I'm looking for) or "OFREZCO" (I'm offering)
- Select a skill and provide details
- Your post will be visible to other users

#### 5. **User Profile**
- Visit your profile to view and edit information
- Set your timezone
- Manage your skills and interests

---

## 📊 Workflow

### Git Strategy: Git Flow with GitHub

Our team follows a **modified Git Flow** model combined with **GitHub's collaborative features** for professional coordination:

#### Branch Structure

```
main (production)
└── develop (staging)
    ├── feature/feature-name
    ├── bugfix/bug-name
    └── hotfix/critical-issue
```

**Branch Naming Conventions:**
- `feature/description` - New features
- `bugfix/description` - Bug fixes
- `hotfix/critical-issue` - Critical production fixes
- Examples: `feature/cookies`, `bugfix/docker-expose-port`, `feature/sessions`

#### Workflow Steps

##### 1. **Create a Feature/Bugfix Branch**

```bash
# Update develop branch
git checkout develop
git pull upstream develop

# Create feature branch from develop
git checkout -b feature/your-feature-name

# Or from your fork
git checkout -b feature/your-feature-name origin/develop
```

##### 2. **Develop and Commit**

```bash
# Make your changes
git add .
git commit -m "feat: description of your changes"

# Follow conventional commits:
# feat: new feature
# fix: bug fix
# docs: documentation
# style: formatting
# refactor: code restructuring
# test: adding tests
```

##### 3. **Push to Your Fork**

```bash
# Push to your fork
git push origin feature/your-feature-name
```

##### 4. **Create a Pull Request**

- Go to **GitHub repository** → **Pull Requests** → **New PR**
- **Base**: `jherhum1702/skillswap-django` `develop`
- **Compare**: `your-fork/skillswap-django` `feature/your-feature-name`
- **Fill PR template** with:
  - Description of changes
  - Key changes (bullet points)
  - Testing checklist
  - Related issues
  - Screenshots if applicable

**PR Template:**
```markdown
## Description
Brief explanation of what this PR does

## Key changes
- Changed X
- Added Y
- Fixed Z

## Testing checklist
- [ ] Migrations apply without errors
- [ ] Feature works as expected
- [ ] No breaking changes

## Related issues
Closes #(ISSUE NUMBER)
```

##### 5. **Code Review**

- Team members review the PR
- Discuss changes in PR comments
- Request changes if needed
- Approve when ready

**Code Review Focus:**
- Django best practices
- Database migrations validity
- Template safety
- No hard-coded values
- Meaningful commit messages

##### 6. **Merge to Develop**

```bash
# After approval, merge is done through GitHub
# Usually "Squash and merge" or "Create a merge commit"
```

##### 7. **Sync Your Fork**

```bash
# After merge, sync your fork
git checkout develop
git pull upstream develop
git push origin develop
```

##### 8. **Delete Feature Branch**

```bash
# Delete local branch
git branch -d feature/your-feature-name

# Delete remote branch
git push origin --delete feature/your-feature-name
```

#### Issues Management

**How we use GitHub Issues:**

1. **Feature Requests**: Use "Feature request" template
   - Describe the desired functionality
   - Priority: Low/Medium/High
   - Assigned to team member

2. **Bug Reports**: Use "Bug report" template
   - Problem statement
   - Steps to reproduce
   - Expected vs actual behavior
   - Assign to team member

3. **Labeling System**:
   - `enhancement` - New features
   - `bug` - Bug fixes needed
   - `documentation` - Docs improvements
   - `high`, `medium`, `low` - Priority levels
   - `in-progress` - Currently being worked on
   - `review` - Waiting for code review

**Workflow Example:**
```
Issue #45: "Add dark mode support"
  ↓
Create branch: feature/cookies
  ↓
Implement feature
  ↓
Create PR linked to issue
  ↓
Code review
  ↓
Merge to develop
  ↓
Close issue
```

#### Conflict Resolution

**When conflicts occur:**

```bash
# Update your branch with latest develop
git fetch upstream
git rebase upstream/develop

# Resolve conflicts manually in your editor
# Look for: <<<<<<, ======, >>>>>> markers

# After resolving
git add .
git rebase --continue
git push origin feature/your-feature-name -f
```

**Conflict Prevention:**
- Pull latest changes before starting work
- Keep branches focused and short-lived
- Communicate with team about large changes
- Rebase frequently with develop

#### Communication

- **Pull Request Comments**: Discuss specific changes
- **Issues**: Track bugs and features
- **GitHub Discussions**: General questions (if enabled)
- **In-person**: Complex decisions or blockers

---

## 👥 Team

This project was developed collaboratively by:
- **jherhum1702** - Project Lead & Backend
- **amahcan562-ies** - Full Stack (Cookies, Sessions, Filters)
- **fdomcas** - Frontend & Features (Profile, Posts)
- **Other Contributors** - See commit history

---

## 📝 License

This project is licensed under the **MIT License** - see below for details.

```
MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```

---

## 🤝 Contributing

We welcome contributions! Here's how to get started:

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/AmazingFeature`)
3. **Commit changes** (`git commit -m 'feat: Add AmazingFeature'`)
4. **Push to branch** (`git push origin feature/AmazingFeature`)
5. **Open a Pull Request** with detailed description
6. **Wait for review and approval**

Please ensure your code follows our conventions and includes appropriate documentation.

---

## 📚 Additional Resources

- [Django Documentation](https://docs.djangoproject.com/en/6.0/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [Git Workflow Guide](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows)
- [Conventional Commits](https://www.conventionalcommits.org/)

---

## ❓ FAQ

**Q: How do I run the project locally without Docker?**
A: Follow the "Local Development Setup" section above. You'll need PostgreSQL installed locally.

**Q: How do I reset the database?**
A: 
```bash
# With Docker
docker compose down -v  # -v removes volumes
docker compose up -d

# Locally
python manage.py migrate zero  # Reverse all migrations
python manage.py migrate       # Re-apply migrations
```

**Q: How do I add a new dependency?**
A: 
```bash
pip install new-package
pip freeze > requirements.txt
git add requirements.txt
git commit -m "deps: add new-package"
```

**Q: How do I run Django commands in Docker?**
A:
```bash
docker compose exec web python manage.py [command]
```

---

**Last Updated**: February 27, 2026
**Status**: In Active Development 🚀

