# Clinic Appointment & Diagnostics Platform
 
A database design project for a modern clinic that manages doctors, patients, appointments, consultations, diagnostic tests, reports, and payments.
 
> **Web Dev Cohort 2026 — Databases Assignment**  
> Due: Apr 9, 2026
 
---
 
## What This Covers
 
The schema is built to answer real business questions:
 
- Which doctor attended which patient, and when?
- Did an appointment lead to a consultation?
- Were any diagnostic tests prescribed during the visit?
- What reports were generated, and for whom?
- How are payments tied to appointments?
 
---
 
## ER Diagram
 
![Clinic ER Diagram](https://res.cloudinary.com/YOUR_CLOUD_NAME/image/upload/v1/YOUR_IMAGE_PUBLIC_ID.png)
 
> _Replace the Cloudinary URL above with your actual uploaded image link._
 
---
 
## Tables at a Glance
 
| Table | Purpose |
|---|---|
| `patients` | Core patient identity and contact info |
| `department` | Clinic departments (Cardiology, Dermatology, etc.) |
| `doctor` | Doctors and their specialties, linked to departments |
| `appointment` | A booked slot — may or may not result in a consultation |
| `consultation` | The actual doctor–patient visit that happened |
| `diagnostic_tests` | Master catalog of all available tests |
| `prescription` | Tests ordered during a consultation |
| `report` | Lab/diagnostic report generated from a prescription |
| `payment` | Payment record linked to an appointment |
 
---
 
## Key Design Decisions
 
- **Appointment ≠ Consultation.** An appointment is just a booking. A consultation only exists if the patient actually showed up. This is a 1:1 relationship, but not every appointment has one.
- **Tests belong to consultations.** A doctor prescribes tests during a visit, not at booking time.
- **Prescription is a bridge.** Each prescription row links one consultation to one diagnostic test. Multiple tests = multiple prescription rows.
- **Reports link back to prescriptions.** A report is the result of a specific prescription, and is also denormalized to the patient for fast lookup.
 
---
 
## ERD Source Code
 
The full ERD source (Eraser-compatible) is in [`schema.md`](./schema.md).
 
---
 
## Relationships
 
```
department     ||--o{  doctor          (one dept → many doctors)
patients       ||--o{  appointment     (one patient → many appointments)
doctor         ||--o{  appointment     (one doctor → many appointments)
appointment    ||--||  consultation    (one appointment → one consultation)
patients       ||--o{  consultation    (one patient → many consultations)
doctor         ||--o{  consultation    (one doctor → many consultations)
consultation   ||--||  prescription    (one consultation → one prescription)
diagnostic_tests ||--o{ prescription   (one test → many prescriptions)
prescription   ||--||  report          (one prescription → one report)
patients       ||--o{  report          (one patient → many reports)
patients       ||--o{  payment         (one patient → many payments)
appointment    ||--o{  payment         (one appointment → many payments)
```
