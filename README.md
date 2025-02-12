# IRCTC Railway Management System

## Problem Statement

Hey there, Mr. X. You have been assigned the task of designing a **Railway Management System** similar to IRCTC. Users should be able to search for available trains between two stations, check seat availability, and book seats if available. The system must support real-time seat booking with concurrency control to prevent multiple users from booking the same seat simultaneously.

---

This project is a **Railway Management System** built using **Node.js**, **Express.js**, and **MySQL**. It provides functionalities for train bookings, seat availability checking, and role-based access for users and admins.

## Features

- User registration and login
- JWT-based authentication for security
- Search available trains between stations
- Book train seats with race condition handling
- Admin functionalities: add trains, update seat availability
- Role-based access control (admin/user)
- Input validation and error handling

---

## Project Setup

### Prerequisites

Ensure you have the following installed:

- [Node.js](https://nodejs.org/en/) (v14 or later)
- [MySQL](https://www.mysql.com/)
- [Postman](https://www.postman.com/) (for API testing)

### Environment Variables

Create a `.env` file in the project root with:

```bash
PORT=3000
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=yourpassword
DB_NAME=railway_db
JWT_SECRET=your_jwt_secret
API_KEY=your_admin_api_key
```

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/railway-management.git
   cd railway-management
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Set up the MySQL database:
   ```sql
   CREATE DATABASE railway_db;
   USE railway_db;

   CREATE TABLE users (
       id INT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(255) NOT NULL,
       email VARCHAR(255) UNIQUE NOT NULL,
       password VARCHAR(255) NOT NULL,
       role ENUM('user', 'admin') DEFAULT 'user',
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TABLE trains (
       id INT AUTO_INCREMENT PRIMARY KEY,
       train_number VARCHAR(50) NOT NULL,
       source VARCHAR(255) NOT NULL,
       destination VARCHAR(255) NOT NULL,
       total_seats INT NOT NULL,
       available_seats INT NOT NULL,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TABLE bookings (
       id INT AUTO_INCREMENT PRIMARY KEY,
       user_id INT,
       train_id INT,
       seats INT NOT NULL,
       FOREIGN KEY (user_id) REFERENCES users(id),
       FOREIGN KEY (train_id) REFERENCES trains(id)
   );
   ```

### Start the Server

```bash
npm start
```

By default, the server runs on **http://localhost:3000**.

---

## API Endpoints

### **User Routes**

#### 1. Register User
   - **Method:** POST
   - **Endpoint:** `/user/register`
   - **Body:**
   ```json
   {
       "name": "Alice Smith",
       "email": "alice@example.com",
       "password": "securepassword"
   }
   ```

#### 2. Login User
   - **Method:** POST
   - **Endpoint:** `/user/login`
   - **Body:**
   ```json
   {
       "email": "alice@example.com",
       "password": "securepassword"
   }
   ```

#### 3. Check Train Availability
   - **Method:** GET
   - **Endpoint:** `/user/availability?source=Mumbai&destination=Chennai`
   - **Response:**
   ```json
   {
       "available": true,
       "availableTrainCount": 2,
       "trains": [
           {
               "trainNumber": "11027",
               "availableSeats": 120
           },
           {
               "trainNumber": "12655",
               "availableSeats": 80
           }
       ]
   }
   ```

#### 4. Book Seats
   - **Method:** POST
   - **Endpoint:** `/user/book`
   - **Body:**
   ```json
   {
       "trainId": 2,
       "seatsToBook": 3
   }
   ```
   - **Response:**
   ```json
   {
       "message": "Seats booked successfully"
   }
   ```
   - **Authentication Required:** Yes (JWT Token)

#### 5. View Booking Details
   - **Method:** GET
   - **Endpoint:** `/user/getAllBookings`
   - **Response:**
   ```json
   [
       {
           "booking_id": 12,
           "number_of_seats": 3,
           "train_number": "11027",
           "source": "Mumbai",
           "destination": "Chennai"
       }
   ]
   ```

### **Admin Routes**

#### 1. Add a New Train
   - **Method:** POST
   - **Endpoint:** `/admin/addTrain`
   - **Body:**
   ```json
   {
       "trainNumber": "12139",
       "source": "Bangalore",
       "destination": "Delhi",
       "totalSeats": 250,
       "availableSeats": 250
   }
   ```
   - **Authentication Required:** Yes (Admin API Key)

#### 2. Update Seat Availability
   - **Method:** PUT
   - **Endpoint:** `/admin/update-seats/5`
   - **Body:**
   ```json
   {
       "totalSeats": 300,
       "availableSeats": 200
   }
   ```
   - **Response:**
   ```json
   {
       "message": "Seats updated successfully"
   }
   ```
   - **Authentication Required:** Yes (Admin API Key)

---

## Testing the API

You can test API endpoints using Postman.

Sample Train Data:
```json
[
    {
        "trainNumber": "11027",
        "source": "Mumbai",
        "destination": "Chennai",
        "totalSeats": 150
    },
    {
        "trainNumber": "12655",
        "source": "Mumbai",
        "destination": "Chennai",
        "totalSeats": 200
    }
]
```

## Technologies Used

- **Node.js** - Backend runtime
- **Express.js** - Web framework
- **MySQL** - Database management
- **JWT** - Authentication
- **bcrypt** - Password hashing
- **dotenv** - Environment variable management

## Future Enhancements

- Add a frontend with React.js or Angular
- Implement seat selection
- Send email notifications for booking confirmations
- Integrate a payment gateway

## Contributing

Feel free to fork this repository and submit pull requests. Contributions for improvements and bug fixes are welcome!
