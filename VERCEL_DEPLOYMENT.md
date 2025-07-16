# Vercel Deployment Guide

This guide will help you deploy your complete portfolio (frontend + backend) to Vercel.

## 🚀 Prerequisites

1. **GitHub Account** - Your code should be in a GitHub repository
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

## 🌐 Step 2: Vercel Deployment

### 2.1 Connect GitHub Repository
1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click "New Project"
3. Import your GitHub repository
4. Select the repository containing your portfolio

### 2.2 Configure Project Settings
1. **Framework Preset**: Other
2. **Root Directory**: `./` (root of repository)
3. **Build Command**: Leave empty (handled by vercel.json)
4. **Output Directory**: Leave empty (handled by vercel.json)
5. **Install Command**: `npm install`

### 2.3 Add Environment Variables
In the Vercel project settings, go to "Environment Variables" and add:

```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/portfolio_contacts?retryWrites=true&w=majority
NODE_ENV=production
CORS_ORIGIN=https://your-vercel-domain.vercel.app
```

**Important**: Replace `username`, `password`, and `cluster` with your actual MongoDB credentials.

### 2.4 Deploy
1. Click "Deploy"
2. Wait for deployment to complete
3. Your portfolio will be live at `https://your-project-name.vercel.app`

## 🔧 Step 3: Update Frontend API URL

After deployment, you need to update the frontend to use the correct API URL:

1. Go to your Vercel project dashboard
2. Navigate to "Settings" → "Environment Variables"
3. Add a new environment variable:
   - **Name**: `REACT_APP_API_URL`
   - **Value**: `https://your-vercel-domain.vercel.app`
4. Redeploy the project

## 🧪 Step 4: Testing Your Deployment

### 4.1 Test Backend API
Visit your backend health endpoint:
```
https://your-vercel-domain.vercel.app/api/health
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
1. Visit your Vercel URL
2. Test the contact form
3. Test the book suggestion form
4. Test the visitor feedback form

### 4.3 Check Database
1. Go to MongoDB Atlas
2. Navigate to "Browse Collections"
3. You should see your data being saved

## 📁 Project Structure for Vercel

```
My Portfolio/
├── vercel.json                    # Vercel configuration (root level)
├── portfolio-backend/
│   ├── api/
│   │   └── index.js              # Vercel API handler
│   ├── server.js                 # Express server
│   ├── package.json              # Backend dependencies
│   └── .env                      # Backend environment variables
├── portfolio-frontend/
│   ├── src/                      # React source code
│   ├── public/                   # Static assets
│   ├── package.json              # Frontend dependencies
│   └── .env                      # Frontend environment variables
└── README.md
```

## 🛠️ Troubleshooting

### Common Issues:

1. **Build Failures**
   - Check the build logs in Vercel
   - Ensure all dependencies are in package.json
   - Verify the vercel.json configuration

2. **API Not Working**
   - Check environment variables in Vercel dashboard
   - Verify MongoDB connection string
   - Check function logs in Vercel

3. **CORS Errors**
   - Ensure CORS_ORIGIN is set correctly
   - Check that frontend and backend are on the same domain

4. **Database Connection Issues**
   - Verify your MongoDB connection string
   - Check if your IP is whitelisted in MongoDB Atlas
   - Ensure database user has correct permissions

## 🔄 Updating Your Deployment

### Automatic Deployments
- Push changes to your GitHub repository
- Vercel will automatically redeploy
- Check the deployment status in your Vercel dashboard

### Manual Redeployments
1. Go to your Vercel project dashboard
2. Click "Redeploy" button
3. Wait for deployment to complete

## 📊 Vercel Features

### Free Tier Benefits:
- **Unlimited deployments**
- **Automatic HTTPS**
- **Global CDN**
- **Serverless functions**
- **Custom domains** (with verification)

### Performance:
- **Edge Network** - Global CDN for fast loading
- **Serverless Functions** - Automatic scaling
- **Image Optimization** - Automatic image optimization

## 🔒 Security Notes

1. **Environment Variables**: Never commit `.env` files
2. **MongoDB Security**: Use strong passwords and restrict network access
3. **API Keys**: Store sensitive data in Vercel environment variables
4. **CORS**: Configure CORS properly for production

## 📞 Support

If you encounter issues:
1. Check Vercel deployment logs
2. Verify environment variables
3. Test locally first
4. Check MongoDB Atlas connection
5. Review Vercel documentation

Your portfolio should now be fully deployed and accessible online! 🎉

## 🎯 Next Steps

1. **Custom Domain**: Add a custom domain in Vercel settings
2. **Analytics**: Set up Vercel Analytics for performance monitoring
3. **SEO**: Optimize meta tags and sitemap
4. **Performance**: Monitor Core Web Vitals
5. **Backup**: Set up database backups in MongoDB Atlas 