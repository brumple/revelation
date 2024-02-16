# Weather Dashboard Application with Comparison Mode

This project is a Weather Dashboard application that leverages the Open-Meteo API to fetch and display weather data. It includes a comparison mode that allows users to compare the weather forecast between two different locations. The application is designed to be responsive and accessible, and it is hosted on GitHub Pages for easy access via web browsers.

## Setup Instructions

To run this application locally, follow these steps:

1. Clone this repository to your local machine.
2. Navigate to the project directory.
3. Install dependencies using npm or yarn:

    ```bash
    npm install
    ```

   or

    ```bash
    yarn install
    ```

4. Run the development server:

    ```bash
    npm run serve
    ```

   or

    ```bash
    yarn serve
    ```

5. Open your web browser and visit `http://localhost:8080` to view the application.

## How to Use the Application

The Weather Dashboard application allows users to compare the weather forecast between two locations. Here's how to use it:

1. Upon loading the application, you'll see an interface to input two locations for comparison.
2. Enter the details of the two locations you want to compare. You can input locations by city name, ZIP code, or coordinates.
3. After entering the locations, the application will fetch the weather forecasts for both locations.
4. Select the specific location from the provided choices for each of the two locations.
5. The application will then display the current weather conditions and a 5-day forecast for both locations side by side.
6. Differences in temperature, precipitation, wind speed, and other relevant weather conditions between the two locations will be highlighted:
   - Differences greater than 5% will be highlighted in orange.
   - Differences greater than 10% will be highlighted in red.

## Project Overview

The Weather Dashboard application allows users to:

- Search for and select two locations to compare their weather forecasts.
- Display the current weather conditions and a 5-day forecast for both locations side by side.
- Highlights differences in temperature, precipitation, wind speed, and other relevant weather conditions.

## Technology Stack

The project is built using Vue.js framework. Vue.js was chosen for its simplicity, reactivity, and ease of integration with APIs. The Vue ecosystem provides excellent tools for building responsive and interactive web applications, making it a suitable choice for this project to be hosted on GitHub pages.

## Unique Comparison Mode Feature

The comparison mode feature of this application allows users to visualize differences in weather conditions between two locations. It highlights significant variations in temperature, precipitation, wind speed, and other relevant parameters, providing users with meaningful insights into weather disparities.

## Live Demo

A live demo of the application is available at [GitHub Pages](https://brumple.github.io/).
