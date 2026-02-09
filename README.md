# Database-Systems

```
hospital-management-system-db/
│
├── database/
│   ├── hospital_system_2025-12-08_165102.sql  
│   └── schema_diagram.png                     

```

---


```markdown
#  Hospital Management System Database

A comprehensive MySQL database system for hospital management, featuring audit logging, role-based access control, automated triggers, and backup mechanisms.

##  Features

- **Patient Management**: Complete patient records with demographics and medical history
- **Doctor & Staff Management**: Doctor assignments and hospital affiliations
- **Medical Workflow**: Visits, medications, conditions, and lab test results
- **Billing System**: Insurance provider tracking with audit logging
- **Audit & Security**: Comprehensive audit trails for all critical tables
- **Role-Based Views**: Customized views for doctors, lab technicians, and billing clerks
- **Automated Triggers**: For data integrity and audit logging
- **Backup System**: Separate backup tables for data recovery

##  Database Schema

### Main Tables
- `patients` - Patient personal information
- `doctors` - Doctor details and system user accounts
- `visits` - Hospital visit records
- `medicalconditions` - Patient medical conditions
- `medications` - Prescribed medications with audit trail
- `testresults` - Laboratory test results
- `billing` - Billing information with insurance details
- `audit_log`, `billingaudit`, `medicationaudit`, `patientaudit` - Audit tables

### Key Relationships
- One-to-Many: Patients → Visits
- One-to-Many: Doctors → Visits
- One-to-Many: Visits → Medications, TestResults, Billing
- Audit trails maintained via triggers on insert/update operations



##  Security Features

### Audit Triggers
- Automatic logging of all changes to `patients`, `medications`, and `billing` tables
- Timestamped audit trails with user/doctor identification

### Role-Based Access
- **DoctorView**: Patient medical records and visit history
- **BillingClerkView**: Billing information and insurance details
- **LabTechView**: Laboratory test results

### Backup System
- Dedicated backup tables for critical data
- Schema identical to production tables for easy restoration

##  Advanced Features

### Automated User Creation
```sql
-- Trigger automatically creates system user when new doctor is added
CREATE TRIGGER after_doctor_insert AFTER INSERT ON doctors
FOR EACH ROW BEGIN
    INSERT INTO SystemUsers (Username, Password, Role, DoctorID)
    VALUES (CONCAT('doc_', NEW.DoctorID), SHA2('temp_password', 256), 'Doctor', NEW.DoctorID);
END;
```

### Data Validation
- Foreign key constraints maintain referential integrity
- Unique constraints prevent duplicate records
- Check constraints (where applicable) for data validation

##  Performance Considerations

- Indexed foreign keys for faster joins
- Appropriate data types for storage optimization
- Normalized schema to 3NF to reduce redundancy
- Views for complex queries without performance overhead

##  License

This project is available for educational and portfolio purposes. Please ensure compliance with data privacy regulations (HIPAA/GDPR) if using with real patient data.

##  Contributing

This is a portfolio project. Suggestions and improvements are welcome via Issues and Pull Requests.


**Note**: This database contains sample/dummy data for demonstration purposes only.
```

---

## ** Additional Files to Create**

### **documentation/ER_DIAGRAM.md**
```markdown
# Entity Relationship Diagram

## Entities

### Patients
- PatientID (PK)
- Name, Age, Gender, BloodType

### Doctors  
- DoctorID (PK)
- DoctorName

### Visits
- VisitID (PK)
- PatientID (FK)
- DoctorID (FK)
- DateOfAdmission, DischargeDate, RoomNumber, AdmissionType

### MedicalConditions
- ConditionID (PK)
- PatientID (FK)
- ConditionName

### Medications
- MedicationID (PK)
- VisitID (FK)
- DoctorID (FK)
- MedicationName

### Billing
- BillingID (PK)
- VisitID (FK, Unique)
- BillingAmount
- InsuranceProvider

## Relationships
1. **Patient** → **Visit** (1:M)
2. **Doctor** → **Visit** (1:M)  
3. **Visit** → **Medication** (1:M)
4. **Visit** → **TestResult** (1:M)
5. **Visit** → **Billing** (1:1)
6. **Patient** → **MedicalCondition** (1:M)
```



