# Clinic Appointment and Diagnostics Platform

Web Dev Cohort 2026

Hi!<br>
This is my approach to designing the DB for given task. <br>

[Clinic_platform](./ClinicAppointment.png)

<br>

## Entity_Relation Diagram 

- Patient
- Doctor
- Appointment
- consultation
- Diagnostic Test
- Reports
- payments

### DBML Code
```
title 03_clinic_appointment_andDiagnostics_platform
colorMode pastel
styleMode shadow
 
doctor [icon:hospital, color:blue]{
 id int pk
 name varchar(40) not null
 phone char(12) not null
 experience int 
 speciality varchar(50)
 consultation_fee int 
 is_available boolean 

 createdAt timestamp
 updatedAt timestamp
}

patient [icon:person, color:green]{
  id int pk
  name varchar(20) not null
  age char(4)
  gender varchar(10)
  phone char(12)
  appointment_id int fk
  doctor_id int fk
  history varchar(100)
  total_visits int
  
  createdAt timestamp
  updatedAt timestamp
}
doctor.id <> patient.id: [color: blue]

appointment [icon: calendar-check]{
  id int pk
  patient_id int fk
  patient_name varchar(30)
  date_time Date 
  doctor_name varchar(20)
  doctor_id int fk
  status ['confirmed','cancelled']

  createdAt timestamp
  updatedAt timestamp
}

doctor.id < appointment.id
patient.id < appointment.id: [color: green]

consultation [icon: video, color:yellow]{
  id int pk
  appointment_id int fk
  consultation_fee int
  payment_id int fk
  type varchar(20) //follow-up, first-time
  doctor_notes text
  test_prescribed boolean
  test_name text
  next_visit Date

  createdAt timestamp
}
appointment.id - consultation.id
patient.id < consultation.appointment_id: [color: green]

diagnostic_test[icon: aws-health-dashboard, color: purple]{
  id serial pk
  name varchar(30)
  amount int 
  appointment_id int fk
  report_id iunt fk
  payment_id int fk
  createdAt timestamp
} 
consultation.appointment_id  < diagnostic_test.id

reports[icon:report, color:green]{
  id int pk
  patient_id int fk
  patient_name varchar(20)
  description
  createdAt timestamp
}
patient.id < reports.id
diagnostic_test.report_id < reports.id: [color: purple]

payments[icon: money, color: blue]{
  id int pk 
  txn_id int
  patient_id int fk
  amount int 
  method varchar(20)
}
payments.id < reports.id: [color: blue]
consultation.payment_id - payments.id: [color: yellow]



```
