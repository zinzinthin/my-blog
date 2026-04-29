# My Blog 📝

A simple blog application built with **Node.js**, **Express**, **EJS**, and **MongoDB**.

---

## ✨ Features

- Create, view, edit, and delete blog posts  
- MongoDB database (`posts` collection)  
- EJS templating engine with reusable layouts  
- Static assets served from `public/` directory  
- Bootstrap + FontAwesome integration for UI styling  

---

## 🛠️ Tech Stack

- Node.js  
- Express.js  
- EJS (Embedded JavaScript Templates)  
- MongoDB  
- Bootstrap  
- FontAwesome  

---

## 📁 Project Setup

### 1. Clone the repository

```bash
git clone <repo-url>
cd my_blog
```

### 2. Install dependencies

```bash
pnpm install
```

### 3. Environment Variables

```bash
PORT=3000

# MongoDB Configuration
CLUSTER_NAME=cluster0
DB_NAME=my_blog_db
DB_USER=your_username
DB_PASSWORD=your_password

# Local MongoDB (if not using Atlas)
MONGODB_URI=mongodb://127.0.0.1:27017/my_blog_db
```

### 4. Run the Application

```bash 
pnpm dev
```