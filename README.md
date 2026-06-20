# Weather Comparison Application

![Weather App Logo](screenshots/clouds.jpg)

## Overview

This is a small weather web application that compares weather data from two different weather APIs.

The application shows weather information such as:

* Temperature
* Feels-like temperature
* Chance of rain
* Hourly forecast
* Weekly forecast

The main purpose of this project is to show that weather forecasts are not always identical between providers. Even when APIs update frequently, the data can still be different depending on the source, calculation method, and forecast model.

---

## Table of Contents

* [Purpose](#purpose)
* [Features](#features)
* [Tech Stack](#tech-stack)
* [Installation](#installation)
* [Environment Variables](#environment-variables)
* [Running the Application](#running-the-application)
* [Usage](#usage)
* [Screenshots](#screenshots)
* [Architecture](#architecture)
* [Project Structure](#project-structure)
* [License](#license)

---

## Purpose

The Weather Comparison Application was created to help users understand that weather forecast data can vary between providers.

Instead of showing data from only one API, the application compares data from two sources side by side. This makes it easier to see differences between forecast providers and understand that weather predictions are not always fully accurate.

---

## Features

### API Comparison

The application compares weather data from:

* WeatherAPI
* Open-Meteo

This allows users to see differences between two forecast sources.

### Location-Based Weather

The application uses the user’s IP address to detect their approximate location and display weather data for that area.

### API Call Optimization

WeatherAPI has a limited number of monthly API calls. To reduce unnecessary requests, the application stores weather data per user IP address using Redis.

### Current Day Forecast

Users can view hourly weather data for the current day.

### Weekly Forecast

Users can switch from the daily view to a weekly forecast view.

### Interactive Graphs

The application visualizes weather data using graphs, making it easier to compare temperature, rain chance, and forecast changes.

---

## Tech Stack

* Python
* Flask
* Redis
* WeatherAPI
* Open-Meteo API
* IP Geolocation API
* HTML / CSS / JavaScript

---

## Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd <repository-name>
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
```

Activate the virtual environment.

On Windows:

```bash
venv\Scripts\activate
```

On macOS / Linux:

```bash
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

> Note: Make sure the file is named `requirements.txt`. If your project currently uses `requirments.txt`, rename it to `requirements.txt`.

---

## Environment Variables

Create a `.env` file inside:

```txt
main/env/.env
```

Example `.env` file:

```env
REDIS_HOST_URI="your redis host"
REDIS_PORT=6379
REDIS_USERNAME="your redis username"
REDIS_PASSWORD="your redis password"

SECRET_FLASK_KEY="your secret flask key"

# WeatherAPI
WEATHERAPI_API_URI="http://api.weatherapi.com/v1"
WEATHERAPI_API_KEY="your weatherapi key"
WEATHERAPI_NAME="weatherapi.com"

# Open-Meteo
OPENMETEO_URI="https://api.open-meteo.com/v1/forecast"
OPENMETEO_KEY="no key"
OPENMETEO_SOURCE="openmeteo_api"

# IP Geolocation
GEO_API_KEY="your ip geolocation api key"
```

---

## Required External Services

You need accounts or API keys for the following services:

### WeatherAPI

Used as one of the weather data providers.

Website:

```txt
https://www.weatherapi.com/
```

### Open-Meteo

Used as the second weather data provider.

Website:

```txt
https://open-meteo.com/
```

Open-Meteo does not require an API key for basic usage.

### IP Geolocation

Used to detect the user’s approximate location based on their IP address.

Website:

```txt
https://ipgeolocation.io/
```

### Redis

Used for caching weather data and reducing repeated API calls.

You can use a local Redis server or a hosted Redis service such as Redis Cloud.

---

## Running the Application

In `weather.py`, make sure the Flask app is configured correctly.

For development, you can enable debug mode:

```python
if __name__ == "__main__":
    app.secret_key = os.getenv("SECRET_FLASK_KEY")
    app.debug = True
    app.run()
```

Run the application:

```bash
python weather.py
```

The application should start locally.

Example:

```txt
http://127.0.0.1:5000
```

---

## Usage

### For Users

Users can open the application and view weather data for their location.

The application displays weather information from two APIs, allowing users to compare the results and see how forecasts can differ between providers.

### For Developers

The code is structured to make it easier to refactor or add new weather APIs.

To integrate another weather API:

1. Create a new API interface class.
2. Add the new API logic inside the API folder.
3. Connect the new API class to the API adapter.
4. Register it inside the API facade.
5. Display the new API data in the UI.

---

## Screenshots

### Home Screen

![Home Screen](screenshots/first_page2.PNG)

The home page explains the purpose of the application and introduces the weather comparison concept.

### Weather Page — Current Day View

![Weather Page](screenshots/weather_page.PNG)

Displays hourly weather data for the current day.

### Display Navigation

![Display Bar](screenshots/navbar.PNG)

Allows users to switch between different weather views.

### Weather Page — Week View

![Week View](screenshots/weather_page.PNG)

Displays weather data grouped by days, starting from the current day.

---

## Architecture

The application uses a layered structure with several design patterns to keep the code readable and easier to maintain.

### MVC Pattern

![MVC Diagram](screenshots/mvc.PNG)

The top layer of the application follows the MVC pattern.

* **Model**: Handles data and API responses.
* **View**: Displays weather data to the user.
* **Controller**: Handles requests and controls the flow between the view and the application logic.

### Adapter Pattern

![API Adapter Diagram](screenshots/api_diagram.PNG)

The adapter pattern is used to normalize data from different weather APIs.

Each API may return different response structures. The adapter converts those different structures into a common format that the rest of the application can use.

### Facade Pattern

The facade pattern provides a simpler interface for working with multiple APIs.

Instead of the rest of the application calling every API directly, the facade manages the API calls and returns the required weather data in a cleaner way.

### Top Layer Diagram

![Application Diagram](screenshots/top_layer_diagram.PNG)

The top layer diagram shows the general flow of the application and how the main components communicate with each other.

### Component Diagram

The component diagram shows the relationships between the main parts of the application.

In the diagram:

* Arrows represent the flow of data or requests.
* Each component has a specific responsibility.
* External APIs provide weather and location data.
* Redis stores cached weather results to reduce repeated API calls.

---

## Project Structure

```txt
.
├── weather.py
├── main
│   └── env
│       └── .env
├── screenshots
│   ├── clouds.jpg
│   ├── first_page2.PNG
│   ├── weather_page.PNG
│   ├── navbar.PNG
│   ├── top_layer_diagram.PNG
│   ├── mvc.PNG
│   ├── api_diagram.PNG
│   └── interactive_map.png
├── architecture
│   └── component_diagram.png
├── requirements.txt
└── README.md
```

---

## Security Notes

Do not commit your `.env` file to GitHub.

Add it to `.gitignore`:

```txt
.env
main/env/.env
```

API keys, Redis credentials, and Flask secret keys should always stay private.

---

## License

This project is licensed under the MIT License.

See the [LICENSE](LICENSE) file for details.
