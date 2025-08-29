# ALX Travel App

A robust travel listing platform built with Django and Django REST Framework. The application allows users to browse and book travel accommodations, leave reviews, and manage listings as hosts.

## ✨ Features

- 🔌 **RESTful API** with Django REST Framework
- 🗄️ **MySQL Database** for reliable, scalable data storage
- 📄 **Swagger/OpenAPI** documentation for interactive API exploration
- 🌐 **CORS Support** for frontend-backend communication
- 🔐 **Environment Variables** for secure configuration
- ⚙️ **Celery Integration** for asynchronous background tasks (e.g., emails, cleanup)
- 📧 **Email Notifications** for booking confirmations and payment processing
- 💳 **Payment Integration** with Chapa payment gateway
- 🔔 **Automated Email System** for user engagement and booking management

## 📧 Email Notification System

The application includes a comprehensive email notification system powered by Celery for asynchronous processing:

- **Booking Confirmation Emails**: Automatically sent when users create new bookings
- **Payment Reminder Emails**: Sent with checkout links for pending payments
- **Payment Confirmation Emails**: Delivered upon successful payment completion
- **Asynchronous Processing**: All emails are handled via Celery tasks to ensure optimal performance

### Email Tasks

- `send_booking_confirmation_email()` - Triggered on booking creation
- `send_payment_email()` - Sent with payment checkout URLs
- `send_payment_confirmation()` - Confirmation of successful payments

## Prerequisites

- Python 3.10+
- MySQL 5.7+
- RabbitMQ (for Celery tasks)

## Installation

1. **Clone the repository**
```bash
git clone https://github.com/Loutimi/alx_travel_app_0x00.git
cd alx_travel_app_0x00
```

2. **Set up virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Configure environment variables**

- Create a .env file in the project root
- Add your configuration (see example below)

```bash
# Example .env file:
DEBUG=True
SECRET_KEY=your-secret-key-here
DB_NAME=alx_travel_db
DB_USER=root
DB_PASSWORD=yourpassword
DB_HOST=localhost
DB_PORT=3306

# Celery/RabbitMQ Configuration
RABBITMQ_USER=guest
RABBITMQ_PASS=guest
RABBITMQ_HOST=localhost
RABBITMQ_PORT=5672

# Payment Gateway
CHAPA_SECRET_KEY=your-chapa-secret-key
CHAPA_BASE_URL=https://api.chapa.co/v1

# Email Configuration (Development)
EMAIL_BACKEND=django.core.mail.backends.console.EmailBackend
DEFAULT_FROM_EMAIL=no-reply@airbnbclone.com
```

5. **Set up database**
   
- Create a MySQL database
- Update the .env file with your database credentials

6. **Run migrations**
```bash
python manage.py makemigrations
python manage.py migrate
```

## Running the Application

### 1. Start the Django development server
```bash
python manage.py runserver
```

### 2. Start Celery worker (in a separate terminal)
```bash
celery -A alx_travel_app worker -l info
```

### 3. (Optional) Start Celery Beat for scheduled tasks
```bash
celery -A alx_travel_app beat -l info
```

## 🚀 API Endpoints

### Core Endpoints
- `GET/POST /api/listings/` - Browse and create listings
- `GET/POST /api/bookings/` - Manage bookings
- `GET/POST /api/reviews/` - User reviews and ratings

### Payment Endpoints
- `POST /api/payments/initiate/<booking_id>/` - Initialize payment
- `GET /api/payments/verify/<tx_ref>/` - Verify payment status
- `GET /api/payments/success/` - Payment success page

### Documentation
- `GET /api/swagger/` - Interactive API documentation
- `GET /api/redoc/` - Alternative API documentation

## Project Structure

```bash
alx_travel_app/
├── alx_travel_app/            # Project settings and configurations
│   ├── settings.py            # Django settings with email & Celery config
│   ├── urls.py                # Root URL configuration
│   ├── celery.py              # Celery configuration
│   └── ...
├── listings/                  # Travel listing and booking app
│   ├── migrations/            # Database migrations
│   ├── models.py              # Models: Listing, Booking, Review, Payment
│   ├── views.py               # API views with email triggers
│   ├── serializers.py         # DRF serializers
│   ├── tasks.py               # Celery email tasks
│   └── urls.py                # App URL routing
├── users/                     # Custom User model and related logic
│   └── ...
├── .env                       # Environment configuration
├── requirements.txt           # Python dependencies
└── manage.py                  # Django CLI
```

## 🔧 Email Configuration

The application supports multiple email backends:

### Development (Console)
```python
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
```

### Production (SMTP)
```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'your-smtp-server.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'your-email@domain.com'
EMAIL_HOST_PASSWORD = 'your-email-password'
```

## 🧪 Testing Email Tasks

To test the email functionality:

1. Create a booking via the API
2. Check your console (development) or email inbox (production)
3. Monitor Celery worker logs for task execution

## 📝 Notes

- Emails are processed asynchronously to maintain API performance
- All email tasks include error handling and retry mechanisms
- Payment emails include secure checkout URLs with transaction references
- Booking confirmation emails are triggered immediately upon booking creation

## 📄 License

This project is licensed under the MIT License.
