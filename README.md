# Modern Parking System - Kenya 🇰🇪

An intelligent, real-time parking management solution designed to handle automated slot allocation, entry/exit workflows, dynamic fee calculation, and auditable transaction logging.

---

## Module Algorithms

### 1. Slot Display Module
* **Algorithm**: Iterates sequentially through slot keys to generate the current availability mapping.
* **Time Complexity**: $O(N)$

### 2. Vehicle Entry Module
* **Algorithm**:
  1. Converts incoming license plate input to uppercase.
  2. Checks existing active records via Hash Map lookup ($O(1)$ time complexity).
  3. Performs a **Linear Search** ($O(N)$) across all slots to find and assign the first available parking bay.
  4. Registers the current entry timestamp and signals the entry barrier gate.

### 3. Vehicle Exit & Fee Calculation Module
* **Algorithm**:
  1. Performs an $O(1)$ lookup for active vehicle records.
  2. Computes duration based on elapsed parking time.
  3. Calculates the total parking fee in Kenyan Shillings (KES) using the formula:
     $$\text{Fee (KES)} = \max(\text{Base Fee}, \text{Hours} \times \text{Rate})$$
  4. Updates slot availability back to **Free** and triggers the exit barrier.

---

## Data Structures & Performance Rationale

| Data Structure | Variable / Object | Time Complexity | Justification |
| :--- | :--- | :--- | :--- |
| **Dictionary / Hash Map** | `active_vehicles` | $O(1)$ | Provides constant-time search, insertion, and deletion for active vehicle lookups during high-traffic operations. |
| **Dictionary / Array** | `parking_slots` | $O(1)$ access | Maps each slot ID directly to its occupancy status, enabling fast random access and sequential rendering on the visualizer grid. |

---

## Database Design (Schema Concept)

### 1. `Slots`
* **`slot_id`** *(Primary Key)* — Unique identifier for each parking bay.
* **`status`** — Current bay state (`Available` / `Occupied`).

### 2. `Vehicles`
* **`plate_number`** *(Primary Key)* — Vehicle registration number (e.g., `KCA 123X`).
* **`vehicle_type`** — Vehicle classification category.

### 3. `ParkingLogs`
* **`log_id`** *(Primary Key)* — Unique transaction log identifier.
* **`plate_number`** *(Foreign Key)* — References `Vehicles.plate_number`.
* **`slot_id`** *(Foreign Key)* — References `Slots.slot_id`.
* **`entry_time`** — Timestamp recorded upon entry.
* **`exit_time`** — Timestamp recorded upon payment/exit.
* **`total_fee_kes`** — Final fee charged in KES.
* **`payment_status`** — Transaction state (e.g., `Paid`, `Pending`).