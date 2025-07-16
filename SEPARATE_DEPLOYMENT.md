# Separate Repository Deployment Guide

This guide will help you deploy your frontend and backend as separate repositories to Vercel.

## 🚀 Prerequisites

1. **Two GitHub Repositories**:
   - `portfolio-frontend` - React frontend
   - `portfolio-backend` - Node.js/Express backend
2. **Vercel Account** - Sign up at [vercel.com](https://vercel.com)
3. **MongoDB Atlas** - For database (free tier available)

## 📋 Step 1: MongoDB Atlas Setup

### 1.1 Create MongoDB Atlas Account
1. Go to [MongoDB Atlas](https://www.mongodb.com/atlas)
2. Sign up for a free account
3. Create a new project

### 1.2 Create Database Cluster
1. Click "Build a Database"
2. Choose "FREE" tier (M0)
3. Select your preferred cloud provider and region
4. Click "Create"

### 1.3 Configure Database Access
1. Go to "Database Access" in the left sidebar
2. Click "Add New Database User"
3. Create a username and password (save these!)
4. Select "Read and write to any database"
5. Click "Add User"

### 1.4 Configure Network Access
1. Go to "Network Access" in the left sidebar
2. Click "Add IP Address"
3. Click "Allow Access from Anywhere" (for Vercel deployment)
4. Click "Confirm"

### 1.5 Get Connection String
1. Go to "Database" in the left sidebar
2. Click "Connect"
3. Choose "Connect your application"
4. Copy the connection string
5. Replace `<password>` with your database user password
6. Replace `<dbname>` with `portfolio_contacts`

Your connection string should look like:
```
mongodb+srv://username:password@cluster.mongodb.net/portfolio_contacts?retryWrites=true&w=majority
```

## 🌐 Step 2: Deploy Backend First

### 2.1 Deploy Backend Repository
1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click "New Project"
3. Import your `portfolio-backend` repository
4. Configure the project:
   - **Framework Preset**: Node.js
   - **Root Directory**: `./` (root of repository)
   - **Build Command**: Leave empty
   - **Output Directory**: Leave empty
   - **Install Command**: `npm install`

### 2.2 Add Backend Environment Variables
In the Vercel project settings, go to "Environment Variables" and add:

```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/portfolio_contacts?retryWrites=true&w=majority
NODE_ENV=production
CORS_ORIGIN=https://your-frontend-domain.vercel.app
```

**Important**: Replace `username`, `password`, and `cluster` with your actual MongoDB credentials.

### 2.3 Deploy Backend
1. Click "Deploy"
2. Wait for deployment to complete
3. Copy your backend URL (e.g., `https://portfolio-backend.vercel.app`)

## 🎨 Step 3: Deploy Frontend

### 3.1 Deploy Frontend Repository
1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click "New Project"
3. Import your `portfolio-frontend` repository
4. Configure the project:
   - **Framework Preset**: Create React App
   - **Root Directory**: `./` (root of repository)
   - **Build Command**: `npm run build`
   - **Output Directory**: `build`
   - **Install Command**: `npm install`

### 3.2 Add Frontend Environment Variables
In the Vercel project settings, go to "Environment Variables" and add:

```
REACT_APP_API_URL=https://your-backend-name.vercel.app
```

**Important**: Replace `your-backend-name` with your actual backend Vercel domain.

### 3.3 Deploy Frontend
1. Click "Deploy"
2. Wait for deployment to complete
3. Your frontend will be live at `https://your-frontend-name.vercel.app`

## 🧪 Step 4: Testing Your Deployment

### 4.1 Test Backend API
Visit your backend health endpoint:
```
https://your-backend-name.vercel.app/api/health
```

You should see:
```json
{
  "status": "OK",
  "message": "Portfolio API is running",
  "timestamp": "2024-01-01T00:00:00.000Z"
}
```

### 4.2 Test Frontend
1. Visit your frontend Vercel URL
2. Test the contact form
3. Test the book suggestion form
4. Test the visitor feedback form

### 4.3 Check Database
1. Go to MongoDB Atlas
2. Navigate to "Browse Collections"
3. You should see your data being saved

## 📁 Repository Structure

### Backend Repository (`portfolio-backend/`):
```
portfolio-backend/
├── vercel.json               # Vercel configuration
├── server.js                 # Express server
├── package.json              # Backend dependencies
├── .env                      # Backend environment variables
└── README.md
```

### Frontend Repository (`portfolio-frontend/`):
```
portfolio-frontend/
├── vercel.json               # Vercel configuration
├── src/                      # React source code
├── public/                   # Static assets
├── package.json              # Frontend dependencies
├── .env                      # Frontend environment variables
└── README.md
```

## 🔧 Configuration Files

### Backend `vercel.json`:
```json
{
  "version": 2,
  "builds": [
    {
      "src": "server.js",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/server.js"
    }
  ],
  "functions": {
    "server.js": {
      "maxDuration": 30
    }
  }
}
```

### Frontend `vercel.json`:
```json
{
  "version": 2,
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/static-build",
      "config": {
        "distDir": "build"
      }
    }
  ],
  "routes": [
    {
      "handle": "filesystem"
    },
    {
      "src": "/(.*)",
      "dest": "/build/index.html"
    }
  ]
}
```

## 🛠️ Troubleshooting

### Common Issues:

1. **CORS Errors**
   - Ensure CORS_ORIGIN in backend includes your frontend domain
   - Update the environment variable in Vercel

2. **API Connection Issues**
   - Verify REACT_APP_API_URL is set correctly in frontend
   - Check that the backend URL is accessible

3. **Build Failures**
   - Check the build logs in Vercel
   - Ensure all dependencies are in package.json

4. **Database Connection Issues**
   - Verify your MongoDB connection string
   - Check if your IP is whitelisted in MongoDB Atlas

## 🔄 Updating Your Deployment

### Backend Updates:
1. Push changes to your backend repository
2. Vercel will automatically redeploy
3. Check the deployment logs

### Frontend Updates:
1. Push changes to your frontend repository
2. Vercel will automatically redeploy
3. Check the deployment status

## 📊 Benefits of Separate Deployment

1. **Independent Scaling** - Frontend and backend can scale separately
2. **Easier Maintenance** - Each repository can be updated independently
3. **Better Organization** - Clear separation of concerns
4. **Team Collaboration** - Different teams can work on each part
5. **Cost Optimization** - Only pay for what you use

## 🔒 Security Notes

1. **Environment Variables**: Never commit `.env` files
2. **CORS Configuration**: Set up CORS properly for production
3. **API Security**: Use environment variables for sensitive data
4. **Database Security**: Use strong passwords and restrict access

Your portfolio should now be fully deployed with separate frontend and backend services! 🎉 