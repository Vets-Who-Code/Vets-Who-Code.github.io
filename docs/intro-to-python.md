# Vets Who Code - Python Fundamentals 🎖️

## Mission Briefing

Welcome to the Python track of Vets Who Code! Like any good military operation, we'll break this down into clear, manageable objectives. Your military experience has already equipped you with the discipline and problem-solving skills needed to excel in programming.

## Phase 1: Establishing Base (Setup)

### Setting Up Your FOB (Forward Operating Base)

1. Download Python from python.org (like requisitioning new equipment)
2. Install with "Add Python to PATH" checked (establishing your supply lines)
3. Verify installation in terminal:
   
```bash
python --version
```

### First Boots on the Ground

```python
# Your first line of code is like your first patrol
print("Vets Who Code: Reporting for Duty!")
```

## Phase 2: Basic Training

### Intelligence Types (Data Types)

```python
# Personnel Count (Integer)
squad_size = 12

# Supply Levels (Float)
fuel_level = 85.5

# Call Signs (String)
unit_name = "Alpha"
status = 'Oscar Mike'  # 'On Mission'

# Mission Status (Boolean)
is_deployed = True
mission_complete = False

# Intel Not Available (None)
pending_orders = None
```

### Field Communications (Basic Operations)

```python
# Tactical Math
available_transport = 5
required_transport = 3

total_transport = available_transport + required_transport
remaining_transport = available_transport - required_transport

# Unit Designations
battalion = "2nd"
division = "Infantry"
unit = battalion + " " + division  # 2nd Infantry

# Field Reports
print(f"Unit Strength: {squad_size}")
print(f"Unit Status: {status}")
```

## Phase 3: Combat Operations (Control Flow)

### Mission Planning (If Statements)

```python
def check_readiness(troops, vehicles):
    if troops >= 10 and vehicles >= 2:
        return "Unit is Combat Ready"
    elif troops >= 5:
        return "Unit is Patrol Ready"
    else:
        return "Unit Needs Reinforcement"

# Example Usage
unit_status = check_readiness(8, 1)
print(unit_status)
```

### Patrol Patterns (Loops)
```python
# Perimeter Check (For Loop)
sectors = ["North", "East", "South", "West"]
for sector in sectors:
    print(f"Sector {sector} is secure")

# Watch Rotation (While Loop)
watch_hours = 4
current_hour = 0
while current_hour < watch_hours:
    print(f"Hour {current_hour + 1} of watch")
    current_hour += 1
```

## Phase 4: Equipment and Logistics (Data Structures)

### Equipment Roster (Lists)
```python
# Basic gear list
gear = ["rifle", "nods", "radio", "ruck"]

# Adding gear
gear.append("ifak")  # Adds to end
gear.insert(0, "weapon")  # Adds at position

# Checking gear
print(f"First item: {gear[0]}")
print(f"Last item: {gear[-1]}")

# Removing gear
gear.remove("radio")  # Removes specific item
last_item = gear.pop()  # Removes and returns last item
```

### Mission Intel (Dictionaries)

```python
# Mission details
mission = {
    "type": "Training",
    "location": "Virtual FOB",
    "duration": 14,
    "unit": "VWC Python Platoon"
}

# Accessing intel
print(f"Mission Type: {mission['type']}")
print(f"Location: {mission.get('location')}")

# Updating intel
mission["status"] = "In Progress"
mission["duration"] = 21
```

## Phase 5: Field Exercise

Here's a practical exercise combining the concepts - a Military Awards Database:

```python
def awards_database():
    """
    Military Awards Reference System
    """
    awards = {
        "1": {
            "name": "Purple Heart",
            "criteria": "Wounded in combat",
            "precedence": "High",
            "branch": "All services"
        },
        "2": {
            "name": "Combat Action Ribbon",
            "criteria": "Engaged in combat",
            "precedence": "Medium",
            "branch": "Navy/Marines"
        }
    }

    while True:
        print("\nVWC Awards Database")
        print("------------------")
        for key, award in awards.items():
            print(f"{key}: {award['name']}")
        print("3: Exit System")

        selection = input("\nEnter award number: ")
        
        if selection == "3":
            print("Database Secured!")
            break
        elif selection in awards:
            award = awards[selection]
            print(f"\nName: {award['name']}")
            print(f"Criteria: {award['criteria']}")
            print(f"Precedence: {award['precedence']}")
            print(f"Branch: {award['branch']}")
        else:
            print("Invalid selection, try again")

if __name__ == "__main__":
    awards_database()
```

## Next Mission Parameters

### Coming Up Next:

1. Functions (Battle Drills)
2. Error Handling (Combat Contingencies)
3. File Operations (Mission Logs)
4. Classes (Unit Organization)
5. Modules (Support Elements)

### Core Values to Remember:

- Attention to Detail (Clean Code)
- Adapt and Overcome (Problem Solving)
- Leave No One Behind (Help Others Learn)
- Mission First (Working Code)

Ready to move to advanced operations? Remember:

- Stay motivated
- Keep practicing
- Debug with discipline
- Learn from each mission

🇺🇸 #VetsWhoCode 