# 🎬 StreamForge

A full-featured backend application for a modern video streaming platform built with Node.js, Express, and MongoDB.

StreamForge showcases real-world backend development concepts including authentication, authorization, media uploads, MongoDB aggregation pipelines, cloud storage integration, and RESTful API design. The project implements features commonly found in modern content-sharing platforms while following clean architecture and industry-standard development practices.

---

## 🚀 Tech Stack

* **Runtime** — Node.js
* **Framework** — Express.js v5
* **Database** — MongoDB with Mongoose ODM
* **Authentication** — JWT (Access Token + Refresh Token)
* **Password Hashing** — bcrypt
* **File Uploads** — Multer
* **Cloud Storage** — Cloudinary
* **API Architecture** — RESTful APIs with standardized ApiResponse and ApiError patterns

---

## ✨ Features

### 🔐 Authentication & User Management

* User registration and login with JWT authentication.
* Secure HTTP-only cookie-based session handling.
* Access and refresh token strategy.
* Password update functionality.
* Profile management with avatar and cover image support.

### 🎥 Video Management

* Upload videos and thumbnails to Cloudinary.
* Publish and unpublish videos.
* Track video views.
* Maintain personalized watch history.
* Search, sort, filter, and paginate videos.

### 💬 Comments

* Create, update, and delete comments.
* Ownership-based authorization checks.
* Retrieve comments for individual videos.

### ❤️ Likes

* Unified like system for videos, comments, and posts.
* Toggle like and unlike functionality.
* Retrieve all videos liked by a user.

### 📺 Subscriptions

* Subscribe and unsubscribe from creators.
* View creator subscriber lists.
* View subscribed channels.

### 📚 Playlists

* Create and manage playlists.
* Add and remove videos using MongoDB operators.
* Retrieve all playlists created by a user.

### 📝 Posts

* Lightweight text-post feature.
* Full CRUD operations.
* Integrated like support.

### 📊 Creator Dashboard

* Creator performance analytics.
* Total subscribers.
* Total views.
* Total likes.
* Total uploaded videos.

### 🔍 MongoDB Aggregation Pipelines

* Creator profile generation.
* Subscriber analytics.
* Watch history aggregation.
* Video feed generation.
* Dashboard statistics and reporting.

---

## 📂 Project Structure

```text
src/
├── controllers/
│   ├── user.controller.js
│   ├── video.controller.js
│   ├── comment.controller.js
│   ├── like.controller.js
│   ├── subscription.controller.js
│   ├── playlist.controller.js
│   ├── tweet.controller.js
│   ├── dashboard.controller.js
│   └── healthcheck.controller.js
├── models/
│   ├── user.model.js
│   ├── video.model.js
│   ├── comment.model.js
│   ├── like.model.js
│   ├── subscription.model.js
│   ├── playlist.model.js
│   └── tweet.model.js
├── routes/
│   ├── user.routes.js
│   ├── video.routes.js
│   ├── comment.routes.js
│   ├── like.routes.js
│   ├── subscription.routes.js
│   ├── playlist.routes.js
│   ├── tweet.routes.js
│   ├── dashboard.routes.js
│   └── healthcheck.routes.js
├── middlewares/
│   ├── auth.middleware.js
│   └── multer.middleware.js
├── utils/
│   ├── ApiError.js
│   ├── ApiResponse.js
│   ├── asyncHandler.js
│   └── cloudinary.js
├── app.js
└── index.js

Public/
└── Temp/
```

---

## 🔗 API Endpoints

### 🔐 Authentication & Users — `/api/v1/users`

| Method | Endpoint           | Auth | Description                               |
| ------ | ------------------ | ---- | ----------------------------------------- |
| POST   | `/register`        | ❌    | Register with avatar and cover image      |
| POST   | `/login`           | ❌    | Login and receive access & refresh tokens |
| POST   | `/logout`          | ✅    | Logout and invalidate session             |
| POST   | `/refresh-token`   | ❌    | Generate a new access token               |
| POST   | `/change-password` | ✅    | Change current password                   |
| GET    | `/current-user`    | ✅    | Get authenticated user details            |
| PATCH  | `/update-account`  | ✅    | Update account information                |
| PATCH  | `/avatar`          | ✅    | Update avatar image                       |
| PATCH  | `/cover-image`     | ✅    | Update cover image                        |
| GET    | `/c/:username`     | ✅    | Get creator profile                       |
| GET    | `/history`         | ✅    | Get watch history                         |

### 🎥 Videos — `/api/v1/videos`

| Method | Endpoint                   | Auth | Description                                |
| ------ | -------------------------- | ---- | ------------------------------------------ |
| GET    | `/`                        | ❌    | Retrieve videos with search and pagination |
| POST   | `/`                        | ✅    | Upload a new video                         |
| GET    | `/:videoId`                | ✅    | Retrieve a video and increment view count  |
| PATCH  | `/:videoId`                | ✅    | Update video details                       |
| DELETE | `/:videoId`                | ✅    | Delete a video                             |
| PATCH  | `/toggle/publish/:videoId` | ✅    | Toggle publish status                      |

### 💬 Comments — `/api/v1/comments`

| Method | Endpoint        | Auth | Description                   |
| ------ | --------------- | ---- | ----------------------------- |
| GET    | `/:videoId`     | ✅    | Retrieve comments for a video |
| POST   | `/:videoId`     | ✅    | Add a comment                 |
| PATCH  | `/c/:commentId` | ✅    | Update a comment              |
| DELETE | `/c/:commentId` | ✅    | Delete a comment              |

### ❤️ Likes — `/api/v1/likes`

| Method | Endpoint               | Auth | Description              |
| ------ | ---------------------- | ---- | ------------------------ |
| POST   | `/toggle/v/:videoId`   | ✅    | Toggle like on a video   |
| POST   | `/toggle/c/:commentId` | ✅    | Toggle like on a comment |
| POST   | `/toggle/t/:tweetId`   | ✅    | Toggle like on a post    |
| GET    | `/videos`              | ✅    | Get liked videos         |

### 📺 Subscriptions — `/api/v1/subscriptions`

| Method | Endpoint           | Auth | Description              |
| ------ | ------------------ | ---- | ------------------------ |
| POST   | `/c/:channelId`    | ✅    | Subscribe or unsubscribe |
| GET    | `/c/:channelId`    | ✅    | Get channel subscribers  |
| GET    | `/u/:subscriberId` | ✅    | Get subscribed channels  |

### 📚 Playlists — `/api/v1/playlist`

| Method | Endpoint                       | Auth | Description                |
| ------ | ------------------------------ | ---- | -------------------------- |
| POST   | `/`                            | ✅    | Create a playlist          |
| GET    | `/:playlistId`                 | ✅    | Get playlist details       |
| PATCH  | `/:playlistId`                 | ✅    | Update playlist            |
| DELETE | `/:playlistId`                 | ✅    | Delete a playlist          |
| PATCH  | `/add/:videoId/:playlistId`    | ✅    | Add video to playlist      |
| PATCH  | `/remove/:videoId/:playlistId` | ✅    | Remove video from playlist |
| GET    | `/user/:userId`                | ✅    | Get user playlists         |

### 📝 Posts — `/api/v1/tweets`

| Method | Endpoint        | Auth | Description         |
| ------ | --------------- | ---- | ------------------- |
| POST   | `/`             | ✅    | Create a post       |
| GET    | `/user/:userId` | ✅    | Get posts by a user |
| PATCH  | `/:tweetId`     | ✅    | Update a post       |
| DELETE | `/:tweetId`     | ✅    | Delete a post       |

### 📊 Dashboard — `/api/v1/dashboard`

| Method | Endpoint  | Auth | Description           |
| ------ | --------- | ---- | --------------------- |
| GET    | `/stats`  | ✅    | Get creator analytics |
| GET    | `/videos` | ✅    | Get creator videos    |

### 🩺 Health Check — `/api/v1/healthcheck`

| Method | Endpoint | Auth | Description         |
| ------ | -------- | ---- | ------------------- |
| GET    | `/`      | ❌    | Check server status |

---

## ⚙️ Setup & Installation

### Prerequisites

* Node.js v18+
* MongoDB Atlas account or local MongoDB instance
* Cloudinary account

### Environment Variables

Create a `.env` file in the project root:

```env
PORT=8000

MONGODB_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net

CORS_ORIGIN=*

ACCESS_TOKEN_SECRET=your_access_token_secret
ACCESS_TOKEN_EXPIRY=1d

REFRESH_TOKEN_SECRET=your_refresh_token_secret
REFRESH_TOKEN_EXPIRY=10d

CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
```

### Run Locally

```bash
git clone https://github.com/Veddd27/streamforge-backend.git

cd streamforge-backend

npm install

npm run dev
```

Server runs at:

```bash
http://localhost:8000
```

---

## 🏗️ Key Implementation Details

### 🔑 JWT Authentication

* Access tokens for authorization.
* Refresh tokens for session persistence.
* Refresh token invalidation on logout.

### ❤️ Unified Like Model

A single Like schema handles likes for:

* Videos
* Comments
* Posts

### ⚡ Aggregation Pipelines

MongoDB aggregation pipelines power:

* Creator profiles
* Subscriber counts
* Watch history
* Dashboard analytics
* Video feeds

### ☁️ Media Upload Workflow

1. Multer stores files temporarily in `Public/Temp/`.
2. Files are uploaded to Cloudinary.
3. Cloudinary URLs are stored in MongoDB.
4. Temporary files are cleaned up automatically.

---

## 🎯 Key Concepts Demonstrated

* 🔐 JWT Authentication & Authorization
* 🗄️ MongoDB Data Modeling
* ⚡ Aggregation Pipelines
* ☁️ Cloudinary Integration
* 📤 File Upload Handling
* 🔄 RESTful API Design
* 🧩 Middleware Architecture
* 🛡️ Error Handling & Validation
* 🔗 Database Relationships & Lookups
* 🍪 Secure Cookie-Based Authentication

---

## 👨‍💻 Author

**Ved Ambulkar**

GitHub: https://github.com/Veddd27

