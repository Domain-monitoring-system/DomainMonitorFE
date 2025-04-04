# Domain Monitor Frontend

A web-based interface for the Domain Monitoring System that allows users to track domain status and SSL certificate expiration.

## Project Overview

The Domain Monitor Frontend is a web application that provides a user-friendly interface for the Domain Monitoring System. Users can register, log in, add domains to monitor, view domain status, and configure scheduled checks.

## Features

- **User Authentication**: Registration, login, and Google OAuth integration
- **Domain Management**: Add, view, and remove domains
- **Status Dashboard**: Visual display of domain status and SSL certificate information
- **Scheduled Monitoring**: Configure hourly or daily domain checks
- **Responsive Design**: Works on desktop and mobile devices

## Project Structure

```
domain-monitor-frontend/
├── static/                     # Static files
│   ├── dashboard-style.css     # Dashboard styles
│   ├── style.css               # Login/registration styles
│   ├── script.js               # Login/registration scripts
│   ├── registration_script.js  # Registration validation
│   ├── dashbored_script.js     # Dashboard functionality
│   └── favicon.ico             # Website favicon
├── templates/                  # HTML templates
│   ├── index.html              # Login page
│   ├── registration.html       # Registration page
│   └── dashboard.html          # Main dashboard
├── __pycache__/                # Python cache files (not tracked in git)
├── app.py                      # Main Flask application
├── config.py                   # Configuration and environment variables
├── Dockerfile                  # Docker configuration
├── requirements.txt            # Python dependencies
├── .env                        # Environment variables (not tracked in git)
└── README.md                   # Project documentation
```

## Setup and Installation

### Prerequisites

- Python 3.9+
- Flask and required packages
- Backend API service running

### Local Development Setup

1. Clone the repository
   ```bash
   git clone https://github.com/RazielRey/domain-monitor-frontend.git
   cd domain-monitor-frontend
   ```

2. Create and activate a virtual environment
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies
   ```bash
   pip install -r requirements.txt
   ```

4. Set up environment variables
   ```bash
   cp .env.example .env
   # Edit .env with your configuration, especially BACKEND_URL
   ```

5. Run the application
   ```bash
   python app.py
   ```

6. Open your browser and navigate to `http://localhost:8080`

### Docker Setup

1. Build the Docker image
   ```bash
   docker build -t domain-monitor-frontend .
   ```

2. Run the container
   ```bash
   docker run -p 8080:8080 -e BACKEND_URL=http://backend-service:5001 domain-monitor-frontend
   ```

## Configuration

The application can be configured through environment variables when running in Docker:

- `BACKEND_URL`: URL of the backend API (default: `http://localhost:5001`)

## User Interface

### Login Page
- User login with username and password
- Registration link
- Google OAuth login option

### Registration Page
- User registration form with validation
- Back button to return to login

### Dashboard
- Top navigation with user profile and logout button
- Add Domain section with single domain input and bulk upload
- Schedule configuration for automated checks
- Domain table with status, SSL information, and action buttons

## API Endpoints and Backend Integration

The frontend application interacts with the backend through the following REST API endpoints:

### Authentication Endpoints
- `POST /api/login`: Authenticate user with username and password
  ```json
  {
    "username": "user@example.com",
    "password": "userpassword"
  }
  ```

- `POST /api/register`: Register a new user
  ```json
  {
    "username": "newuser@example.com",
    "password": "newpassword"
  }
  ```

- `GET /api/check-username?username=user@example.com`: Check if username is available
  - Returns: `{"available": true/false}`

- `POST /api/google-login`: Register/login user with Google credentials
  ```json
  {
    "username": "googleuser@gmail.com",
    "password": "googletoken",
    "is_google_user": true
  }
  ```

### Domain Management Endpoints
- `GET /api/domains?username=user@example.com`: Get user's domains
  - Returns: Array of domain objects with status information

- `POST /api/check-domains`: Check status of provided domains
  ```json
  {
    "domains": ["example.com", "google.com"],
    "username": "user@example.com"
  }
  ```

- `DELETE /api/domains?username=user@example.com&domain=example.com`: Remove a domain
  - Returns: Success/failure message

### Scheduler Endpoints
- `POST /api/schedule/hourly`: Configure hourly domain checks
  ```json
  {
    "username": "user@example.com",
    "interval": 2
  }
  ```

- `POST /api/schedule/daily`: Configure daily domain checks
  ```json
  {
    "username": "user@example.com",
    "time": "08:00"
  }
  ```

- `GET /api/schedule/status?username=user@example.com`: Get current schedule status
  - Returns: Schedule configuration and next run time

- `POST /api/schedule/stop`: Stop scheduled domain checks
  ```json
  {
    "username": "user@example.com"
  }
  ```

### Utility Endpoints
- `GET /health`: Backend health check endpoint
  - Returns: `{"status": "healthy"}`

The frontend manages user sessions and handles API authentication, making requests to the backend service configured in the `BACKEND_URL` environment variable.

## Browser Compatibility

The application is compatible with:
- Google Chrome (latest 2 versions)
- Mozilla Firefox (latest 2 versions)
- Microsoft Edge (latest 2 versions)
- Safari (latest 2 versions)

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -am 'Add my feature'`
4. Push to the branch: `git push origin feature/my-feature`
5. Submit a pull request

## License

[MIT License](LICENSE)