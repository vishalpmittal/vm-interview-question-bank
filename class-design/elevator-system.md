# Elevator System

## Prompt

Design an elevator system that handles requests from floors and inside elevators, coordinates multiple elevators, and steps the system forward in time.

## Clarifying Questions

- How many elevators and floors?
- Single shaft per elevator, or shared shafts?
- Scheduling policy — FCFS, SCAN/LOOK, nearest-car?
- Capacity / weight limits?
- Service modes — express, freight, maintenance?
- Real-time simulation, or discrete `step()` calls?

## Class Skeleton

```
Direction (Enum)               # UP, DOWN, IDLE
ElevatorState (Enum)           # MOVING, STOPPED, MAINTENANCE, ...

Request (Class)
    sourceFloor
    destinationFloor
    direction: Direction

Elevator (Class)
    id
    currentFloor
    state: ElevatorState
    direction: Direction
    requests: Queue[Request]
    addRequest(request)
    step()                     # advance one tick: move / open doors / pick up
    _openDoors()

ElevatorController (Class)
    elevators: List[Elevator]
    requestElevator(sourceFloor, direction)
    stepAll()                  # tick every elevator
    processRequests()          # assign pending requests to elevators
    getElevatorFloor(id)
    getElevatorState(id)
    getElevatorDirection(id)
```

## Design Notes

- **Separation of concerns**: `Elevator` knows how to move; `ElevatorController` decides *which* elevator handles a request.
- **Scheduling** lives in `processRequests` — swap algorithm (nearest-car, LOOK) without touching the elevator class.
- **Time progression**: `step()` model makes the design testable and decouples simulation from real wall time.
- **Concurrency**: requests can arrive from multiple sources; queue per elevator is the natural mutex boundary.
- **Extension points**: maintenance mode, freight-only elevators, priority requests (medical, fire).
