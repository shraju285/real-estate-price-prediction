# Bangalore House Price Prediction

A full-stack machine learning web application that predicts house prices in Bangalore based on property characteristics such as location, total square footage, number of bedrooms, and bathrooms.

The project includes a trained Scikit-learn regression model, a Flask REST API, a JavaScript frontend, and an AWS EC2 deployment with Nginx.

## Live Demo

**Web Application:**  
http://13.53.35.108

> The application is hosted on an AWS EC2 instance for demonstration purposes. Availability depends on the EC2 instance being running.

---

## Project Overview

The goal of this project is to build an end-to-end machine learning application that takes property information from a user and returns an estimated house price.

The application follows this workflow:

```text
User
 │
 ▼
Web Interface
 │
 ▼
JavaScript
 │
 ▼
Flask REST API
 │
 ▼
Scikit-learn Model
 │
 ▼
Predicted House Price
```

The application is deployed on AWS using Nginx as a reverse proxy.

---

## Features

- Bangalore house price prediction
- Location-based predictions
- BHK/bedroom input
- Bathroom input
- Total square footage input
- Interactive web interface
- Flask REST API
- Scikit-learn machine learning model
- Nginx reverse proxy
- AWS EC2 deployment
- Linux/systemd-based backend service

---

## Technologies Used

### Machine Learning

- Python
- NumPy
- Scikit-learn
- Joblib

### Backend

- Flask
- Python REST API

### Frontend

- HTML
- CSS
- JavaScript

### Deployment

- AWS EC2
- Ubuntu Linux
- Nginx
- systemd
- Git/GitHub

---

## Machine Learning Model

The project uses a trained Scikit-learn regression model to estimate property prices.

The model uses property information including:

- Location
- Total square footage
- Number of bedrooms (BHK)
- Number of bathrooms

The trained model and supporting artifacts are stored in:

```text
server/artifacts/
├── banglore_home_prices_model.pickle
└── columns.json
```

The original machine learning notebook is available at:

```text
model/bhp.ipynb
```

---

## Project Structure

```text
real-estate-price-prediction/
│
├── client/
│   ├── app.html
│   ├── app.css
│   └── app.js
│
├── model/
│   └── bhp.ipynb
│
├── server/
│   ├── artifacts/
│   │   ├── banglore_home_prices_model.pickle
│   │   └── columns.json
│   ├── requirements.txt
│   ├── server.py
│   └── util.py
│
├── .gitignore
└── README.md
```

---

## API

The Flask backend provides two main API endpoints.

### Get Available Locations

```http
GET /api/get_location_names
```

This endpoint returns the locations supported by the prediction model.

Example:

```bash
curl http://127.0.0.1:5000/get_location_names
```

When running through the deployed Nginx configuration:

```bash
curl http://127.0.0.1/api/get_location_names
```

---

### Predict House Price

```http
POST /api/predict_home_price
```

Example request:

```bash
curl -X POST http://127.0.0.1/api/predict_home_price \
  -d "total_sqft=1000" \
  -d "location=1st phase jp nagar" \
  -d "bhk=2" \
  -d "bath=2"
```

Example response:

```json
{
    "estimated_price": 83.5
}
```

The estimated price is returned in lakhs.

---

## Running the Project Locally

### 1. Clone the repository

```bash
git clone https://github.com/shraju285/real-estate-price-prediction.git
cd real-estate-price-prediction
```

### 2. Create a virtual environment

```bash
python3 -m venv venv
```

Activate it:

**macOS/Linux:**

```bash
source venv/bin/activate
```

**Windows:**

```bash
venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r server/requirements.txt
```

### 4. Start the Flask server

```bash
python server/server.py
```

The Flask API will run on:

```text
http://127.0.0.1:5000
```

---

## Running the Frontend

The frontend files are located in:

```text
client/
```

The deployed version is served by Nginx.

For local development, the frontend can be served using a simple HTTP server or another web server.

For example:

```bash
cd client
python3 -m http.server 8000
```

Then open:

```text
http://127.0.0.1:8000
```

> When running the frontend separately from Flask locally, make sure the API URL configuration matches your local Flask server.

---

## AWS Deployment

The application is deployed on an AWS EC2 Ubuntu server.

The production architecture is:

```text
                    Internet
                       │
                       ▼
                  AWS EC2
                       │
                       ▼
                  Nginx :80
                  /          \
                 /            \
                ▼              ▼
          Static Files       /api/
                              │
                              ▼
                        Flask :5000
                              │
                              ▼
                     Scikit-learn Model
```

### Nginx

Nginx serves the frontend and forwards API requests to Flask.

Frontend:

```text
/
```

API:

```text
/api/
```

Nginx forwards API requests to:

```text
http://127.0.0.1:5000
```

This means Flask does not need to be publicly exposed on port `5000`.

---

## Backend Service

The Flask application is managed using `systemd`.

The service:

```text
bhp.service
```

is configured to automatically restart the Flask application if it stops.

Useful commands:

```bash
sudo systemctl status bhp
```

Start the service:

```bash
sudo systemctl start bhp
```

Restart the service:

```bash
sudo systemctl restart bhp
```

Stop the service:

```bash
sudo systemctl stop bhp
```

Enable it at system startup:

```bash
sudo systemctl enable bhp
```

View application logs:

```bash
sudo journalctl -u bhp -n 50 --no-pager
```

---

## Nginx Configuration

Nginx is configured to:

1. Serve the frontend files.
2. Route `/api/` requests to Flask.
3. Keep Flask listening locally on port `5000`.

The application therefore uses:

```text
Browser
   │
   ▼
Nginx :80
   │
   ├── Frontend
   │
   └── /api/
          │
          ▼
       Flask :5000
```

---

## Dependencies

The tested deployment environment uses:

```text
Flask==2.0.3
Jinja2==3.0.3
MarkupSafe==2.0.1
Werkzeug==2.0.3
itsdangerous==2.0.1
click==8.5.0
numpy==2.2.6
scikit-learn==1.7.2
joblib==1.6.0
scipy==1.15.3
threadpoolctl==3.7.0
cloudpickle==3.1.2
```

Python 3.10 was used for the deployed environment.

---

## Example Prediction

Example input:

```text
Location: 1st Phase JP Nagar
BHK: 2
Bathrooms: 2
Total Area: 1000 sq ft
```

The model returns an estimated property price based on the trained data and model.

Example API response:

```json
{
    "estimated_price": 83.5
}
```

---

## Deployment Environment

The deployed application uses:

- AWS EC2
- Ubuntu Linux
- Python 3.10
- Flask
- Nginx
- systemd
- Scikit-learn

The Flask application runs locally on the EC2 instance:

```text
127.0.0.1:5000
```

Nginx handles public HTTP traffic on:

```text
Port 80
```

---

## What I Learned

This project provided practical experience with:

- Machine learning model development
- Data preprocessing
- Regression models
- Python and Scikit-learn
- Flask REST APIs
- Frontend/backend integration
- Linux server administration
- SSH
- Nginx reverse proxy configuration
- systemd services
- AWS EC2
- Git and GitHub
- Deploying a machine learning application to the cloud

---

## Future Improvements

Potential improvements include:

- Improve model accuracy through additional feature engineering
- Add more detailed property features
- Improve frontend design and responsiveness
- Add input validation
- Add automated testing
- Containerize the application with Docker
- Add CI/CD using GitHub Actions
- Add HTTPS with an SSL certificate
- Use a domain name instead of the EC2 public IP
- Add monitoring and logging
- Deploy using a more scalable cloud architecture

---

## Disclaimer

This project is intended for educational and demonstration purposes.

The predicted prices are machine learning estimates and should not be considered professional property valuations or financial advice.

---

## Author

**MD Shahadat Hossain Raju**

GitHub:  
https://github.com/shraju285

---

## Repository

GitHub repository:

https://github.com/shraju285/real-estate-price-prediction
