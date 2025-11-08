# Vercel Deployment Guide

## Prerequisites

1. **Vercel Account**: Sign up at [vercel.com](https://vercel.com)
2. **Vercel CLI** (optional): `npm i -g vercel`
3. **Database**: A PostgreSQL database (recommended: [Neon](https://neon.tech) or Vercel Postgres)
4. **Firebase Project**: Set up Firebase Authentication and get credentials

## Environment Variables

Before deploying, you need to set up the following environment variables in your Vercel project:

### Required Environment Variables:

1. **Database**
   - `DATABASE_URL` - Your PostgreSQL connection string

2. **Firebase Client** (for frontend)
   - `VITE_FIREBASE_API_KEY`
   - `VITE_FIREBASE_AUTH_DOMAIN`
   - `VITE_FIREBASE_PROJECT_ID`
   - `VITE_FIREBASE_STORAGE_BUCKET`
   - `VITE_FIREBASE_MESSAGING_SENDER_ID`
   - `VITE_FIREBASE_APP_ID`

3. **Firebase Admin** (for backend)
   - `FIREBASE_ADMIN_CREDENTIALS` - JSON string of your service account credentials

4. **Server Configuration**
   - `NODE_ENV=production`

See `.env.example` for the complete list.

## Deployment Steps

### Option 1: Deploy via Vercel Dashboard (Recommended)

1. **Connect Repository**
   - Go to [vercel.com/new](https://vercel.com/new)
   - Import your Git repository (GitHub, GitLab, or Bitbucket)

2. **Configure Project**
   - Framework Preset: **Other**
   - Root Directory: `./` (leave as default)
   - Build Command: `npm run build` (should auto-detect)
   - Output Directory: `dist/public` (should auto-detect from vercel.json)

3. **Add Environment Variables**
   - In the project settings, add all environment variables listed above
   - Make sure to add them to **Production**, **Preview**, and **Development** environments

4. **Deploy**
   - Click "Deploy"
   - Wait for the build to complete

### Option 2: Deploy via CLI

```bash
# Install Vercel CLI globally
npm i -g vercel

# Login to Vercel
vercel login

# Deploy to production
vercel --prod

# Follow the prompts to link your project
```

After deployment, add environment variables:

```bash
# Add environment variables via CLI
vercel env add DATABASE_URL production
vercel env add VITE_FIREBASE_API_KEY production
# ... add all other environment variables

# Redeploy to apply environment variables
vercel --prod
```

## Database Setup

1. **Create Database**
   - Recommended: [Neon PostgreSQL](https://neon.tech) (free tier available)
   - Or use [Vercel Postgres](https://vercel.com/docs/storage/vercel-postgres)

2. **Get Connection String**
   - Copy your DATABASE_URL from your database provider

3. **Push Schema**
   ```bash
   # Set DATABASE_URL in your local .env file
   echo "DATABASE_URL=your_connection_string" > .env
   
   # Push database schema
   npm run db:push
   ```

## Important Limitations on Vercel

⚠️ **WebSockets**: This application uses WebSockets for real-time features. Vercel's serverless architecture has limitations with WebSockets:
- WebSockets may not work as expected on Vercel
- Real-time features (live mining updates, notifications) may be degraded

⚠️ **Background Jobs**: Background jobs (mining auto-collection, etc.) won't run continuously on Vercel's serverless platform.

### Recommended Alternative Platforms for Full Functionality:

If you need full WebSocket and background job support, consider these platforms:
- **Railway** ([railway.app](https://railway.app))
- **Render** ([render.com](https://render.com))
- **DigitalOcean App Platform** ([digitalocean.com](https://digitalocean.com))
- **Fly.io** ([fly.io](https://fly.io))

These platforms support long-running processes and WebSockets out of the box.

## Troubleshooting

### Build Fails
- Check that all environment variables are set correctly
- Ensure `DATABASE_URL` is accessible from Vercel's network
- Review build logs in the Vercel dashboard

### Runtime Errors
- Check function logs in Vercel dashboard
- Verify all environment variables are set in production
- Ensure database schema is up to date (`npm run db:push`)

### Database Connection Issues
- Make sure your database allows connections from Vercel's IP ranges
- For Neon: Enable "Allow all" in connection settings
- Check that DATABASE_URL format is correct

### Firebase Authentication Issues
- Verify all Firebase environment variables are set
- Check Firebase project settings and ensure the domain is authorized
- Make sure `FIREBASE_ADMIN_CREDENTIALS` is valid JSON

## Post-Deployment

1. **Test the Application**
   - Visit your deployed URL
   - Test user authentication
   - Verify database operations work

2. **Set up Custom Domain** (Optional)
   - Go to your Vercel project settings
   - Add your custom domain
   - Update Firebase authorized domains to include your custom domain

3. **Monitor Performance**
   - Use Vercel Analytics to monitor your application
   - Check function execution logs for errors

## Support

For issues related to:
- Vercel deployment: [Vercel Docs](https://vercel.com/docs)
- Database: Your database provider's documentation
- Firebase: [Firebase Docs](https://firebase.google.com/docs)
