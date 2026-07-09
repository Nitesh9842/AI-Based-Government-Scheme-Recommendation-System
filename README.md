 # SchemeAssist AI

 Government scheme recommendation engine that helps citizens find and compare government benefit schemes they are eligible for.

 ## Table of contents
 - [Project overview](#project-overview)
 - [Features](#features)
 - [Architecture & key files](#architecture--key-files)
 - [Quickstart](#quickstart)
 - [API reference](#api-reference)
 - [Data format](#data-format)
 - [Development & tests](#development--tests)
 - [Contributing](#contributing)
 - [License](#license)
 - [Contact](#contact)

 ## Project overview

 SchemeAssist AI (Project ID: AID105) is a lightweight Flask-based backend paired with a static frontend to recommend government schemes to users using eligibility rules and a scoring engine.

 The repository layout (top-level) is:

 ```
 aid105-Nitesh9842/
 ├── backend/          # Flask API, storage helpers and recommender logic
 ├── data/             # CSV dataset: combined_schemes.csv
 ├── frontend/         # Static UI (HTML/JS/CSS)
 ├── reports/          # Report generation scripts
 ├── tests/            # Unit tests
 └── SETUP_GUIDE.md    # Additional setup notes
 ```

 ## Features

 - Personalized scheme recommendations with eligibility scoring
 - Scheme search and filtering
 - Compare up to 4 schemes with side-by-side insights
 - Track favorites and application status (stored in JSON files)
 - Export recommendation results (JSON / CSV)
 - Simple user registration/login and profile storage (file-based tokens)

 ## Architecture & key files

 - Backend Flask app: [backend/app.py](backend/app.py)
 - Recommendation engine and scoring: [backend/recommender.py](backend/recommender.py)
 - Schemes dataset: [data/combined_schemes.csv](data/combined_schemes.csv)
 - Frontend entry: [frontend/index.html](frontend/index.html)
 - Tests: [tests/test_recommender.py](tests/test_recommender.py)
 - Setup guide: [SETUP_GUIDE.md](SETUP_GUIDE.md)

 ## Quickstart

 Prerequisites: Python 3.8+ and pip.

 1) Create and activate a virtual environment (recommended):

 ```bash
 python -m venv .venv
 # Windows
 .venv\Scripts\activate
 # macOS / Linux
 source .venv/bin/activate
 ```

 2) Install dependencies:

 ```bash
 pip install -r backend/requirements.txt
 ```

 3) Run the backend:

 ```bash
 python backend/app.py
 ```

 The API server will start on http://localhost:5000 by default. The static frontend files are served by Flask; open http://localhost:5000 in a browser to use the UI.

 If you prefer to open the frontend statically, open [frontend/index.html](frontend/index.html) in your browser (some features require the API server).

 ## API reference

 The backend exposes a REST API. Main endpoints (all under `/api`):

 - `GET /api/health` — health check
 - `POST /api/register` — register a new user
 - `POST /api/login` — login and receive a token
 - `GET|POST /api/profile` — read/update profile (requires `Authorization` header with token)
 - `POST /api/recommend` — request scheme recommendations (pass user profile fields)
 - `POST /api/search` — search schemes with filters
 - `POST /api/compare` — compare multiple schemes (max 4)
 - `GET /api/statistics` — aggregated scheme statistics
 - `GET|POST|DELETE /api/favorites` — manage favorites (query param `user_id` optional)
 - `GET|POST|PUT /api/applications` — track application status per user
 - `POST /api/export` — export selected schemes (json or csv)
 - `POST /api/feedback` — submit user feedback

 Example: request recommendations with curl:

 ```bash
 curl -X POST http://localhost:5000/api/recommend \
   -H "Content-Type: application/json" \
   -d '{"state":"Karnataka","income":150000,"category":"Education","age":28}'
 ```

 Authentication: The simple file-based auth issues a token on login; include it in `Authorization` header for protected endpoints, e.g. `Authorization: <token>`.

 ## Data format

 Schemes are stored in [data/combined_schemes.csv](data/combined_schemes.csv). Key columns expected by the recommender include (but are not limited to):

 - `scheme_id`, `scheme_name`, `state`, `category`, `min_income`, `max_income`, `min_age`, `max_age`, `target_group`, `benefits`, `is_active`, `last_updated`, `level`

 The recommender filters inactive schemes (`is_active != "Yes"`) and computes an eligibility `score` (0–100) based on income, age, category, caste targets and state.

 ## Development & tests

 - Run tests (pytest):

 ```bash
 pip install pytest
 pytest -q
 ```

 - Linting / formatting: use your preferred tools (flake8/black) as desired.

 - The recommender logic lives in [backend/recommender.py](backend/recommender.py). Unit tests that validate core scoring are in [tests/test_recommender.py](tests/test_recommender.py).

 ## Contributing

 Contributions are welcome. Typical workflow:

 1. Fork the repository and create a feature branch.
 2. Run tests and add/update tests for new behavior.
 3. Open a pull request describing the change.

 Before changing public APIs, please open an issue to discuss the design.

 ## Security & caveats

 - This project stores user data and tokens in JSON files for simplicity. For production use, migrate to a proper database and secure authentication (hashed tokens, TLS, session management).
 - Validate and sanitize any user-provided data before trusting it in production.

 ## License

 This repository includes a top-level `LICENSE` file. See it for licensing terms.

 ## Contact

 Project author: Nitesh

 If you need help running the project, check [SETUP_GUIDE.md](SETUP_GUIDE.md) or open an issue.

 ---
 Generated README — updated to include setup, API, and development notes.
