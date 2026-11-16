# MedicaMarket

A medical e-commerce platform where individual clients and clinics can buy pharmaceutical products online, with stock and expiry-date management, simulated OTP payment, PDF invoices, role-based dashboards and AI features.

> Academic team project (2026).

## Team

| Developer | Branch | Scope |
|---|---|---|
| Eya ([@Eyablg-ops](https://github.com/Eyablg-ops)) | `dev1/auth-users` | Authentication, users, roles, permissions |
| Maryem Bourourou | `dev2/ecommerce` | E-commerce: products, cart, orders, payment |
| Asma Letaief | `dev3/ai` | AI, chatbot, recommendations |

The original team repository is [Eyablg-ops/medicamarket](https://github.com/Eyablg-ops/medicamarket).

## Features

**E-commerce**
- Product catalogue with categories, filters and search
- Expiry dates with visual alerts (expiring soon / expired)
- Stock management with automatic decrement
- Shopping cart (add, change quantity, remove)
- Checkout with address and a simulated 3-step OTP payment
- Order history with statuses and PDF invoice download
- Order confirmation emails (tested with Mailtrap)

**Dashboards**
- *Client*: order summary, total spent, purchase history, recommendations
- *Clinic*: product and order statistics, top-selling products, stock and expiry alerts, product and category management with image upload
- *Admin*: KPIs, monthly sales chart, user list with search, top products

**Roles**

| Role | Access |
|---|---|
| `client` | Shop, cart, orders, client dashboard |
| `clinique` | Everything a client can do, plus clinic dashboard and product / category management |
| `admin` | Everything, plus admin dashboard and the Django admin interface |

## Tech stack

| Layer | Technologies |
|---|---|
| Backend | Python 3.11+, Django 4.2.7, Django REST Framework 3.14.0, SimpleJWT 5.3.0, django-cors-headers, django-filter, Pillow, ReportLab |
| Frontend | React 18, Vite, Tailwind CSS 3.4.0, React Router, Axios, Recharts |
| Database | MySQL / MariaDB 10.4 (via XAMPP) |
| AI | Ollama (configurable provider and model) |

## Project structure

```
.
├── config/          # Django settings
├── users/           # Authentication, roles, clinic profile
├── products/        # Products, categories, stock, expiry dates
├── orders/          # Cart, orders, confirmation emails, admin statistics
├── payments/        # Simulated OTP payment, PDF invoices
├── ai_features/     # AI features
├── media/           # Uploaded and seed images
├── frontend/        # React application (Vite)
└── manage.py
```

## Getting started

**Prerequisites:** Python 3.11+, Node.js 18+, XAMPP with MySQL running.

1. **Clone the project**
   ```bash
   git clone https://github.com/MaryemBourourou/medicamarket.git
   cd medicamarket
   ```
2. **Database.** Start Apache and MySQL in XAMPP, open `http://localhost/phpmyadmin` and create a database named `medicamarket_db`.
3. **Backend**
   ```bash
   python -m venv venv
   venv\Scripts\activate
   pip install django==4.2.7 djangorestframework==3.14.0 djangorestframework-simplejwt==5.3.0 django-cors-headers==4.3.0 django-filter==23.3 mysqlclient Pillow reportlab python-dotenv requests
   python manage.py migrate
   python manage.py createsuperuser
   python manage.py runserver
   ```
4. **Frontend**
   ```bash
   cd frontend
   npm install
   npm run dev
   ```
   The app is available at `http://localhost:5173` and the API at `http://localhost:8000`.

## Configuration

Secrets must not be committed. Create a `.env` file next to `manage.py` (see `.env.example`):

```
SECRET_KEY=your-own-secret-key
EMAIL_HOST_USER=your_mailtrap_username
EMAIL_HOST_PASSWORD=your_mailtrap_password
AI_PROVIDER=ollama
OLLAMA_BASE_URL=http://localhost:11434/api
OLLAMA_MODEL=llama3:8b
AI_FALLBACK_ENABLED=true
EXPIRY_ALERT_DAYS=30
```

Order confirmation emails use [Mailtrap](https://mailtrap.io) (optional). The AI features need [Ollama](https://ollama.com) running locally.

## API overview

| Endpoint | Description |
|---|---|
| `/api/users/` | Authentication and users |
| `/api/products/` | Products |
| `/api/products/categories/` | Categories |
| `/api/products/clinic/products/` | Clinic product management |
| `/api/products/clinic/categories/` | Clinic category management |
| `/api/orders/cart/` | Cart |
| `/api/orders/checkout/` | Create an order |
| `/api/orders/` | Order list |
| `/api/orders/admin/stats/` | Admin statistics |
| `/api/payments/initiate/` | Start an OTP payment |
| `/api/payments/verify-otp/` | Verify the OTP |
| `/api/payments/invoice/:id/` | Download a PDF invoice |

## Known constraints

- Stay on Django 4.2.7: newer versions are not compatible with MariaDB 10.4.
- Stay on Tailwind CSS 3.4.0 (not v4).
- Remove `import pymysql` from `settings.py` if present: it conflicts with `mysqlclient`.
- Stripe is not available in Tunisia, so payment is a local OTP simulation.
- If images do not display, check `MEDIA_URL` and the `static()` entry in `urls.py`.

## Test data

After migrating and creating a superuser, add data from `http://localhost:8000/admin`: a few categories (for example medicines, medical equipment, vitamins), some products, and create a clinic account through `/register`.

## Credits

Academic team project by Eya, Maryem Bourourou and Asma Letaief.
