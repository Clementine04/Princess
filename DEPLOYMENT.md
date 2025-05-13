# Deploying to PythonAnywhere

This guide walks you through deploying the Study Assistant application on PythonAnywhere.

## 1. Set Up GitHub Repository

1. Create a new GitHub repository
   - Go to github.com and log in
   - Click "New" to create a new repository
   - Name it (e.g., "study-assistant")
   - Choose visibility (public or private)
   - Click "Create repository"

2. Push your code to GitHub
   ```
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin https://github.com/your-username/study-assistant.git
   git push -u origin main
   ```

## 2. Set Up PythonAnywhere Account

1. Create a PythonAnywhere account at https://www.pythonanywhere.com/ (if you don't have one)
2. Log in to your account

## 3. Set Up a Web App on PythonAnywhere

1. Go to the Dashboard and click on "Web" tab
2. Click on "Add a new web app"
3. Choose "Manual Configuration"
4. Select Python version (3.8 or newer recommended)
5. Click "Next" to create the web app

## 4. Clone Your Repository

1. Open a Bash console from the PythonAnywhere dashboard
2. Clone your GitHub repository:
   ```
   cd ~
   git clone https://github.com/your-username/study-assistant.git
   ```

## 5. Set Up a Virtual Environment

1. Create and activate a virtual environment:
   ```
   cd ~/study-assistant
   python -m venv venv
   source venv/bin/activate
   ```

2. Install the required packages:
   ```
   pip install -r requirements.txt
   ```

## 6. Configure the Web App

1. Go back to the "Web" tab in PythonAnywhere
2. Set the following in the "Code" section:
   - Source code: `/home/yourusername/study-assistant`
   - Working directory: `/home/yourusername/study-assistant`

3. Set up the WSGI configuration file:
   - Click on the WSGI configuration file link (e.g., `/var/www/yourusername_pythonanywhere_com_wsgi.py`)
   - Replace the content with:

   ```python
   import sys
   import os
   
   # Add your project directory to the sys.path
   path = '/home/yourusername/study-assistant'
   if path not in sys.path:
       sys.path.insert(0, path)
   
   # Set environment variables
   os.environ['GOOGLE_GENAI_API_KEY'] = 'your_api_key_here'
   
   # Import the Flask app object
   from app import app as application
   ```

4. Save the file

## 7. Set Up Static Files (Optional but Recommended)

1. In the Web tab, go to "Static Files"
2. Add a new mapping:
   - URL: `/static/`
   - Directory: `/home/yourusername/study-assistant/static`

## 8. Configure Environment Variables

1. From the PythonAnywhere dashboard, open a Bash console
2. Create a `.env` file in your project directory:
   ```
   cd ~/study-assistant
   echo "GOOGLE_GENAI_API_KEY=your_api_key_here" > .env
   ```

## 9. Initialize the Database

1. From the PythonAnywhere Bash console:
   ```
   cd ~/study-assistant
   python
   ```

2. In the Python console:
   ```python
   from app import app, db
   with app.app_context():
       db.create_all()
   exit()
   ```

## 10. Reload and Test Your Web App

1. Go back to the "Web" tab
2. Click the "Reload" button
3. Visit your web app URL (should be something like `yourusername.pythonanywhere.com`)

## 11. Updating Your Application

When you make changes to your code, push them to GitHub, then:

1. Open a Bash console on PythonAnywhere
2. Navigate to your project directory:
   ```
   cd ~/study-assistant
   ```
3. Pull the changes:
   ```
   git pull
   ```
4. Reload your web app from the "Web" tab

## Troubleshooting

If your application doesn't work:

1. Check the error logs in the "Web" tab
2. Ensure all dependencies are installed
3. Make sure your database is properly initialized
4. Verify environment variables are correctly set
5. Check PythonAnywhere's help documentation for specific issues 