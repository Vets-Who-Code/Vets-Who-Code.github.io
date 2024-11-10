# Vets Who Code - FastAPI Operations Guide 🎖️

## Mission Brief: FastAPI Introduction

FastAPI is like a highly efficient FOB (Forward Operating Base) for your web operations. It's:

- Fast (high-performance)
- Easy to deploy
- Built for modern Python operations
- Automatically documented
- Production-ready

## Phase 1: Base Setup

### Required Equipment

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

# Install dependencies
pip install fastapi uvicorn
```

### First Deployment

```python
# main.py
from fastapi import FastAPI

# Initialize FOB (Create FastAPI instance)
app = FastAPI(
    title="VWC API Operations",
    description="Veteran-focused API operations",
    version="1.0.0"
)

# First Checkpoint (Root endpoint)
@app.get("/")
async def base_check():
    return {
        "status": "operational",
        "message": "VWC API Standing By"
    }
```

### Launch Operations

```bash
# Start the server
uvicorn main:app --reload
```

## Phase 2: Basic Operations (Routes)

### Personnel Management System

```python
from typing import List, Optional
from pydantic import BaseModel
from datetime import date

# Data Models (Intel Structure)
class ServiceMember(BaseModel):
    id: int
    first_name: str
    last_name: str
    rank: str
    branch: str
    service_start: date
    status: str = "Active"

# Database Simulation
db_personnel = []

# Routes
@app.post("/personnel/", response_model=ServiceMember)
async def create_personnel(member: ServiceMember):
    db_personnel.append(member)
    return member

@app.get("/personnel/", response_model=List[ServiceMember])
async def get_all_personnel():
    return db_personnel

@app.get("/personnel/{member_id}", response_model=ServiceMember)
async def get_personnel(member_id: int):
    for member in db_personnel:
        if member.id == member_id:
            return member
    raise HTTPException(status_code=404, detail="Service member not found")
```

## Phase 3: Advanced Operations

### Authentication & Security

```python
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from jose import JWTError, jwt
from passlib.context import CryptContext
from datetime import datetime, timedelta

# Security Configuration
SECRET_KEY = "your-secret-key"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

# User Model
class User(BaseModel):
    username: str
    full_name: Optional[str] = None
    disabled: Optional[bool] = None

# Token Generation
def create_access_token(data: dict):
    to_encode = data.copy()
    expire = datetime.utcnow() + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt

# Authentication Route
@app.post("/token")
async def login(form_data: OAuth2PasswordRequestForm = Depends()):
    # Authentication logic here
    access_token = create_access_token(data={"sub": form_data.username})
    return {"access_token": access_token, "token_type": "bearer"}
```

### Award System API

```python
from enum import Enum

class AwardType(str, Enum):
    PURPLE_HEART = "Purple Heart"
    BRONZE_STAR = "Bronze Star"
    COMBAT_ACTION = "Combat Action Ribbon"

class Award(BaseModel):
    type: AwardType
    recipient_id: int
    award_date: date
    citation: Optional[str] = None

@app.post("/awards/")
async def issue_award(award: Award, token: str = Depends(oauth2_scheme)):
    # Award issuing logic
    return {"status": "Award issued", "award": award}

@app.get("/personnel/{member_id}/awards")
async def get_member_awards(member_id: int, token: str = Depends(oauth2_scheme)):
    # Award retrieval logic
    return {"member_id": member_id, "awards": []}
```

## Phase 4: Database Integration

### Using SQLAlchemy

```python
from sqlalchemy import create_engine, Column, Integer, String, Date
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

SQLALCHEMY_DATABASE_URL = "sqlite:///./vwc_operations.db"
engine = create_engine(SQLALCHEMY_DATABASE_URL)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
Base = declarative_base()

# Database Models
class DBServiceMember(Base):
    __tablename__ = "service_members"

    id = Column(Integer, primary_key=True, index=True)
    first_name = Column(String)
    last_name = Column(String)
    rank = Column(String)
    branch = Column(String)
    service_start = Column(Date)
    status = Column(String)

# Database Dependency
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

# Updated Routes with Database
@app.post("/personnel/", response_model=ServiceMember)
async def create_personnel(
    member: ServiceMember,
    db: Session = Depends(get_db)
):
    db_member = DBServiceMember(**member.dict())
    db.add(db_member)
    db.commit()
    db.refresh(db_member)
    return db_member
```

## Phase 5: Background Tasks

### Mission Status Updates

```python
from fastapi import BackgroundTasks

def update_mission_status(mission_id: str):
    # Simulate long-running task
    time.sleep(5)
    # Update mission status logic here
    print(f"Mission {mission_id} status updated")

@app.post("/missions/{mission_id}/status")
async def update_status(
    mission_id: str,
    background_tasks: BackgroundTasks
):
    background_tasks.add_task(update_mission_status, mission_id)
    return {"message": "Status update initiated"}
```

## Field Exercise: Complete API Implementation

Here's a practical exercise combining these concepts - a Veteran Service Record System:

```python
# Complete implementation example
from fastapi import FastAPI, HTTPException, Depends, BackgroundTasks
from typing import List, Optional
from pydantic import BaseModel
from datetime import date
import asyncio

app = FastAPI(title="Veteran Service Record System")

# Models
class ServiceRecord(BaseModel):
    id: int
    name: str
    service_number: str
    branch: str
    rank: str
    start_date: date
    end_date: Optional[date] = None
    status: str
    awards: List[str] = []

# Simulated Database
records_db = {}

# Routes
@app.post("/records/", response_model=ServiceRecord)
async def create_record(record: ServiceRecord):
    if record.id in records_db:
        raise HTTPException(status_code=400, detail="Record already exists")
    records_db[record.id] = record
    return record

@app.get("/records/{record_id}", response_model=ServiceRecord)
async def get_record(record_id: int):
    if record_id not in records_db:
        raise HTTPException(status_code=404, detail="Record not found")
    return records_db[record_id]

@app.put("/records/{record_id}/awards")
async def add_award(
    record_id: int,
    award: str,
    background_tasks: BackgroundTasks
):
    if record_id not in records_db:
        raise HTTPException(status_code=404, detail="Record not found")
    
    background_tasks.add_task(update_award_database, record_id, award)
    records_db[record_id].awards.append(award)
    return {"message": "Award added successfully"}

# Background task
async def update_award_database(record_id: int, award: str):
    await asyncio.sleep(2)  # Simulate database update
    print(f"Award {award} recorded for service member {record_id}")
```

## Next Mission Parameters

1. Advanced Topics:
   - WebSocket implementations
   - File uploads/downloads
   - Rate limiting
   - Caching
   - Testing FastAPI applications

2. Deployment Operations:
   - Docker containerization
   - Cloud deployment (AWS, Azure, GCP)
   - CI/CD pipelines
   - Production configurations

Remember:

- Document your APIs thoroughly
- Implement proper security measures
- Test all endpoints
- Handle errors gracefully
- Monitor performance

🇺🇸 #VetsWhoCode