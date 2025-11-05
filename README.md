## Weather Backend (Django)

Modern Django app that shows current weather and a 5-slot short-term forecast for temperature and humidity. It fetches live conditions from OpenWeatherMap and trains lightweight models on your local historical CSV to predict near-future values.

### Features
- Fetch current conditions by city (metric units)
- Display humidity, clouds, pressure, visibility, wind direction/speed
- 5 future time slots (hourly) temperature and humidity prediction
- Simple, responsive UI with Chart.js visualization

### Tech Stack
- Django 5.x
- scikit-learn, pandas, numpy
- Requests
- Chart.js for client-side charting

---

## Prerequisites
- Python 3.12+
- An OpenWeatherMap API key
- A historical CSV file at `weather.csv` with at least these columns:
  - `MinTemp`, `MaxTemp`, `WindGustDir`, `Humidity`, `Pressure`, `Temp`, `RainTomorrow`
 - Optional but recommended: a `.env` file for secrets management

Note: The app expects the CSV to be located at `D:\Projects\weather backend\weather.csv` (Windows path). You can change this path in `weatherProject/forecast/views.py` if your project directory differs.

---

## Quick Start

1) Clone and enter the project
```bash
git clone <your-repo-url>
cd "weather backend"
```

2) Create and activate a virtual environment (Windows PowerShell)
```bash
python -m venv .venv
./.venv/Scripts/Activate.ps1
```

3) Install dependencies
```bash
pip install -r requirements.txt
```

4) Provide your OpenWeatherMap API key
- Option A (recommended): create a `.env` file at the repo root
```bash
echo API_KEY=<YOUR_OPENWEATHERMAP_API_KEY> > .env
```
- Option B: set it in your shell (PowerShell)
```bash
$env:API_KEY = "<YOUR_OPENWEATHERMAP_API_KEY>"
```

5) Ensure `weather.csv` exists at the project root
```text
weather backend/weather.csv
```

6) Run migrations and start the server
```bash
python weatherProject/manage.py migrate
python weatherProject/manage.py runserver
```

7) Open the app
```text
http://127.0.0.1:8000/
```

---

## Usage
- Type a city (e.g., "London") and submit.
- The page shows current metrics and 5 upcoming hourly predictions.

Routes
- `/` root: main weather search and display.

---

## Configuration Notes
- Environment variable: `API_KEY` must be set in your shell before starting the server.
- CSV path: automatically resolved to one level above `settings.BASE_DIR` as `weather.csv` at the repository root.
- Environment loading: the app automatically loads environment variables from a `.env` file located at the repository root (via `python-dotenv`).

### Django environment variables
The following variables can be set in `.env` or your environment:
- `DJANGO_DEBUG` (default `True`) — set to `False` for production
- `DJANGO_ALLOWED_HOSTS` — comma-separated list when `DJANGO_DEBUG=False` (e.g. `example.com,www.example.com`)
- `DJANGO_SECRET_KEY` — required in production; overrides the default development key

Example `.env` snippet:
```bash
DJANGO_DEBUG=False
DJANGO_ALLOWED_HOSTS=yourdomain.com,www.yourdomain.com
DJANGO_SECRET_KEY=replace_with_a_long_random_secret
API_KEY=<YOUR_OPENWEATHERMAP_API_KEY>
```

### Deploying
- Ensure `DJANGO_DEBUG=False`, set `DJANGO_SECRET_KEY`, and configure `DJANGO_ALLOWED_HOSTS`.
- Serve static files using a production-ready setup (e.g., WhiteNoise or via your web server).
- Use a real database (PostgreSQL/MySQL) by updating `DATABASES` in `settings.py`.

Static files
- Served from the `forecast/static/` directory. The template `forecast/templates/weather.html` references CSS, images, and JS under that folder.

---

## Project Structure
```text
weather backend/
├── weather.csv
└── weatherProject/
    ├── manage.py
    ├── weatherProject/
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    └── forecast/
        ├── views.py
        ├── urls.py
        ├── templates/
        │   └── weather.html
        └── static/
            ├── css/
            ├── js/
            └── img/
```

---

## Development
Run checks and server:
```bash
python weatherProject/manage.py check
python weatherProject/manage.py runserver
```

Create a superuser (optional) and visit `/admin/`:
```bash
python weatherProject/manage.py createsuperuser
```

---

## Troubleshooting
- 400/404 or template not rendering: ensure `forecast` is in `INSTALLED_APPS` and `forecast.urls` is included in the project `urls.py`.
- OpenWeather errors: verify `$env:API_KEY` is set and valid.
- CSV/KeyErrors during training: confirm `weather.csv` columns and values match the expected schema. Remove blank rows and duplicates if necessary.
- Path issues on Windows: keep the project folder name as `weather backend` or update the `csv_path` in `views.py`.

---

## License
MIT or your preferred license.


