# Vets Who Code - FastAPI Testing Operations Guide 🎖️

## Mission Brief: Testing Strategy

Testing is like a pre-combat equipment check - it ensures everything works before deployment.

## Phase 1: Testing Setup

### Required Equipment

```bash
# Install testing dependencies
pip install pytest httpx pytest-asyncio pytest-cov

# Project structure
military_api/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── models.py
│   ├── routes.py
│   └── database.py
└── tests/
    ├── __init__.py
    ├── test_main.py
    ├── test_routes.py
    └── conftest.py
```

### Basic Test Configuration (conftest.py)

```python
# tests/conftest.py
import pytest
from fastapi.testclient import TestClient
from typing import Generator
from app.main import app
from app.database import Base, get_db, engine

@pytest.fixture(scope="function")
def test_client() -> Generator:
    """
    Creates a test client for API testing operations
    """
    with TestClient(app) as client:
        yield client

@pytest.fixture(scope="function")
def test_db():
    """
    Creates a clean test database for each test
    """
    Base.metadata.create_all(bind=engine)
    yield
    Base.metadata.drop_all(bind=engine)
```

## Phase 2: Basic Unit Tests

### Testing API Endpoints

```python
# tests/test_routes.py
import pytest
from fastapi.testclient import TestClient

def test_read_main(test_client: TestClient):
    """
    Tests the base endpoint response
    """
    response = test_client.get("/")
    assert response.status_code == 200
    assert response.json() == {
        "status": "operational",
        "message": "VWC API Standing By"
    }

def test_create_service_member(test_client: TestClient):
    """
    Tests service member creation endpoint
    """
    test_data = {
        "id": 1,
        "first_name": "John",
        "last_name": "Smith",
        "rank": "SGT",
        "branch": "Army",
        "service_start": "2020-01-01",
        "status": "Active"
    }
    
    response = test_client.post("/personnel/", json=test_data)
    assert response.status_code == 200
    data = response.json()
    assert data["first_name"] == test_data["first_name"]
    assert data["rank"] == test_data["rank"]
```

### Testing Authentication

```python
# tests/test_auth.py
import pytest
from fastapi.testclient import TestClient

def test_login(test_client: TestClient):
    """
    Tests user authentication
    """
    response = test_client.post(
        "/token",
        data={"username": "test_user", "password": "test_pass"}
    )
    assert response.status_code == 200
    assert "access_token" in response.json()

@pytest.mark.parametrize(
    "credentials,expected_status",
    [
        ({"username": "wrong_user", "password": "test_pass"}, 401),
        ({"username": "test_user", "password": "wrong_pass"}, 401),
    ],
)
def test_login_invalid_credentials(
    test_client: TestClient,
    credentials,
    expected_status
):
    """
    Tests authentication with invalid credentials
    """
    response = test_client.post("/token", data=credentials)
    assert response.status_code == expected_status
```

## Phase 3: Advanced Testing Operations

### Testing Database Operations

```python
# tests/test_database.py
import pytest
from sqlalchemy.orm import Session
from app.models import ServiceMember
from datetime import date

def test_create_service_member(test_db: Session):
    """
    Tests database operations for service member creation
    """
    member = ServiceMember(
        id=1,
        first_name="John",
        last_name="Smith",
        rank="SGT",
        branch="Army",
        service_start=date(2020, 1, 1)
    )
    test_db.add(member)
    test_db.commit()
    
    db_member = test_db.query(ServiceMember).first()
    assert db_member.first_name == "John"
    assert db_member.rank == "SGT"

@pytest.mark.asyncio
async def test_get_service_member(test_db: Session):
    """
    Tests async database operations
    """
    # Setup test data
    member = ServiceMember(
        id=1,
        first_name="John",
        last_name="Smith",
        rank="SGT",
        branch="Army",
        service_start=date(2020, 1, 1)
    )
    test_db.add(member)
    test_db.commit()

    # Test retrieval
    db_member = test_db.query(ServiceMember).filter(
        ServiceMember.id == 1
    ).first()
    assert db_member is not None
    assert db_member.first_name == "John"
```

### Testing Background Tasks

```python
# tests/test_tasks.py
import pytest
from fastapi import BackgroundTasks
from unittest.mock import Mock, patch

def test_background_task_creation(test_client: TestClient):
    """
    Tests background task scheduling
    """
    with patch('app.tasks.update_mission_status') as mock_task:
        response = test_client.post("/missions/1/status")
        assert response.status_code == 200
        mock_task.assert_called_once_with("1")

@pytest.mark.asyncio
async def test_award_update_task():
    """
    Tests award update background task
    """
    mock_db = Mock()
    record_id = 1
    award = "Purple Heart"
    
    with patch('asyncio.sleep'):  # Mock the sleep function
        await update_award_database(record_id, award)
        mock_db.update_award.assert_called_once_with(
            record_id,
            award
        )
```

## Phase 4: Integration Testing

### Testing Complete Workflows

```python
# tests/test_integration.py
import pytest
from fastapi.testclient import TestClient

def test_complete_service_record_workflow(test_client: TestClient):
    """
    Tests complete service record workflow:
    1. Create service member
    2. Add awards
    3. Update status
    4. Retrieve complete record
    """
    # Create service member
    member_data = {
        "id": 1,
        "first_name": "John",
        "last_name": "Smith",
        "rank": "SGT",
        "branch": "Army",
        "service_start": "2020-01-01"
    }
    response = test_client.post("/personnel/", json=member_data)
    assert response.status_code == 200
    member_id = response.json()["id"]

    # Add award
    award_data = {
        "type": "Purple Heart",
        "award_date": "2021-06-15"
    }
    response = test_client.post(
        f"/personnel/{member_id}/awards",
        json=award_data
    )
    assert response.status_code == 200

    # Verify final record
    response = test_client.get(f"/personnel/{member_id}")
    assert response.status_code == 200
    data = response.json()
    assert len(data["awards"]) == 1
    assert data["awards"][0]["type"] == "Purple Heart"
```

## Phase 5: Test Coverage and Reporting

### Running Tests with Coverage

```bash
# Run tests with coverage report
pytest --cov=app tests/
```

### Coverage Configuration (setup.cfg)

```ini
[coverage:run]
source = app
omit = tests/*

[coverage:report]
exclude_lines =
    pragma: no cover
    def __repr__
    raise NotImplementedError
```

## Field Exercise: Complete Test Suite

Here's a practical example combining all testing concepts:

```python
# tests/test_awards_system.py
import pytest
from fastapi.testclient import TestClient
from app.models import Award, ServiceMember
from datetime import date

class TestAwardsSystem:
    @pytest.fixture
    def test_member(self, test_db):
        """Creates test service member"""
        member = ServiceMember(
            id=1,
            first_name="John",
            last_name="Smith",
            rank="SGT",
            branch="Army",
            service_start=date(2020, 1, 1)
        )
        test_db.add(member)
        test_db.commit()
        return member

    def test_create_award(
        self,
        test_client: TestClient,
        test_member: ServiceMember
    ):
        """Tests award creation"""
        award_data = {
            "type": "Purple Heart",
            "recipient_id": test_member.id,
            "award_date": "2021-06-15",
            "citation": "Wounded in action"
        }
        response = test_client.post("/awards/", json=award_data)
        assert response.status_code == 200
        data = response.json()
        assert data["type"] == award_data["type"]
        assert data["recipient_id"] == test_member.id

    def test_get_member_awards(
        self,
        test_client: TestClient,
        test_member: ServiceMember
    ):
        """Tests award retrieval"""
        response = test_client.get(
            f"/personnel/{test_member.id}/awards"
        )
        assert response.status_code == 200
        data = response.json()
        assert isinstance(data["awards"], list)

    @pytest.mark.asyncio
    async def test_award_background_processing(
        self,
        test_client: TestClient,
        test_member: ServiceMember
    ):
        """Tests award processing background task"""
        with patch('app.tasks.process_award') as mock_task:
            response = test_client.post(
                f"/awards/process/{test_member.id}"
            )
            assert response.status_code == 200
            mock_task.assert_called_once()
```

## Mission Parameters (Best Practices)

1. Testing Guidelines:
   - Write tests before code (TDD)
   - Test both success and failure cases
   - Use meaningful test names
   - Keep tests independent
   - Mock external services

2. Testing Structure:
   - Unit tests for individual components
   - Integration tests for workflows
   - End-to-end tests for complete features
   - Performance tests for critical paths

Remember:

- Test early and often
- Maintain test coverage
- Document test cases
- Update tests with code changes

🇺🇸 #VetsWhoCode