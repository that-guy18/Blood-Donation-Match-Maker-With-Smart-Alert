# 🩸 Raktsetu - Blood Donation Match Maker with Smart Alert

A comprehensive web platform built with HTML, CSS, JavaScript, MongoDB, and Node.js to bridge the gap between blood donors and recipients through intelligent matching and real-time alert systems.

## 📋 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [Project Structure](#project-structure)
- [Database Schema](#database-schema)
- [API Documentation](#api-documentation)
- [Screenshots](#screenshots)
- [Contributing](#contributing)
- [License](#license)

## 🌟 Overview

**Raktsetu** (meaning "Blood Bridge" in Sanskrit) is a full-stack web application designed to create an efficient connection between blood donors and those in need. The platform features intelligent matching algorithms, real-time notifications, and a user-friendly interface to facilitate life-saving blood donations.

## ✨ Features

### 👤 User Features
- **Dual Registration**: Register as donor, recipient, or both
- **Profile Management**: Complete profiles with medical history and contact details
- **Smart Search**: Find donors/recipients by blood type, location, and availability
- **Donation History**: Track past donations and requests
- **Privacy Controls**: Manage information visibility

### 🔍 Matching System
- **Blood Type Compatibility**: Automatic compatibility checking (A+, A-, B+, B-, AB+, AB-, O+, O-)
- **Location-based Matching**: Find nearby donors using geolocation
- **Availability Tracking**: Real-time status updates
- **Emergency Priority**: Urgent requests get higher visibility

### 🔔 Smart Alert System
- **Urgent Request Alerts**: Instant notifications for critical needs
- **Donor Availability Alerts**: Notify when compatible donors are available
- **SMS/Email Notifications**: Multi-channel alert system
- **Reminder System**: Donation eligibility and appointment reminders

### 🏥 Admin Panel
- **Dashboard Analytics**: Real-time statistics and insights
- **User Management**: Verify and manage donors/recipients
- **Request Moderation**: Review and approve donation requests
- **Emergency Broadcast**: Send mass alerts during crises

## 🛠️ Tech Stack

### Frontend
- **HTML5** - Semantic markup
- **CSS3** - Styling with Flexbox/Grid
- **JavaScript (ES6+)** - Client-side interactivity
- **EJS Templates** - Dynamic content rendering
- **Chart.js** - Data visualization
- **Map APIs** - Location services

### Backend
- **Node.js** - Runtime environment
- **Express.js** - Web application framework
- **MongoDB** - NoSQL database
- **Mongoose** - MongoDB object modeling
- **Socket.io** - Real-time communication

### Additional Dependencies
- **bcryptjs** - Password hashing
- **jsonwebtoken** - Authentication tokens
- **nodemailer** - Email notifications
- **twilio** (optional) - SMS notifications
- **express-session** - Session management
- **multer** - File uploads
- **geolib** - Distance calculations

## 📋 Prerequisites

Before installation, ensure you have:

1. **Node.js** (v14 or higher) - [Download](https://nodejs.org/)
2. **MongoDB** (v4.4 or higher) - [Download](https://www.mongodb.com/try/download/community)
3. **npm** (Node Package Manager)
4. **Git** (for cloning repository)

## 🚀 Installation

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/raktsetu.git
cd raktsetu
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Set Up MongoDB
```bash
# Start MongoDB service (Linux/Mac)
sudo systemctl start mongod

# Or on Windows
net start MongoDB

# Create database directory (if needed)
mkdir -p /data/db
```

### 4. Configure Environment Variables
Create a `.env` file in the root directory:
```env
# Server Configuration
PORT=3000
NODE_ENV=development

# Database Configuration
MONGODB_URI=mongodb://localhost:27017/raktsetu

# Session Secret
SESSION_SECRET=your_session_secret_key_here

# Email Configuration (for notifications)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_email_password

# SMS Configuration (optional - for Twilio)
TWILIO_ACCOUNT_SID=your_account_sid
TWILIO_AUTH_TOKEN=your_auth_token
TWILIO_PHONE_NUMBER=+1234567890

# JWT Secret
JWT_SECRET=your_jwt_secret_key_here
```

### 5. Initialize Database
```bash
# Run database initialization script (if available)
npm run init-db

# Or manually create collections
mongo < scripts/init.mongo.js
```

## ⚙️ Configuration

### Email Setup (for notifications)
1. For Gmail, enable "Less secure app access" or use App Passwords
2. Update `.env` with your email credentials
3. Test email functionality with: `npm run test-email`

### SMS Setup (optional)
1. Sign up for [Twilio](https://www.twilio.com/)
2. Get your Account SID, Auth Token, and phone number
3. Add them to your `.env` file

### Geolocation Setup
1. Get API key from Google Maps or OpenStreetMap
2. Update map configuration in `config/map.js`

## 🏃 Running the Application

### Development Mode
```bash
npm run dev
```
The application will be available at `http://localhost:3000`

### Production Mode
```bash
npm start
```

### With Nodemon (auto-restart on changes)
```bash
npm run watch
```

## 📁 Project Structure

```
raktsetu/
├── public/                 # Static files
│   ├── css/              # Stylesheets
│   ├── js/               # Client-side JavaScript
│   ├── images/           # Images and icons
│   └── uploads/          # User uploaded files
├── views/                # EJS templates
│   ├── layouts/          # Layout templates
│   ├── partials/         # Reusable components
│   ├── pages/            # Page templates
│   └── errors/           # Error pages
├── routes/               # Express routes
│   ├── auth.js          # Authentication routes
│   ├── donor.js         # Donor-related routes
│   ├── recipient.js     # Recipient-related routes
│   ├── admin.js         # Admin routes
│   └── api.js           # API endpoints
├── models/               # Mongoose models
│   ├── User.js          # User schema
│   ├── Donor.js         # Donor schema
│   ├── Recipient.js     # Recipient schema
│   ├── Request.js       # Donation request schema
│   └── Alert.js         # Alert schema
├── controllers/          # Business logic
├── middleware/           # Custom middleware
├── config/              # Configuration files
├── utils/               # Utility functions
├── .env                 # Environment variables
├── .gitignore           # Git ignore file
├── package.json         # Dependencies
├── server.js            # Entry point
└── README.md            # This file
```

## 🗄️ Database Schema

### User Collection
```javascript
{
  _id: ObjectId,
  email: String,
  password: String,
  role: ['donor', 'recipient', 'admin'],
  phone: String,
  address: {
    street: String,
    city: String,
    state: String,
    zip: String,
    coordinates: [Number]  // [longitude, latitude]
  },
  createdAt: Date,
  isVerified: Boolean
}
```

### Donor Collection
```javascript
{
  _id: ObjectId,
  userId: ObjectId,
  fullName: String,
  bloodType: String,
  age: Number,
  weight: Number,
  lastDonation: Date,
  medicalConditions: [String],
  availability: {
    status: ['available', 'unavailable', 'emergency-only'],
    nextAvailable: Date
  },
  donationsMade: Number,
  rating: Number
}
```

### Recipient Collection
```javascript
{
  _id: ObjectId,
  userId: ObjectId,
  patientName: String,
  bloodType: String,
  hospital: String,
  urgency: ['low', 'medium', 'high', 'critical'],
  requiredBy: Date,
  quantity: Number,  // in units
  status: ['pending', 'matched', 'fulfilled', 'cancelled']
}
```

### Alert Collection
```javascript
{
  _id: ObjectId,
  type: ['urgent', 'match', 'reminder', 'broadcast'],
  recipientId: ObjectId,
  donorIds: [ObjectId],
  message: String,
  priority: Number,
  status: ['sent', 'pending', 'failed'],
  createdAt: Date
}
```

## 📡 API Documentation

### Authentication
```
POST    /api/auth/register     - Register new user
POST    /api/auth/login        - User login
POST    /api/auth/logout       - User logout
GET     /api/auth/profile      - Get user profile
```

### Donors
```
GET     /api/donors            - List all donors
GET     /api/donors/:id        - Get donor details
POST    /api/donors            - Create donor profile
PUT     /api/donors/:id        - Update donor profile
GET     /api/donors/nearby     - Find nearby donors
```

### Recipients
```
GET     /api/recipients        - List requests
POST    /api/recipients        - Create request
PUT     /api/recipients/:id    - Update request
DELETE  /api/recipients/:id    - Cancel request
POST    /api/recipients/urgent - Create urgent request
```

### Matching
```
GET     /api/match/:requestId  - Find matches for request
POST    /api/match/confirm     - Confirm match
GET     /api/match/history     - Get match history
```

### Alerts
```
GET     /api/alerts            - Get user alerts
POST    /api/alerts            - Create alert
PUT     /api/alerts/:id/read   - Mark alert as read
POST    /api/alerts/broadcast  - Admin broadcast
```

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/AmazingFeature`
3. Commit your changes: `git commit -m 'Add AmazingFeature'`
4. Push to the branch: `git push origin feature/AmazingFeature`
5. Open a Pull Request

### Code Style
- Use meaningful variable names
- Comment complex logic
- Follow existing code structure
- Test your changes thoroughly

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

For support, email: support@raktsetu.org or create an issue in the GitHub repository.

## 🙏 Acknowledgments

- Medical professionals for guidance on blood donation protocols
- Open-source community for invaluable tools and libraries
- Blood banks and donation centers for their cooperation
- All donors who make this platform meaningful

---

**Disclaimer**: This platform facilitates connections but does not guarantee blood availability. Always consult medical professionals for urgent needs.

**Emergency Contact**: In case of immediate life-threatening situations, please contact emergency services first.
