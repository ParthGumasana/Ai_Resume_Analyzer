# AI Resume Analyzer

This project provides an AI-powered resume analyzer with a FastAPI/Socket.IO backend and a simple HTML/JavaScript frontend for testing. The backend analyzes resumes against job descriptions, provides a compatibility score, offers suggestions, allows for resume editing and PDF generation, and helps with interview preparation.

## Features

* **Resume Analysis:** Upload a PDF resume and a job description to get a compatibility score and tailored suggestions.
* **Resume Editing:** Edit the extracted resume text based on AI suggestions.
* **PDF Generation:** Generate a new downloadable PDF from the edited resume content.
* **Interview Preparation:** Receive common interview questions and tips based on the job description.
* **Real-time Communication:** Uses Socket.IO for instant feedback and updates.

## Tech Stack

* **Backend:** Python (FastAPI, python-socketio, PyMuPDF, ReportLab, Google Generative AI)
* **Frontend (Testing):** HTML, CSS (TailwindCSS), JavaScript (Socket.IO client)
* **Deployment:** Docker (for containerization)

## Project Structure


.
├── main.py                 # FastAPI backend application
├── index.html              # Frontend HTML for testing (simulates Laravel client)
├── requirements.txt        # Python dependencies for the backend
├── .env.example            # Example for environment variables
├── Dockerfile              # Dockerfile for containerizing the backend
└── README.md               # This documentation file


## Setup and Local Development

Follow these steps to set up and run the project locally.

### Prerequisites

* Python 3.8+
* pip (Python package installer)
* Docker (optional, for containerized deployment)
* A Google Gemini API Key: Obtain one from [Google AI Studio](https://aistudio.google.com/app/apikey).

### 1. Backend Setup

1.  **Clone the repository (or create files manually):**
    Create a new directory and save `main.py`, `requirements.txt`, `.env.example`, and `Dockerfile` into it.
2.  **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: `venv\Scripts\activate`
    ```
3.  **Install Python dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
4.  **Configure Environment Variables:**
    Create a `.env` file in the same directory as `main.py` by copying `.env.example`:
    ```bash
    cp .env.example .env
    ```
    Open the `.env` file and replace `YOUR_GEMINI_API_KEY_HERE` with your actual Google Gemini API Key:
    ```
    GEMINI_API_KEY="your_actual_gemini_api_key"
    ```
    *(Optional: You can also define `SERVER_HOST` and `SERVER_PORT` if you want to override defaults.)*
5.  **Run the Backend Server:**
    ```bash
    uvicorn main:app_with_sio --host 0.0.0.0 --port 5000 --reload --log-level info
    ```
    The `--reload` flag is useful for local development as it restarts the server on code changes. For production, you'd typically remove `--reload`.
    The backend should now be running on `http://127.0.0.1:5000`.

### 2. Frontend Setup (for Testing)

1.  **Save `index.html`:**
    Save the `index.html` content into the same directory as your `main.py` (or a `public` directory if you're simulating a static frontend served by a web server).
2.  **Open in Browser:**
    Simply open the `index.html` file in your web browser.
    * **CORS Note:** If you encounter Cross-Origin Resource Sharing (CORS) issues when opening `index.html` directly from your file system (`file://` protocol), you might need to:
        * Serve `index.html` via a simple HTTP server (e.g., `python -m http.server 8000` from the directory where `index.html` is, then navigate to `http://localhost:8000`).
        * Use a browser extension to temporarily disable CORS (for testing only).
        * **For your actual Laravel frontend:** Ensure your Laravel application is served from a domain/port that is allowed in the `CORS_ALLOWED_ORIGINS` configuration in `main.py`.

## Using the Application

1.  **Start both the backend server and open the `index.html` in your browser.**
2.  **Upload Resume:** Click "Choose File" and select a PDF resume.
3.  **Enter Job Description:** Paste the text of the job description into the provided textarea.
4.  **Analyze:** Click "Analyze Resume." The application will show a compatibility score and suggestions.
5.  **Edit and Generate PDF:** The extracted resume text will appear in an editable area. Make changes, then click "Generate Updated PDF" to download a new PDF.
6.  **Interview Prep:** Click "Prepare for Interview" to get common interview questions and tips.

## Docker Deployment (for Production)

For a production environment, it's highly recommended to use Docker.

1.  **Build the Docker image:**
    Navigate to the directory containing `Dockerfile` and `main.py`.
    ```bash
    docker build -t resume-analyzer-backend .
    ```
2.  **Run the Docker container:**
    Replace `YOUR_GEMINI_API_KEY_HERE` with your actual API key.
    ```bash
    docker run -d --name resume-analyzer -p 5000:5000 -e GEMINI_API_KEY="YOUR_GEMINI_API_KEY_HERE" resume-analyzer-backend
    ```
    This will run the backend on port `5000` of your host machine.

3.  **Configure Laravel Frontend for Production:**
    In your Laravel application's JavaScript code (where you initialize `socket.io`), ensure the connection URL points to your deployed backend.
    For example, if your backend is deployed at `https://api.yourdomain.com`:
    ```javascript
    const socket = io('[https://api.yourdomain.com](https://api.yourdomain.com)');
    ```
    Also, remember to update the `allow_origins` in your `main.py` (FastAPI `CORSMiddleware` and `socketio.AsyncServer`) to explicitly include your Laravel frontend's domain (e.g., `https://www.yourdomain.com`).

## Future Enhancements (Production Ready Considerations)

* **Robust PDF Parsing:** Implement more advanced PDF parsing or OCR for highly complex resume layouts.
* **User Authentication:** Integrate with a robust authentication system (e.g., Laravel Breeze/Jetstream for the frontend, JWT for the backend).
* **Database Integration:** Store user resumes, analysis history, and personalized settings (e.g., PostgreSQL, MySQL).
* **Deployment Strategy:** Choose a suitable cloud provider (AWS, Google Cloud, Azure, Heroku) and set up CI/CD pipelines for automated deployments.
* **Scalability:** Consider load balancing and horizontal scaling for high traffic.
* **Logging and Monitoring:** Implement structured logging and integrate with monitoring tools.
* **Frontend Framework:** Fully build out the frontend using Laravel's capabilities, potentially with a modern JS framework (Vue, React) for a richer user experience.
* **Rate Limiting:** Protect your API from abuse by implementing rate limiting.
* **Error Handling:** More granular error handling and user-friendly error messages.
* **Testing:** Comprehensive unit and integration tests for both backend and frontend.

---
