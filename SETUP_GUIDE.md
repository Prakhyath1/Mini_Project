# AI Timetable Generator - Complete Setup Guide

## Overview
This is a full-stack application for generating optimized timetables using a Genetic Algorithm. It consists of:
- **Frontend**: Next.js React application with a clean UI
- **Backend**: FastAPI Python server with GA algorithm

## Prerequisites
- Node.js 18+ (for frontend)
- Python 3.9+ (for backend)
- npm or yarn (for Node.js package management)
- pip (for Python package management)

---

## Part 1: Frontend Setup (Next.js)

### Step 1: Install Node.js Dependencies
\`\`\`bash
npm install
# or
yarn install
\`\`\`

### Step 2: Run the Frontend Development Server
\`\`\`bash
npm run dev
# or
yarn dev
\`\`\`

The frontend will be available at `http://localhost:3000`

### Step 3: Build for Production
\`\`\`bash
npm run build
npm start
\`\`\`

---

## Part 2: Backend Setup (FastAPI)

### Step 1: Create a Python Virtual Environment
\`\`\`bash
# On Windows
python -m venv venv
venv\Scripts\activate

# On macOS/Linux
python3 -m venv venv
source venv/bin/activate
\`\`\`

### Step 2: Install Python Dependencies
\`\`\`bash
pip install fastapi uvicorn sqlalchemy pytest
\`\`\`

### Step 3: Run the Backend Server
\`\`\`bash
python app/main.py
\`\`\`

The backend will be available at `http://localhost:8000`

### Step 4: Access API Documentation
Once the backend is running, visit `http://localhost:8000/docs` for interactive API documentation

---

## Part 3: Running Both Together

### Option 1: Two Terminal Windows (Recommended for Development)

**Terminal 1 - Frontend:**
\`\`\`bash
npm run dev
# Runs on http://localhost:3000
\`\`\`

**Terminal 2 - Backend:**
\`\`\`bash
# Activate virtual environment first
source venv/bin/activate  # macOS/Linux
# or
venv\Scripts\activate  # Windows

# Run backend
python app/main.py
# Runs on http://localhost:8000
\`\`\`

### Option 2: Using Docker (Optional)

Create a `docker-compose.yml` file:
\`\`\`yaml
version: '3.8'
services:
  frontend:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NEXT_PUBLIC_API_URL=http://localhost:8000
  
  backend:
    build:
      context: .
      dockerfile: Dockerfile.backend
    ports:
      - "8000:8000"
\`\`\`

Then run:
\`\`\`bash
docker-compose up
\`\`\`

---

## Part 4: How to Use the Application

### Adding Teachers
1. Open `http://localhost:3000` in your browser
2. Enter teacher details:
   - **Teacher Name**: Full name of the teacher
   - **Subject**: Subject they teach
   - **Credits per Week**: Number of classes per week (1-8)
3. Click "Add Teacher"
4. Repeat for all teachers

### Generating Timetable
1. After adding all teachers, click "Generate Timetable"
2. The system will:
   - Create 50 random timetable solutions
   - Run 100 generations of the Genetic Algorithm
   - Optimize for constraints (no teacher conflicts, balanced load, etc.)
   - Display the best solution

### Understanding the Timetable
- **Days**: Monday through Friday (rows)
- **Times**: 9:00 AM to 4:00 PM (columns)
- **Red Cells**: Lunch break (12:00 PM) - not available for classes
- **Empty Cells**: Available slots with no class scheduled
- **Filled Cells**: Show teacher name and subject

---

## Part 5: Allocation Strategy

The Genetic Algorithm uses a **round-robin allocation strategy**:

1. **Priority Order**: 
   - Monday Period 1 → Tuesday Period 1 → Wednesday Period 1 → Thursday Period 1 → Friday Period 1
   - Then Monday Period 2 → Tuesday Period 2 → ... and so on

2. **Constraints Enforced**:
   - No teacher teaches more than 2 classes per day
   - No consecutive classes for the same teacher
   - Lunch break (12:00 PM) is always reserved
   - Each teacher gets exactly their required credits per week

3. **Optimization**:
   - Fitness function penalizes constraint violations
   - Tournament selection picks best solutions
   - Crossover and mutation create new solutions
   - Elitism preserves the best solution each generation

---

## Part 6: Testing

### Run Frontend Tests
\`\`\`bash
npm test
\`\`\`

### Run Backend Tests
\`\`\`bash
pytest
# or for verbose output
pytest -v
\`\`\`

---

## Part 7: Troubleshooting

### Frontend won't connect to backend
- Ensure backend is running on `http://localhost:8000`
- Check that both services are on the correct ports
- Look for CORS errors in browser console

### Backend won't start
\`\`\`bash
# Check if port 8000 is already in use
# On Windows:
netstat -ano | findstr :8000

# On macOS/Linux:
lsof -i :8000

# Kill the process if needed and restart
\`\`\`

### Python virtual environment issues
\`\`\`bash
# Deactivate current environment
deactivate

# Remove and recreate
rm -rf venv  # macOS/Linux
rmdir /s venv  # Windows

# Create fresh environment
python -m venv venv
source venv/bin/activate  # macOS/Linux
# or
venv\Scripts\activate  # Windows

pip install -r requirements.txt
\`\`\`

### Port conflicts
- Frontend: Change port with `npm run dev -- -p 3001`
- Backend: Change port in `app/main.py` (uvicorn.run(..., port=8001))

---

## Part 8: Project Structure

\`\`\`
project/
├── app/
│   ├── page.tsx              # Main frontend component
│   ├── layout.tsx            # Layout wrapper
│   ├── globals.css           # Global styles
│   ├── api/
│   │   └── generate/
│   │       └── route.ts      # API endpoint for timetable generation
│   ├── main.py               # FastAPI backend server
│   ├── models.py             # Database models
│   ├── database.py           # Database setup
│   ├── schemas.py            # Pydantic schemas
│   ├── genetic_algorithm.py  # GA implementation
│   └── constraints.py        # Constraint validation
├── components/
│   └── ui/                   # shadcn/ui components
├── tests/
│   ├── test_genetic_algorithm.py
│   ├── test_constraints.py
│   └── test_api.py
├── public/                   # Static assets
├── package.json              # Node.js dependencies
├── tsconfig.json             # TypeScript config
└── SETUP_GUIDE.md           # This file
\`\`\`

---

## Part 9: Environment Variables

### Frontend (.env.local)
\`\`\`
NEXT_PUBLIC_API_URL=http://localhost:8000
\`\`\`

### Backend (.env)
\`\`\`
DATABASE_URL=sqlite:///./timetable.db
\`\`\`

---

## Part 10: Deployment

### Deploy Frontend to Vercel
\`\`\`bash
npm install -g vercel
vercel
\`\`\`

### Deploy Backend to Heroku
\`\`\`bash
heroku login
heroku create your-app-name
git push heroku main
\`\`\`

---

## Support & Documentation

- **Next.js Docs**: https://nextjs.org/docs
- **FastAPI Docs**: https://fastapi.tiangolo.com
- **Genetic Algorithm**: https://en.wikipedia.org/wiki/Genetic_algorithm

---

## License
MIT License - Feel free to use and modify as needed.
