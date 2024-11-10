# VetsWhoCode: Strategic System Design Patterns 🎖️

> "Like military logistics, system design requires planning for scale, resilience, and rapid response." - VetsWhoCode

## Overview
This field manual covers large-scale system design patterns through a military perspective. Each pattern represents battle-tested approaches to building robust, scalable systems.

## Table of Contents
- [Scalability Patterns](#scalability-patterns)
- [Reliability Patterns](#reliability-patterns)
- [Data Management Patterns](#data-management-patterns)

---

## Scalability Patterns

### Military Context
Like expanding military operations from platoon to battalion level, scalability patterns enable systems to grow while maintaining operational effectiveness.

### 1. Load Balancer Pattern - Unit Distribution

```python
class LoadBalancer:
    """
    Mission: Distribute incoming requests across multiple servers.
    
    Tactical Application:
    - Like distributing troops across multiple objectives
    - Prevents single point of failure
    - Maintains operational tempo under heavy load
    
    Strategy:
    - Round-robin distribution
    - Health monitoring
    - Automatic failover
    """
    def __init__(self):
        self.servers = []
        self.current_index = 0
        
    def add_server(self, server):
        self.servers.append({
            'address': server,
            'health': 100,
            'active': True
        })
    
    def get_next_server(self):
        # Round-robin selection of healthy servers
        available_servers = [s for s in self.servers if s['active']]
        if not available_servers:
            raise Exception("No servers available")
            
        server = available_servers[self.current_index % len(available_servers)]
        self.current_index += 1
        return server['address']
    
    def health_check(self):
        # Simulate health checks like checking troops' readiness
        for server in self.servers:
            if server['health'] < 50:
                server['active'] = False

# Field Implementation
balancer = LoadBalancer()
balancer.add_server("server-alpha")
balancer.add_server("server-bravo")
balancer.add_server("server-charlie")
```

### 2. Database Sharding - Territory Division

```python
class DatabaseShard:
    """
    Mission: Distribute data across multiple databases for improved performance.
    
    Tactical Application:
    - Like dividing territory into operational sectors
    - Each shard responsible for specific data range
    - Enables parallel operations
    """
    def __init__(self, shard_key):
        self.shard_key = shard_key
        self.data = {}
        
    def write(self, key, value):
        self.data[key] = value
        
    def read(self, key):
        return self.data.get(key)

class ShardManager:
    def __init__(self, num_shards):
        self.shards = [DatabaseShard(i) for i in range(num_shards)]
    
    def get_shard(self, key):
        # Determine shard based on key hash
        shard_id = hash(key) % len(self.shards)
        return self.shards[shard_id]
    
    def write_data(self, key, value):
        shard = self.get_shard(key)
        shard.write(key, value)
    
    def read_data(self, key):
        shard = self.get_shard(key)
        return shard.read(key)

# Field Implementation
shard_manager = ShardManager(3)  # 3 operational sectors
shard_manager.write_data("soldier_1", "Alpha Company")
shard_manager.write_data("soldier_2", "Bravo Company")
```

---

## Reliability Patterns

### Military Context
Like establishing defensive positions and fallback plans, reliability patterns ensure system resilience under adverse conditions.

### 1. Circuit Breaker - Tactical Retreat

```python
class CircuitBreaker:
    """
    Mission: Prevent system failure by monitoring and stopping problematic operations.
    
    Tactical Application:
    - Like calling tactical retreat to prevent casualties
    - Monitors failure rates
    - Automatic system protection
    """
    def __init__(self, failure_threshold):
        self.failure_threshold = failure_threshold
        self.failure_count = 0
        self.state = "CLOSED"  # CLOSED = operational, OPEN = stopped
        self.last_failure_time = None
        self.reset_timeout = 60  # seconds
        
    def execute(self, operation):
        if self.state == "OPEN":
            if self._should_reset():
                self._reset()
            else:
                raise Exception("Circuit breaker is OPEN")
                
        try:
            result = operation()
            self._success()
            return result
        except Exception as e:
            self._failure()
            raise e
    
    def _failure(self):
        self.failure_count += 1
        if self.failure_count >= self.failure_threshold:
            self.state = "OPEN"
            self.last_failure_time = time.time()
    
    def _success(self):
        self.failure_count = 0
    
    def _should_reset(self):
        return time.time() - self.last_failure_time > self.reset_timeout

# Field Implementation
circuit = CircuitBreaker(failure_threshold=5)
def risky_operation():
    # Simulate remote operation
    pass

try:
    circuit.execute(risky_operation)
except Exception:
    print("Falling back to safe mode")
```

### 2. Bulkhead Pattern - Compartmentalization

```python
import threading
from queue import Queue

class Bulkhead:
    """
    Mission: Isolate components to prevent cascade failures.
    
    Tactical Application:
    - Like compartmentalizing ship sections
    - Isolates failures to specific areas
    - Prevents total system failure
    """
    def __init__(self, name, max_concurrent_calls):
        self.name = name
        self.semaphore = threading.Semaphore(max_concurrent_calls)
        self.queue = Queue()
        
    def execute(self, operation):
        if not self.semaphore.acquire(blocking=False):
            raise Exception(f"Bulkhead {self.name} is full")
        
        try:
            result = operation()
            return result
        finally:
            self.semaphore.release()

# Field Implementation
database_bulkhead = Bulkhead("Database", max_concurrent_calls=10)
api_bulkhead = Bulkhead("API", max_concurrent_calls=20)
```

---

## Data Management Patterns

### Military Context
Like managing military intelligence and logistics, data management patterns ensure efficient information flow and resource utilization.

### 1. CQRS - Operations and Intelligence Separation

```python
class CommandStack:
    """
    Mission: Handle write operations separately from read operations.
    
    Tactical Application:
    - Like separating operations from intelligence
    - Optimized for specific tasks
    - Improved performance and scaling
    """
    def __init__(self):
        self.events = []
        
    def handle_command(self, command):
        # Process and validate command
        event = self._process_command(command)
        self.events.append(event)
        return event
    
    def _process_command(self, command):
        # Command processing logic
        return {"type": command.type, "data": command.data}

class QueryStack:
    def __init__(self):
        self.read_model = {}
        
    def handle_query(self, query):
        # Return data from read-optimized model
        return self.read_model.get(query.key)

# Field Implementation
command_stack = CommandStack()
query_stack = QueryStack()
```

### 2. API Gateway - Forward Operating Base

```python
class APIGateway:
    """
    Mission: Provide centralized access point for multiple services.
    
    Tactical Application:
    - Like a Forward Operating Base
    - Central point of control
    - Resource coordination
    """
    def __init__(self):
        self.services = {}
        self.auth_provider = None
        
    def register_service(self, name, service):
        self.services[name] = service
        
    def route_request(self, service_name, request):
        if not self._authenticate(request):
            raise Exception("Authentication failed")
            
        service = self.services.get(service_name)
        if not service:
            raise Exception(f"Service {service_name} not found")
            
        return service.handle_request(request)
    
    def _authenticate(self, request):
        return self.auth_provider.verify(request.token)

# Field Implementation
gateway = APIGateway()
gateway.register_service("logistics", LogisticsService())
gateway.register_service("personnel", PersonnelService())
```

---

## Additional Resources

### Training Materials
- [System Design Field Manual](https://vetswhocode.io/system-design)
- [Scalability Best Practices](https://vetswhocode.io/scalability)

### Field Notes
1. System design patterns are like strategic military planning
2. Always plan for:
   - Scalability (force multiplication)
   - Reliability (defensive positions)
   - Data Management (intelligence operations)
3. Remember: Systems must be both robust and adaptable

---

*"Building resilient systems with military precision"* - VetsWhoCode