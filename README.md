# Bangalore Home Price Prediction

A machine learning web application that predicts Bangalore residential property prices based on area, BHK, bathrooms, and location.

The application uses a Linear Regression model and provides a web interface built with HTML, CSS, and JavaScript. The backend is implemented with Flask and deployed on AWS EC2 behind Nginx.

## Live Demo

http://ec2-13-53-35-108.eu-north-1.compute.amazonaws.com/

> The live demo is hosted on an AWS EC2 instance and may be unavailable when the instance is stopped.

## Features

- Predict Bangalore house prices using:
  - Property area in square feet
  - Number of bedrooms (BHK)
  - Number of bathrooms
  - Location
- Location dropdown populated dynamically from the Flask API
- Machine learning prediction using Linear Regression
- REST API built with Flask
- Nginx reverse proxy
- Deployed on AWS EC2
- systemd service for automatic Flask startup

## Architecture

```text
                    User Browser
                         |
                         v
                    Nginx :80
                    /         \
                   /           \
                  v             v
             Frontend       /api/*
          HTML/CSS/JS           |
                                v
                         Flask :5000
                                |
                                v
                       Machine Learning Model
                                |
                                v
                         Price Prediction
