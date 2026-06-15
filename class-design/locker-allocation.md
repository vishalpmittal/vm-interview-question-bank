# Amazon / Costco Locker Allocation System

## Prompt

Design a locker allocation system for package pickup (Amazon Locker, Costco). Customers' packages are delivered to lockers at pickup locations; customers retrieve them with a code. The system allocates the smallest fitting locker.

## Clarifying Questions

- Locker sizes — fixed set or continuous?
- Multiple pickup locations, or single?
- Allocation strategy — smallest fit, nearest location to customer?
- Hold duration / expiry — what happens if the customer doesn't pick up?
- Notification flow in scope?
- Concurrent allocations from multiple incoming packages?

## Class Skeleton

```
LockerSize (Enum)              # SMALL, MEDIUM, LARGE, XL

Locker (Class)
    id
    size: LockerSize
    package: Package | None
    isAvailable()

Package (Class)
    id
    size: LockerSize
    customerId

PickupLocation (Class)
    id
    address
    lockers: List[Locker]
    findAndAllocate(package) -> Locker | None    # smallest fitting available
    releaseLocker(lockerId)
    checkLockerStatus(lockerId)

# Attributes shared across types: size of package and size of locker
# Allocation rule: locker.size >= package.size
```

## Design Notes

- **Smallest-fit allocation**: sort available lockers by size ascending and pick first that fits — minimizes wasted capacity.
- **Concurrency**: multiple packages may arrive simultaneously; per-locker lock or atomic CAS on `package` field prevents double-allocation.
- **Expiry**: background sweeper releases lockers whose holds have expired — also needs to notify customer / re-route package.
- **Multi-location routing**: caller picks `PickupLocation` (nearest to customer) before allocation — keeps the locker class location-agnostic.
- **Extension points**:
  - Temperature-controlled lockers (frozen, refrigerated) as a subclass.
  - Reservations (pre-allocate before delivery arrives).
  - Capacity reporting per location for routing decisions.
