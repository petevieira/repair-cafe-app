# Repair Cafe Management App

A React Native application designed to streamline operations for repair cafes, integrating seamlessly with MongoDB Atlas for data management and Render.com for backend and website deployment.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Pricing](#pricing)
3. [Features](#features)
4. [Prerequisites](#prerequisites)
5. [Installation](#installation)
6. [Configuration](#configuration)
   - [MongoDB Atlas Setup](#mongodb-atlas-setup)
   - [Render.com Deployment](#rendercom-deployment)
7. [Running the Application](#running-the-application)
8. [Contributing](#contributing)
9. [License](#license)
10. [Acknowledgements](#acknowledgements)

---

## Introduction

The Repair Cafe Management App is crafted to assist repair cafes in managing their operations efficiently. It leverages React Native for a cross-platform web and mobile experience, [MongoDB Atlas](https://www.mongodb.com/docs/atlas/) for scalable cloud database solutions, and [Render.com](https://render.com/docs/web-services) for seamless backend and website service deployment.

---

## Pricing

#### Cheapest option

- **MongoDB Atlas**: Free tier (512MB of storage, shared RAM, shared CPU)
- **Render.com**: Hobby tier (free, backend coldstarts)
- **Domain registration**: Price of domain

---

## Features

- **User Authentication:**
  - Manually creation of main volunteer and admin accounts in MongoDB Atlas
  - Secure login as volunteer or admin (to edit past and future events)
- **Repair Logging:**
  - Create and edit repairs
  - Update repair status
  - Update repair repairer
  - Identify follow-up repairs
- **Volunteer Tracking:**
  - Add and edit volunteers for a repair event
- **Terms of Service:**
  - Acceptance of waiver for item owners and volunteers

---

## Prerequisites

Before setting up the application, ensure you have the following installed:

- **Node.js** (tested on version v20.11.1)
- **npm** or **yarn** (for package management)
- **React Native CLI** or **Expo CLI** (based on your development preference)
- **Git** (for version control)
- **MongoDB Atlas Account** (for database services)
- **Render.com Account** (for backend and web-app deployment)

---

## Installation

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/yourusername/repair-cafe-app.git
   cd repair-cafe-app
   ```

2. **Install Dependencies:**

   Using npm:

   ```bash
   npm install
   ```

   Or using yarn:

   ```bash
   yarn install
   ```

---

## Configuration

### MongoDB Atlas Setup

1. **Create a Cluster:**
   - Log in to your MongoDB Atlas account.
   - Create a new cluster, selecting your preferred cloud provider and region.

2. **Database User:**
   - Navigate to the "Database Access" section.
   - Add a new database user with a secure password.

3. **Network Access:**
   - In the "Network Access" section, add IP addresses that are permitted to access the database. For development purposes, you can allow access from all IPs (`0.0.0.0/0`), but it's recommended to restrict this in production environments.

4. **Connection String:**
   - Obtain the connection string from the "Clusters" section by clicking "Connect" and selecting "Connect your application."
   - Replace `<password>` with your database user's password.

   Example:
   ```
   mongodb+srv://<username>:<password>@cluster0.mongodb.net/myFirstDatabase?retryWrites=true&w=majority
   ```

### Render.com Deployment

1. **Create a New Web Service:**
   - Log in to your Render.com account.
   - Click on "New" and select "Web Service."

2. **Connect to Repository:**
   - Connect your GitHub or GitLab account and select the repository containing your backend code.

3. **Environment Variables:**
   - In the "Environment" section, add the following variables:
     - `MONGODB_URI`: Your MongoDB Atlas connection string.
     - Any other variables your application requires (e.g., API keys, secret tokens).

4. **Build and Start Commands:**
   - Specify the commands to build and start your application. For a Node.js backend, these might be:
     - Build Command: `npm install`
     - Start Command: `npm start`

5. **Deploy:**
   - Click "Create Web Service" to deploy your backend. Render.com will handle the deployment process and provide a URL for your live service.

---

## Running the Application

1. **Start the Backend:**
   Ensure your backend is running, either locally or on Render.com.

2. **Configure the Frontend:**
   - In the React Native project, locate the configuration file (e.g., `config.js`) and set the API base URL to your backend's URL.

3. **Run the Application:**

   Using Expo CLI:

   ```bash
   expo start
   ```

   Or using React Native CLI:

   ```bash
   react-native run-android
   ```

   or

   ```bash
   react-native run-ios
   ```

---

## Contributing

We welcome contributions from the community. To contribute:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/YourFeature`).
3. Commit your changes (`git commit -m 'Add new feature'`).
4. Push to the branch (`git push origin feature/YourFeature`).
5. Open a Pull Request.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Acknowledgements

We extend our gratitude to the open-source community and the following resources:

- [React Native Documentation](https://reactnative.dev/docs/getting-started)
- [MongoDB Atlas Documentation](https://docs.atlas.mongodb.com/)
- [Render.com Documentation](https://render.com/docs)

For a practical example of a well-documented React Native project integrating MongoDB, consider reviewing this repository: [github.com](https://github.com/luckydoglou/react-native-mongodb-mobile-app?utm_source=chatgpt.com).

Additionally, for a visual guide on integrating MongoDB App Services in a React Native app, you might find this tutorial helpful:

[Using MongoDB App Services and Realm in a React Native App](https://www.youtube.com/watch?v=AYvcEug8hJI).

By following this structured approach, you can create a README that not only provides clear setup instructions but also serves as a valuable resource for users and contributors alike.

