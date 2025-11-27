# Heroku Deployment Guide

## Prerequisites
- Heroku CLI installed ([download](https://devcenter.heroku.com/articles/heroku-cli))
- Git installed
- Heroku account

## Step 1: Login ke Heroku
```bash
heroku login
```

## Step 2: Create Heroku App
```bash
heroku create nama-app-anda
```
Ganti `nama-app-anda` dengan nama unik untuk aplikasi Anda.

## Step 3: Setup Database di Heroku
Anda memiliki beberapa pilihan untuk database:

### Option A: Menggunakan JawsDB MySQL (Recommended)
```bash
# Add JawsDB add-on
heroku addons:create jawsdb-mysql:kitefin -a nama-app-anda

# Verify
heroku addons -a nama-app-anda
```

### Option B: Menggunakan PostgreSQL
```bash
heroku addons:create heroku-postgresql:mini -a nama-app-anda
```

## Step 4: Set Environment Variables
```bash
# Set Production Database URL (automatically set by add-on)
# Jika menggunakan JawsDB, URL sudah otomatis set sebagai JAWSDB_URL
# Anda perlu add ke config:
heroku config:set DATABASE_URL=$(heroku config:get JAWSDB_URL) -a nama-app-anda

# Set SECRET_KEY
heroku config:set SECRET_KEY=your-secret-key-here -a nama-app-anda

# Set FLASK_ENV
heroku config:set FLASK_ENV=production -a nama-app-anda

# Verify
heroku config -a nama-app-anda
```

## Step 5: Deploy ke Heroku
```bash
# Push code ke Heroku
git push heroku main

# Atau jika branch-nya bukan main
git push heroku your-branch:main
```

## Step 6: Initialize Database (First Time Only)
```bash
# Run init_db.py di server Heroku
heroku run python init_db.py -a nama-app-anda
```

## Step 7: Check Application Status
```bash
# View logs
heroku logs --tail -a nama-app-anda

# Open application
heroku open -a nama-app-anda
```

## Useful Heroku Commands

```bash
# View app config
heroku config -a nama-app-anda

# Update config variable
heroku config:set KEY=value -a nama-app-anda

# Remove config variable
heroku config:unset KEY -a nama-app-anda

# View logs
heroku logs --tail -a nama-app-anda

# Scale dynos
heroku ps:scale web=1 -a nama-app-anda

# View process status
heroku ps -a nama-app-anda

# Run migrations/commands
heroku run python init_db.py -a nama-app-anda

# Access console
heroku run python -a nama-app-anda

# View database
heroku addons:open jawsdb -a nama-app-anda
```

## Troubleshooting

### H14 Error (No web processes running)
```bash
heroku ps -a nama-app-anda
heroku ps:scale web=1 -a nama-app-anda
```

### Database Connection Error
```bash
# Check DATABASE_URL
heroku config:get DATABASE_URL -a nama-app-anda

# Verify database is running
heroku addons -a nama-app-anda
```

### View Application Errors
```bash
heroku logs --tail -a nama-app-anda
```

### Restart Application
```bash
heroku restart -a nama-app-anda
```

## Git Setup for Deployment

Make sure your repository is initialized:
```bash
git init
git add .
git commit -m "Initial commit"
```

If Heroku remote doesn't exist:
```bash
heroku git:remote -a nama-app-anda
```

## Database URL Format
- **JawsDB MySQL**: `mysql+pymysql://username:password@host:port/database`
- **PostgreSQL**: `postgresql://username:password@host:port/database`

## Post-Deployment Checklist
- [ ] Database created and initialized
- [ ] Environment variables configured
- [ ] Application deployed successfully
- [ ] Health check endpoint working (`/api/health`)
- [ ] Logs show no errors
- [ ] Database tables created properly

## Next Steps
1. Test API endpoints
2. Monitor logs regularly
3. Setup automatic scaling if needed
4. Configure custom domain
5. Setup CI/CD pipeline for automatic deployments
