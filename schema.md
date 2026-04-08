# ERD Source Code — Clinic Platform

Copy this into [Eraser.io](https://eraser.io) to view or edit the diagram.

```entity-relationship-diagram
patients [icon: users, color: blue] {
  id string pk
  fullName varchar(50) not null
  email varchar(15) unique not null
  number varchar(15) unique not null
  emergency_contact_phone varchar(15)
  date_of_birth  date
  gender enum[male, female, other]
  blood_group enum[show all bloog groop]
  address_line1 text
  city varchar(20)
  state varchar(20)
  pincode varchar(10)
  created_at timestamp
  updated_at timestamp
}

doctor [icon: doctor, color: green] {
  id serial pk
  department_id int fk
  first_name VARCHAR(50)
  last_name VARCHAR(50)
  phone varchar(20)
  email varchar(50)
  consultation_fee decimal(10,2)
  available_days varchar(50)
  specialty varchar(20)
  qualification	varchar(50)
  license_number varchar(50) not null
  bio text
  profile_image_url text
  is_active boolean default(true)
  created_at timestamp
  updated_at timestamp
}


department [icon: home] {
  id string
  name varchar(50) unique not null
  description varchar(200)
  is_active boolean default(true)
  created_at timestamp
}


appointment [icon: adjust, color:yellow] {
  id serial pk
  patient_id int fk
  doctor_id  int fk
  scheduled_at datetime
  duration_minutes varchar(30)
  type enum ['walk_in','online','emergency']
  status enum ['scheduled','in_progress','completed','cancelled']
  cancellation_reason text 
  cancelled_by enum ['patient','doctor','clinic' ]
  booked_by enum ['patient','receptionist']
  notes text
  created_at timestamp
  updated_at timestamp
}

consultation [icon: sparkles] {
  id serial pk
  appointment_id int fk
  patient_id int fk
  doctor_id int fk
  type enum [online, offline]
  started_at datetime
  ended_at datetime
  symptoms text 
  diagnosis varchar[all diagnosis test] 
//   (if not matched then store as that)
  treatment_plan text 
  follow_up_in_days varchar(10)
  created_at timestamp
}


diagnostic_tests [icon: test-tube, color: red] {
  id serial pk
  test_name	varchar(150)
  description text
  test_code varchar(20)
  category enum ['blood','urine','imaging','cardiology','microbiology']
  sample varchar(50)
  time_taken varchar(20)
  price decimal(10,2)
  instructions text
  is_active boolean default[true]
  created_at timestamp
}

prescription [icon: test-tube, color: orange] {
  id serial pk
  consultation_id int fk
  diag_test_id  int fk
  instructions text
  test_name 	varchar(150)
  description text
  created_at timestamp
}

report[icon: report, color: purple]{
  id serial pk
  prescription_id int fk
  patient_id int fk
  staff_id int fk
// (stuff id get from every clinic)
  status enum ['pending','processing','completed','amended','cancelled'] 
  file_url text
  created_at timestamp
  updated_at timestamp
}

payment[icon: payment, color: green]{
  id serial pk 
  appointment_id int fk
  patient_id int fk
  invoice_number varchar(30)
  consultation_fee decimal(10,2)
  diagnostics_subtotal decimal(10,2)
  discount_amount decimal(10,2)
  tax_amount decimal(10,2)
  total_amount  decimal(10,2)
  paid_amount decimal(10,2)
  due_amount decimal(10,2)
  payment_status enum ['unpaid','partial','paid','refunded']
  payment_method enum ['cash','card','upi','netbanking','insurance']
  paid_at datetime
  notes text
  created_at timestamp
  updated_at timestamp
}

department.id -doctor.department_id
patients.id < appointment.patient_id
doctor.id <appointment.doctor_id
appointment.id - consultation.appointment_id
patients.id <consultation.patient_id
doctor.id < consultation.doctor_id
consultation.id - prescription.consultation_id
diagnostic_tests.id > prescription.diag_test_id
prescription.id - report.prescription_id
patients.id < report.patient_id
patients.id < payment.patient_id
appointment.id < payment.appointment_id
```
