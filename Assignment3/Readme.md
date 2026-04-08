User [icon: user, color: blue] {
  user_id PK
  name string
  phone string
  enum role "patient|doctor|staff"
  access_token string
  refresh_token string
  created_at timestamp
}

Patient [icon: user, color: green] {
  patient_id PK
  user_id FK
  name string
  phone string
  date date_of_birth
  parents_name string
  address_id FK
  email string
}

Doctor [icon: user-check, color: green] {
  doctor_id PK
  user_id FK
  phone string
  education string
  experience_years int
  profile_picture string
}

Staff [icon: briefcase, color: blue] {
  staff_id PK
  user_id FK
  email string(322)
  password_hash string
}

Address [icon: map-pin, color: purple] {
  address_id PK
  street string
  city string
  state string
  pincode string
  landmark string
}

Specialization [icon: tag, color: purple] {
  specialization_id PK
  working_field string
  doctor_type enum "Primary|Specialty|Surgical"
}

DoctorSpecialization [icon: tag, color: purple] {
  id PK
  doctor_id FK
  specialization_id FK
  is_active boolean
  is_main boolean
}

Appointment [icon: calendar, color: green] {
  appointment_id PK
  patient_id FK
  doctor_id FK
  scheduled_at datetime
  status enum "scheduled|completed|cancelled|no_show"
}

Visit [icon: activity, color: green] {
  visit_id PK
  appointment_id FK
  visited_at datetime
  chief_complaint text
  notes text
}

Prescription [icon: file-text, color: green] {
  prescription_id PK
  visit_id FK
  doctor_id FK
  prescribed_at timestamp
}

PrescriptionItem [icon: list, color: green] {
  item_id PK
  prescription_id FK
  medicine_name string
  dosage string
  frequency string
  duration string
  instructions text
}

DiagnosticTest [icon: thermometer, color: orange] {
  test_id PK
  test_name string
  category string
  price  decimal
}

PrescribedTest [icon: clipboard, color: orange] {
  prescribed_test_id PK
  visit_id FK
  test_id FK
  doctor_id FK
  notes text
  ordered_at timestamp
}

Report [icon: file, color: orange] {
  report_id PK
  prescribed_test_id FK
  patient_id FK
  report_name string
  file_link string
  outcome text
  generated_at timestamp
}

Bill [icon: credit-card, color: red] {
  bill_id PK
  visit_id FK
  total_amount decimal
  discount decimal
  net_payable decimal
  enum status "draft|pending|paid|cancelled"
  timestamp created_at
}

Payment [icon: dollar-sign, color: red] {
  payment_id PK
  bill_id FK
  amount_paid decimal
  payment_mode  enum "cash|card|upi|insurance"
  collected_by FK
  paid_at timestamp
}

PaymentLog [icon: list, color: red] {
  log_id PK
  payment_id FK
  status enum "initiated|success|failed|pending"
  amount decimal
  transaction_id string
  created_at timestamp
}

Refund [icon: rotate-ccw, color: red] {
  refund_id PK
  payment_id FK
  amount decimal
  reason text 
  refunded_at timestamp
}

User.user_id <> Patient.user_id
User.user_id <> Doctor.user_id
User.user_id <> Staff.user_id


Patient.address_id > Address.address_id

Doctor.doctor_id <> DoctorSpecialization.doctor_id
Specialization.specialization_id <> DoctorSpecialization.specialization_id

Patient.patient_id < Appointment.patient_id
Doctor.doctor_id < Appointment.doctor_id
Appointment.appointment_id - Visit.appointment_id

Visit.visit_id - Prescription.visit_id
Prescription.prescription_id < PrescriptionItem.prescription_id

Visit.visit_id < PrescribedTest.visit_id
DiagnosticTest.test_id < PrescribedTest.test_id
PrescribedTest.prescribed_test_id - Report.prescribed_test_id
Patient.patient_id < Report.patient_id

Visit.visit_id - Bill.visit_id
Bill.bill_id < Payment.bill_id
Staff.staff_id < Payment.collected_by
Payment.payment_id < PaymentLog.payment_id
Payment.payment_id < Refund.payment_id 
