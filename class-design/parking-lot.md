# Parking Lot / Garage

## Prompt

Design a parking lot / garage that supports multiple vehicle types and spot sizes, tracks availability across levels, and handles vehicle entry / exit.

## Clarifying Questions

- Vehicle types? (motorcycle, car, truck, EV)
- Spot sizes — fixed vs. flexible (can a small vehicle take a large spot)?
- How many levels / floors?
- Payment / ticketing in scope, or only allocation?
- Reservations or first-come-first-served?
- Concurrent entries — multiple gates allocating simultaneously?

## Class Skeleton

```
VehicleType (Enum)              # MOTORCYCLE, CAR, TRUCK, ...
SpotSize  (Enum)                # SMALL, MEDIUM, LARGE, ...

Vehicle (Class)
    licensePlate
    type: VehicleType

ParkingSpot (Class)
    id
    size: SpotSize
    vehicle: Vehicle | None
    isAvailable()
    canFitVehicle(vehicle)
    parkVehicle(vehicle)
    removeVehicle()

ParkingLevel (Class)
    level: int
    spots: List[ParkingSpot]
    initializeSpots()
    findAvailableSpot(vehicle)
    findAvailableSpotsCount()

ParkingLot (Class)
    levels: List[ParkingLevel]
    parkVehicle(vehicle)        # routes to a level, then a spot
    removeVehicle(vehicle)
    getAvailableSpots()
```

## Design Notes

- **Spot-fit logic** belongs in `ParkingSpot.canFitVehicle` — encapsulates size rules in one place.
- **Allocation strategy** lives in `ParkingLevel.findAvailableSpot` — easy to swap (nearest, lowest level, balanced).
- **Concurrency**: if multiple gates allocate in parallel, guard spot assignment with per-spot lock or atomic CAS on `vehicle` field.
- **Extension points**: EV charging stations as a subclass of `ParkingSpot`; reservations as a layer on top of allocation.
