# VetsWhoCode: Strategic Design Patterns 🎖️

> "Good architecture, like good leadership, creates a foundation for success." - VetsWhoCode

## Overview

This field manual covers essential software design patterns through a military lens. Each pattern is presented with tactical analogies and practical Python implementations.

## Table of Contents

- [Creational Patterns](#creational-patterns)
- [Structural Patterns](#structural-patterns)
- [Behavioral Patterns](#behavioral-patterns)

---

## Creational Patterns

### Military Context

Like establishing different types of military installations, creational patterns define how we set up and organize our software assets. Each pattern serves a specific strategic purpose.

### 1. Singleton Pattern - Command Center

```python
class CommandCenter:
    """
    Mission: Ensure only one command center exists for coordinating operations.
    
    Tactical Application:
    - Central command and control
    - Unified communications hub
    - Resource management center
    """
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance.operations_log = []
            cls._instance.active_units = {}
        return cls._instance
    
    def log_operation(self, operation):
        self.operations_log.append(operation)
    
    def register_unit(self, unit_id, unit_type):
        self.active_units[unit_id] = unit_type

# Field Test
command1 = CommandCenter()
command2 = CommandCenter()
print(f"Same command center: {command1 is command2}")  # True
```

### 2. Factory Method - Equipment Factory

```python
from abc import ABC, abstractmethod

class Equipment(ABC):
    @abstractmethod
    def operate(self):
        pass

class Rifle(Equipment):
    def operate(self):
        return "Rifle ready for operation"

class Radio(Equipment):
    def operate(self):
        return "Radio established comms"

class EquipmentFactory:
    """
    Mission: Standardize equipment creation process.
    
    Tactical Application:
    - Standardized equipment procurement
    - Quality control
    - Inventory management
    """
    @staticmethod
    def create_equipment(equipment_type):
        if equipment_type == "rifle":
            return Rifle()
        elif equipment_type == "radio":
            return Radio()
        raise ValueError(f"Unknown equipment type: {equipment_type}")

# Field Test
factory = EquipmentFactory()
rifle = factory.create_equipment("rifle")
radio = factory.create_equipment("radio")
```

### 3. Builder Pattern - Mission Planner

```python
class MissionPlan:
    def __init__(self):
        self.objectives = []
        self.resources = []
        self.timeline = None
        self.extraction_plan = None

class MissionBuilder:
    """
    Mission: Construct complex mission plans step by step.
    
    Tactical Application:
    - Detailed mission planning
    - Resource allocation
    - Contingency planning
    """
    def __init__(self):
        self.mission = MissionPlan()
    
    def add_objective(self, objective):
        self.mission.objectives.append(objective)
        return self
    
    def add_resource(self, resource):
        self.mission.resources.append(resource)
        return self
    
    def set_timeline(self, timeline):
        self.mission.timeline = timeline
        return self
    
    def set_extraction(self, extraction_plan):
        self.mission.extraction_plan = extraction_plan
        return self
    
    def build(self):
        return self.mission

# Field Test
mission = (MissionBuilder()
    .add_objective("Secure perimeter")
    .add_resource("Alpha Team")
    .set_timeline("H-Hour to H+6")
    .set_extraction("Helo pickup at LZ Bravo")
    .build())
```

---

## Structural Patterns

### Military Context

Like organizing units and establishing command relationships, structural patterns define how different components of the system work together.

### 1. Adapter Pattern - Communications Interface

```python
class ModernRadio:
    def transmit_digital(self, message):
        return f"Digital transmission: {message}"

class LegacyRadio:
    def send_analog(self, message):
        return f"Analog signal: {message}"

class RadioAdapter:
    """
    Mission: Enable communication between modern and legacy systems.
    
    Tactical Application:
    - Cross-platform communications
    - Legacy system integration
    - Interoperability assurance
    """
    def __init__(self, legacy_radio):
        self.legacy_radio = legacy_radio
    
    def transmit_digital(self, message):
        analog_message = self.convert_to_analog(message)
        return self.legacy_radio.send_analog(analog_message)
    
    def convert_to_analog(self, message):
        return f"Converted: {message}"

# Field Test
modern = ModernRadio()
legacy = LegacyRadio()
adapter = RadioAdapter(legacy)
print(adapter.transmit_digital("Alpha moving to objective"))
```

### 2. Decorator Pattern - Equipment Modifications

```python
from abc import ABC, abstractmethod

class Weapon(ABC):
    @abstractmethod
    def operate(self):
        pass

class BaseRifle(Weapon):
    def operate(self):
        return "Standard rifle operations"

class WeaponModification(Weapon):
    """
    Mission: Add modular capabilities to equipment.
    
    Tactical Application:
    - Equipment customization
    - Mission-specific modifications
    - Capability enhancement
    """
    def __init__(self, weapon):
        self._weapon = weapon

class ScopeDecorator(WeaponModification):
    def operate(self):
        return f"{self._weapon.operate()} + Enhanced targeting"

class SuppressorDecorator(WeaponModification):
    def operate(self):
        return f"{self._weapon.operate()} + Noise reduction"

# Field Test
rifle = BaseRifle()
scoped_rifle = ScopeDecorator(rifle)
tactical_rifle = SuppressorDecorator(scoped_rifle)
print(tactical_rifle.operate())
```

---

## Behavioral Patterns

### Military Context

Like standard operating procedures (SOPs), behavioral patterns define how different components communicate and operate together.

### 1. Observer Pattern - Alert System

```python
from abc import ABC, abstractmethod

class AlertSystem(ABC):
    @abstractmethod
    def update(self, message):
        pass

class Unit(AlertSystem):
    def __init__(self, name):
        self.name = name
    
    def update(self, message):
        print(f"{self.name} received alert: {message}")

class CommandPost:
    """
    Mission: Coordinate and disseminate information to multiple units.
    
    Tactical Application:
    - Alert dissemination
    - Unit coordination
    - Information broadcasting
    """
    def __init__(self):
        self._units = []
    
    def attach(self, unit):
        self._units.append(unit)
    
    def detach(self, unit):
        self._units.remove(unit)
    
    def notify(self, message):
        for unit in self._units:
            unit.update(message)

# Field Test
command_post = CommandPost()
alpha_team = Unit("Alpha Team")
bravo_team = Unit("Bravo Team")

command_post.attach(alpha_team)
command_post.attach(bravo_team)
command_post.notify("Enemy contact at grid 123456")
```

### 2. Strategy Pattern - Mission Tactics

```python
from abc import ABC, abstractmethod

class TacticalApproach(ABC):
    @abstractmethod
    def execute(self):
        pass

class StealthApproach(TacticalApproach):
    def execute(self):
        return "Executing silent approach"

class DirectAssault(TacticalApproach):
    def execute(self):
        return "Executing frontal assault"

class MissionExecutor:
    """
    Mission: Adapt tactical approach based on situation.
    
    Tactical Application:
    - Flexible mission execution
    - Adaptable tactics
    - Situational response
    """
    def __init__(self, strategy: TacticalApproach):
        self._strategy = strategy
    
    def change_strategy(self, strategy: TacticalApproach):
        self._strategy = strategy
    
    def execute_mission(self):
        return self._strategy.execute()

# Field Test
mission = MissionExecutor(StealthApproach())
print(mission.execute_mission())
mission.change_strategy(DirectAssault())
print(mission.execute_mission())
```

---

### Field Notes

1. Design patterns are like battle-tested strategies - use them wisely
2. Each pattern solves specific tactical problems
3. Remember: Good architecture enables mission success

---

*"Building robust systems with military precision"* - VetsWhoCode