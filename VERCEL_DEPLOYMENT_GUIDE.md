# Vercel Deployment Guide for Club Laptops E-commerce

This guide will help you deploy your e-commerce website to Vercel with a secure database setup.

## Prerequisites

- A Vercel account
- A PostgreSQL database (we recommend Neon, Supabase, or AWS RDS)
- Node.js and npm installed locally
- Git repository for your project

## Step 1: Database Setup

### Option A: Using Neon (Recommended for development)
1. Go to [Neon](https://neon.tech) and create an account
2. Create a new project
3. Copy your database connection string (it will look like: `postgresql://username:password@ep-...us-east-1.aws.neon.tech/neondb?sslmode=require`)

### Option B: Using Supabase
1. Go to [Supabase](https://supabase.com) and create a project
2. Create a new database
3. Copy your database connection string

### Option C: Using AWS RDS or other PostgreSQL providers
1. Set up your PostgreSQL instance
2. Note your connection string with credentials

## Step 2: Environment Variables Setup

Create a `.env` file in your project root with the following variables:

```env
DATABASE_URL="your_postgresql_connection_string"
SESSION_SECRET="your_secure_session_secret_here_must_be_at_least_32_characters_long"
FRONTEND_URL="https://your-project-name.vercel.app"
NODE_ENV="production"
```

> **Important Security Note**: 
> - Generate a strong SESSION_SECRET (at least 32 characters)
> - Never commit your `.env` file to version control
> - Store these values as secrets in Vercel (see Step 4)

## Step 3: Database Schema Setup

The application uses Drizzle ORM for database migrations. When you deploy to Vercel, the database schema will be automatically created and updated if needed. However, you can manually run migrations locally to test:

```bash
# Install dependencies
npm install

# Run the database migrations (optional, for local testing)
npm run db:push
```

## Step 4: Deploy to Vercel

The application will connect to your database upon startup. The database schema should be set up before or during deployment.

### Method 1: Using Vercel CLI (Recommended)

1. Install Vercel CLI:
```bash
npm install -g vercel
```

2. Login to your Vercel account:
```bash
vercel login
```

3. Navigate to your project directory and deploy:
```bash
vercel
```

4. When prompted, set the following environment variables in the Vercel dashboard:
   - DATABASE_URL: Your PostgreSQL connection string
   - SESSION_SECRET: Your secure session secret
   - FRONTEND_URL: Your Vercel project URL (e.g., `https://your-project-name.vercel.app`)

5. For production deployment:
```bash
vercel --prod
```

### Method 2: Connect Git Repository to Vercel

1. Push your code to a Git repository (GitHub, GitLab, or Bitbucket)
2. Go to [Vercel dashboard](https://vercel.com/dashboard)
3. Click "Add New..." and select "Project"
4. Import your Git repository
5. In the project settings, go to "Environment Variables" and add:
   - DATABASE_URL
   - SESSION_SECRET
   - FRONTEND_URL
6. Click "Deploy"

## Step 5: Configure Vercel Project Settings

After your initial deployment:

1. Go to your project dashboard in Vercel
2. Navigate to "Settings" → "Environment Variables"
3. Add the required environment variables if not done during deployment
4. In "Settings" → "Git", you can configure automatic deployments from your Git repository

## Step 6: Database Session Store Setup

The application uses PostgreSQL for session storage. During the first deployment, the session table will be created automatically. If you encounter any issues with sessions, make sure your database user has permissions to create tables.

## Step 7: Security Considerations

### SSL/HTTPS
- Vercel automatically provides SSL certificates for your domain
- The application is configured to use secure cookies in production

### Rate Limiting
- API endpoints are protected with rate limiting (50 requests per 15 minutes per IP)
- General requests are limited to 100 per 15 minutes per IP

### Input Sanitization
- All user inputs are sanitized to prevent XSS and NoSQL injection attacks
- Content Security Policy headers are implemented

### Authentication
- Secure session management with PostgreSQL-backed sessions
- Passwords are hashed using bcrypt with 12 salt rounds

## Step 8: Post-Deployment Tasks

### 1. Set up Custom Domain (Optional)
1. In your Vercel project dashboard, go to "Settings" → "Domains"
2. Add your custom domain
3. Follow the instructions to update your DNS settings

### 2. Database Connection Pooling
The application is configured with:
- Maximum 20 connections in the pool
- 30-second idle timeout
- 2-second connection timeout

### 3. Monitoring
- Check the Vercel logs for any errors after deployment
- Monitor your database connection usage
- Set up alerts for high error rates

## Troubleshooting

### Common Issues:

1. **Database Connection Errors**:
   - Ensure your DATABASE_URL is correct
   - Verify that your database provider allows connections from Vercel's IP ranges
   - Check that SSL is properly configured

2. **Database Schema Issues**:
   - If you encounter database schema errors after deployment, you may need to manually run migrations
   - Run `npm run db:push` locally to sync your database schema
   - Make sure your database user has permissions to create and modify tables

2. **Session Issues**:
   - Verify that SESSION_SECRET is set and consistent across deployments
   - Ensure your domain is properly configured for secure cookies

3. **Build Failures**:
   - Check that all dependencies are properly listed in package.json
   - Verify that the build process works locally before deploying

### Checking Application Health:
- Visit your deployed URL
- Check the Vercel logs for any startup errors
- Test the API endpoints to ensure database connectivity

## Environment Variables Reference

| Variable | Description | Required |
|----------|-------------|----------|
| DATABASE_URL | PostgreSQL connection string | Yes |
| SESSION_SECRET | Secret key for session encryption (min 32 chars) | Yes |
| FRONTEND_URL | URL of your frontend application | Yes |
| NODE_ENV | Set to "production" for production deployment | No (defaults to production) |

## Scaling Recommendations

For production usage:
- Monitor your database connection usage and scale accordingly
- Consider using a more robust database plan as traffic increases
- Implement caching for frequently accessed data
- Set up a CDN for static assets if needed

## Support

If you encounter issues with the deployment:
1. Check the Vercel deployment logs
2. Verify all environment variables are correctly set
3. Ensure your database is accessible from external connections
4. Review the application logs for specific error messages