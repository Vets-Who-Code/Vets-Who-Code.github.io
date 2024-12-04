# Vets Who Code - Advanced Python Operations 🎖️

## Phase 1: Battle Drills (Functions)

### Basic Function Structure

```python
def call_sign_generator(last_name, unit):
    """
    Docstring: Like a mission brief, explains the function's purpose
    Args:
        last_name: String of operator's last name
        unit: String of unit designation
    Returns:
        String: Generated call sign
    """
    return f"VIPER_{last_name.upper()}_{unit}"

# Using the function
operator_call_sign = call_sign_generator("Smith", "PYTHON")
print(operator_call_sign)  # Output: VIPER_SMITH_PYTHON
```

### Function Parameters (Loading Combat Gear)

```python
# Default Parameters
def create_fire_team(team_leader, size=4, designation="Alpha"):
    return f"Team {designation}: {size} operators led by {team_leader}"

# Multiple Parameters
def mission_brief(date, time, location, *objectives, **support_units):
    """
    *objectives: Variable args (tuple) for multiple mission objectives
    **support_units: Keyword args (dict) for support elements
    """
    brief = f"Mission Date: {date} at {time}\nLocation: {location}\n"
    brief += "\nObjectives:\n"
    for obj in objectives:
        brief += f"- {obj}\n"
    brief += "\nSupport Units:\n"
    for unit, role in support_units.items():
        brief += f"- {unit}: {role}\n"
    return brief

# Using complex function
brief = mission_brief("2024-11-10", "0600",
                     "Grid Alpha-7",
                     "Secure Perimeter",
                     "Establish Comms",
                     "Deploy Assets",
                     air_support="Close Air Support",
                     medical="Combat Medic Team")
```

## Phase 2: Combat Contingencies (Error Handling)

### Basic Error Handling

```python
def check_ammunition(mag_count):
    try:
        if not isinstance(mag_count, int):
            raise TypeError("Magazine count must be a number")
        if mag_count < 0:
            raise ValueError("Magazine count cannot be negative")
        if mag_count < 3:
            print("Low ammunition warning!")
        return f"Ammunition check: {mag_count} magazines"
    except TypeError as e:
        return f"Error in ammunition count: {e}"
    except ValueError as e:
        return f"Invalid ammunition count: {e}"
    except Exception as e:
        return f"Unknown error in ammunition check: {e}"
    finally:
        print("Ammunition check complete")
```

### Custom Exceptions

```python
class MissionCriticalError(Exception):
    """Custom exception for mission-critical failures"""
    pass

def verify_comms(signal_strength):
    try:
        if signal_strength < 50:
            raise MissionCriticalError("Communications signal too weak")
        return "Communications operational"
    except MissionCriticalError as e:
        return f"MISSION CRITICAL: {e}"
```

## Phase 3: Mission Logs (File Operations)

### Basic File Operations

```python
# Writing mission logs
def write_mission_log(mission_id, details):
    try:
        with open(f"mission_{mission_id}.log", "w") as log_file:
            log_file.write(f"Mission ID: {mission_id}\n")
            log_file.write(f"Timestamp: {datetime.now()}\n")
            log_file.write("Details:\n")
            log_file.write(details)
    except IOError as e:
        print(f"Error writing mission log: {e}")

# Reading mission logs
def read_mission_log(mission_id):
    try:
        with open(f"mission_{mission_id}.log", "r") as log_file:
            return log_file.read()
    except FileNotFoundError:
        return "Mission log not found"
```

### CSV Operations (Personnel Records)

```python
import csv

def log_personnel_data(filename, personnel_list):
    with open(filename, 'w', newline='') as csvfile:
        writer = csv.DictWriter(csvfile, 
                              fieldnames=['id', 'name', 'rank', 'unit'])
        writer.writeheader()
        for person in personnel_list:
            writer.writerow(person)

def read_personnel_data(filename):
    with open(filename, 'r') as csvfile:
        reader = csv.DictReader(csvfile)
        return list(reader)
```

## Phase 4: Unit Organization (Classes)

### Basic Class Structure

```python
class MilitaryUnit:
    def __init__(self, designation, strength, location):
        self.designation = designation
        self.strength = strength
        self.location = location
        self.status = "Active"
    
    def deploy(self, new_location):
        self.location = new_location
        return f"{self.designation} deployed to {new_location}"
    
    def report_status(self):
        return f"Unit: {self.designation}\nStrength: {self.strength}\nLocation: {self.location}\nStatus: {self.status}"

# Using the class
alpha_team = MilitaryUnit("Alpha", 12, "Base")
print(alpha_team.deploy("Grid Bravo-7"))
print(alpha_team.report_status())
```

### Inheritance (Specialized Units)

```python
class SpecialOperationsUnit(MilitaryUnit):
    def __init__(self, designation, strength, location, specialty):
        super().__init__(designation, strength, location)
        self.specialty = specialty
        self.deployed = False
    
    def conduct_operation(self, operation_type):
        if operation_type == self.specialty:
            return f"Conducting {operation_type} operation with {self.strength} operators"
        return f"Operation type mismatch with unit specialty"

# Using inherited class
delta_force = SpecialOperationsUnit("Delta", 8, "Classified", "Direct Action")
print(delta_force.conduct_operation("Direct Action"))
```

## Phase 5: Support Elements (Modules)

### Creating Custom Modules

```python
# mission_utils.py
def generate_grid_reference(x, y):
    return f"GRID_{x}{y}"

def calculate_bearing(start, end):
    # Complex bearing calculation logic here
    pass

# Using modules
from mission_utils import generate_grid_reference
grid = generate_grid_reference(5, 8)
```

### Module Organization

```python
# Project structure
military_ops/
    __init__.py
    personnel/
        __init__.py
        roster.py
        qualifications.py
    operations/
        __init__.py
        mission_planning.py
        logistics.py
    utils/
        __init__.py
        grid_calc.py
        comms.py
```

## Field Exercise: Combined Arms Operation

Let's combine all these concepts in a practical exercise - a Mission Planning System:

```python
class MissionPlanningSystem:
    def __init__(self):
        self.missions = {}
        self.units = {}
        self.logs = []
    
    def plan_mission(self, mission_id, objective):
        try:
            if mission_id in self.missions:
                raise ValueError("Mission ID already exists")
            
            mission = {
                "id": mission_id,
                "objective": objective,
                "status": "Planning",
                "units": []
            }
            
            self.missions[mission_id] = mission
            self._log_event(f"Mission {mission_id} created")
            return "Mission created successfully"
            
        except Exception as e:
            self._log_event(f"Error creating mission: {e}")
            raise
    
    def assign_unit(self, mission_id, unit):
        if mission_id in self.missions:
            self.missions[mission_id]["units"].append(unit)
            self._log_event(f"Unit {unit.designation} assigned to mission {mission_id}")
    
    def _log_event(self, event):
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        log_entry = f"[{timestamp}] {event}"
        self.logs.append(log_entry)
        
        # Write to file
        with open("mission_logs.txt", "a") as f:
            f.write(log_entry + "\n")

# Using the system
mps = MissionPlanningSystem()
alpha = MilitaryUnit("Alpha", 10, "Base")
try:
    mps.plan_mission("OPS001", "Secure Objective Bravo")
    mps.assign_unit("OPS001", alpha)
except Exception as e:
    print(f"Mission planning failed: {e}")
```

## Movement Orders (Next Steps)

1. Practice implementing these concepts with your own scenarios
2. Build a complete project combining multiple concepts
3. Learn about:
   - Testing (Mission Rehearsal)
   - Debugging (Equipment Checks)
   - Advanced OOP concepts (Unit Organization)
   - API integration (Joint Operations)

Remember:

- Code with discipline
- Document thoroughly
- Test rigorously
- Stay mission-focused

🇺🇸 #VetsWhoCode