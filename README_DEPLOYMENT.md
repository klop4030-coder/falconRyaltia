# Deployment Guide for License Launcher System

This guide explains how to deploy the **Backend (API)** and **Frontend (Admin Dashboard)** of the License Launcher system.

## System Overview
- **Backend**: Python (FastAPI) - Handles logic, database, and API requests.
- **Frontend**: React (Vite) - The admin interface for managing licenses.
- **Database**: SQLite (`licenses.db`) - Embedded database file.

---

## 1. Backend Deployment

### Prerequisites
- Python 3.10 or higher installed on the server.

### Steps
1.  **Navigate to the backend directory**:
    ```bash
    cd backend
    ```

2.  **Install Dependencies**:
    It is recommended to use a virtual environment.
    ```bash
    # Linux/Mac
    python3 -m venv venv
    source venv/bin/activate

    # Windows
    python -m venv venv
    venv\Scripts\activate
    ```
    Then install the requirements:
    ```bash
    pip install -r requirements.txt
    ```

3.  **Run the Server**:
    For production, use `uvicorn` or `gunicorn`.
    ```bash
    # Run on port 8000
    uvicorn main:app --host 0.0.0.0 --port 8000
    ```
    To run in the background (daemonized), use a process manager like `supervisor`, `systemd`, or `pm2`.

4.  **Important Note on Database**:
    The system uses a local SQLite file (`licenses.db`) inside the `backend` folder.
    - **Backups**: Ensure this file is backed up regularly.
    - **Persistence**: If deploying to a platform like Heroku/Render (Free Tier), *the file system is ephemeral* (resets on restart), so your data will be lost. For those platforms, you must switch to a persistent database like PostgreSQL. **For a standard VPS or dedicated server, SQLite works fine.**

---

## 2. Frontend Deployment

### Prerequisites
- Node.js (v18+) and npm installed.

### Steps
1.  **Navigate to the dashboard directory**:
    ```bash
    cd admin-dashboard
    ```

2.  **Install Dependencies**:
    ```bash
    npm install
    ```

3.  **Configure API URL**:
    Create a file named `.env` in the `admin-dashboard` folder (copy from `.env.example`).
    Set the `VITE_API_URL` to your live backend address.
    
    **Example `.env` file:**
    ```env
    VITE_API_URL=http://your-server-ip:8000
    # OR if using a domain
    VITE_API_URL=https://api.yourdomain.com
    ```

4.  **Build the Project**:
    This compiles the React code into static HTML/CSS/JS files.
    ```bash
    npm run build
    ```
    This will create a `dist` folder.

5.  **Serve the Frontend**:
    Upload the contents of the `dist` folder to your web server (Apache, Nginx, cPanel public_html, or static hosting like Vercel/Netlify).

    **If using Vercel/Netlify (Recommended for Frontend):**
    - Connect the repository.
    - Set the Build Command to `npm run build`.
    - Set Output Directory to `dist`.
    - Add the `VITE_API_URL` environment variable in the hosting dashboard.

---

## 3. Verify Connection
1.  Open the frontend URL (e.g., `http://your-domain.com`).
2.  The dashboard should load.
3.  If you see "Error fetching licenses", check:
    - Is the Backend running?
    - Is the `VITE_API_URL` correct?
    - Check the browser console (F12) for CORS errors (The backend is currently configured to allow all origins `*`, so this shouldn't be an issue).

---

## Support
For any issues, please contact the developer.
