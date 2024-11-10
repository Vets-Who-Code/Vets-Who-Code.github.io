# VetsWhoCode: Combat-Ready Coding Patterns 🎖️

> "In code as in combat, patterns and preparation determine success." - VetsWhoCode

## Mission Overview

These battle-tested coding patterns form the foundation of technical interviews and efficient problem-solving. Like standard operating procedures (SOPs) in combat, these patterns provide proven solutions to common challenges.

## Table of Contents

- [Mission Briefing](#mission-briefing)
- [Two Pointers Pattern](#two-pointers-pattern)
- [Sliding Window Pattern](#sliding-window-pattern)
- [Binary Search Pattern](#binary-search-pattern)
- [After Action Review](#after-action-review)

## Mission Briefing

These patterns are your tactical toolset for coding interviews. Like a well-maintained weapon system, each pattern serves a specific purpose and should be mastered through consistent practice and application.

### Battle Rhythm

1. Identify the pattern that fits your mission
2. Apply the appropriate tactical solution
3. Test and verify your implementation
4. Optimize for better performance

---

## Two Pointers Pattern

### Tactical Overview

**Mission Type:** Coordinated Movement Operations

**Military Context:** Like coordinating two fire teams during a patrol - each team moves with purpose and coordination to achieve the mission objective. Just as fire teams maintain relative positioning, our pointers maintain their relationship while moving through the array.

### Battle Plan

1. Initialize two pointers at strategic positions
2. Move pointers based on mission parameters
3. Maintain coordination between pointer movements
4. Achieve objective with minimal traversal

### Implementation: Equipment Load Distribution

```python
def find_equipment_pairs(weights, target_weight):
    """
    Mission: Find pairs of equipment that add up to target weight.
    
    Tactical Application: 
    - Organizing gear for balanced load distribution
    - Matching battle buddies for equipment carry
    - Ensuring optimal weight distribution across the unit
    
    Operation Parameters:
    weights (list): List of equipment weights
    target_weight (int): Target combined weight
    
    Mission Outcome:
    list: Pairs of weights that match target
    """
    weights.sort()  # Organize equipment by weight
    left = 0  # Alpha team position
    right = len(weights) - 1  # Bravo team position
    pairs = []
    
    while left < right:
        current_weight = weights[left] + weights[right]
        if current_weight == target_weight:
            pairs.append((weights[left], weights[right]))
            left += 1  # Alpha team advances
            right -= 1  # Bravo team falls back
        elif current_weight < target_weight:
            left += 1  # Alpha team moves forward
        else:
            right -= 1  # Bravo team adjusts position
            
    return pairs

# Field Operations Test
equipment_weights = [10, 15, 20, 25, 30, 35]
target = 45
pairs = find_equipment_pairs(equipment_weights, target)
print(f"SITREP: Equipment pairs matching {target}lbs: {pairs}")
```

### Combat Tips

- Always sort when dealing with pair finding
- Consider edge cases like empty arrays
- Watch for pointer bounds

---

## Sliding Window Pattern

### Tactical Overview

**Mission Type:** Surveillance and Monitoring Operations

**Military Context:** Analogous to maintaining a surveillance window on an area of operation. Like how a unit maintains constant observation while moving through terrain, the sliding window keeps track of a specific subset of data while moving through the array.

### Battle Plan

1. Establish initial observation window
2. Monitor and record metrics within window
3. Advance window systematically
4. Maintain optimal window size for mission requirements

### Implementation: Unit Performance Tracking

```python
def analyze_performance_window(performance_scores, days):
    """
    Mission: Track maximum average performance over consecutive days.
    
    Tactical Application:
    - Monitoring unit readiness
    - Tracking squad performance trends
    - Identifying optimal training periods
    - Measuring combat effectiveness
    
    Operation Parameters:
    performance_scores (list): Daily performance metrics
    days (int): Window size to analyze
    
    Mission Outcome:
    float: Maximum average performance for any window
    """
    if not performance_scores or days <= 0:
        return 0
        
    # Establish initial observation window
    current_sum = sum(performance_scores[:days])
    max_sum = current_sum
    
    # Advance observation window
    for i in range(days, len(performance_scores)):
        # Shift window forward
        current_sum = current_sum - performance_scores[i - days] + performance_scores[i]
        max_sum = max(max_sum, current_sum)
    
    return max_sum / days

# Field Operations Test
daily_scores = [85, 90, 92, 88, 86, 95, 92]
window_size = 3
max_avg = analyze_performance_window(daily_scores, window_size)
print(f"SITREP: Peak performance average over {window_size} days: {max_avg}")
```

### Combat Tips

- Initialize window bounds correctly
- Update window contents efficiently
- Track maximum/minimum values as needed

---

## Fast & Slow Pointers Pattern

### Tactical Overview

**Mission Type:** Reconnaissance Operations

**Military Context:** Similar to scout team and main element movement. The scout team (fast pointer) moves ahead at twice the pace while maintaining coordination with the main element (slow pointer).

### Battle Plan

1. Deploy scout element (fast pointer)
2. Coordinate with main element (slow pointer)
3. Maintain communication between elements
4. Identify patterns or cycles in movement

### Implementation: Patrol Route Cycle Detection

```python
class PatrolPoint:
    """
    Tactical waypoint in patrol route
    """
    def __init__(self, location, next=None):
        self.location = location
        self.next = next

def detect_circular_patrol(patrol_route):
    """
    Mission: Detect if a patrol route circles back to a previous point.
    
    Tactical Application:
    - Route reconnaissance
    - Patrol pattern analysis
    - Identifying redundant coverage
    - Ensuring efficient area coverage
    
    Operation Parameters:
    patrol_route: Starting point of patrol route
    
    Mission Outcome:
    bool: True if route contains a cycle
    """
    if not patrol_route:
        return False
        
    # Scout team (fast) and main element (slow)
    main_element = patrol_route
    scout_team = patrol_route.next
    
    while scout_team and scout_team.next:
        if main_element == scout_team:
            return True  # Circular pattern detected
        main_element = main_element.next
        scout_team = scout_team.next.next
        
    return False

# Field Operations Test
# Create patrol route: Alpha -> Bravo -> Charlie -> Delta -> Bravo
point_a = PatrolPoint("Alpha")
point_b = PatrolPoint("Bravo")
point_c = PatrolPoint("Charlie")
point_d = PatrolPoint("Delta")

point_a.next = point_b
point_b.next = point_c
point_c.next = point_d
point_d.next = point_b  # Creates cycle

is_circular = detect_circular_patrol(point_a)
print(f"SITREP: Patrol route contains circular pattern: {is_circular}")
```

### Combat Tips

- Watch for null/none values
- Handle edge cases carefully
- Maintain proper pointer speeds

---

## Binary Search Pattern

### Tactical Overview

**Mission Type:** Target Acquisition Operations

**Military Context:** Similar to bracketing procedures in artillery fire missions. You systematically narrow down your target area by adjusting between known boundaries until you achieve target acquisition.

### Battle Plan

1. Establish search boundaries
2. Calculate mid-point
3. Adjust search sector based on target position
4. Repeat until target acquired or sector cleared

### Implementation: Personnel Location in Formation

```python
def locate_in_formation(positions, target_id):
    """
    Mission: Efficiently locate a service member's position in formation.
    
    Tactical Application:
    - Personnel accountability
    - Formation management
    - Quick reaction force assignments
    - Rapid personnel location
    
    Operation Parameters:
    positions (list): Sorted list of position IDs
    target_id (int): ID to locate
    
    Mission Outcome:
    int: Index of target position or -1 if not found
    """
    left = 0  # Left boundary
    right = len(positions) - 1  # Right boundary
    
    while left <= right:
        mid = (left + right) // 2  # Center of current sector
        
        if positions[mid] == target_id:
            return mid  # Target acquired
        elif positions[mid] < target_id:
            left = mid + 1  # Adjust sector right
        else:
            right = mid - 1  # Adjust sector left
            
    return -1  # Target not found

# Field Operations Test
formation_positions = [2, 4, 6, 8, 10, 12, 14, 16]
target = 10
position = locate_in_formation(formation_positions, target)
print(f"SITREP: Service member {target} located at position: {position}")
```

### Combat Tips

- Ensure array is sorted before search
- Handle boundary conditions
- Watch for integer overflow in midpoint calculation

---

## After Action Review

### Key Tactical Points

1. Each pattern represents a battle-tested approach to solving specific types of problems
2. Practice these patterns until they become muscle memory
3. Remember: In both coding and combat, preparation and pattern recognition are key to success

### Field Notes

- Document your approaches
- Learn from failed attempts
- Build your pattern recognition skills

## Standard Operating Procedures

1. Always identify the pattern before coding
2. Test with multiple scenarios
3. Consider edge cases
4. Optimize after achieving basic functionality

---

*"Code with Honor, Debug with Discipline"* - VetsWhoCode

*"Mission success through tactical coding excellence"*