# Comic con Parking System

Web Dev Cohort 2026

Hi!<br>
This is my approach to designing the DB for given task. <br>

[Parking_system_design](./parking_system.png)

<br>

## Entity_Relation Diagram 

- vehicle
- vehicle_category
- parking_level
- parking_spot_category
- parking_spots
- parking_ticket
- parking_session
- payment

### DBML Code
```
title 04_Comic_con_parking_system
colorMode pastel
styleMode shadow

vehicle_category[icon: filter]{
  id int pk
  name varchar(50) not null
  price_per_hour int
  max_height int
  max_weight int
}

vehicle[icon: car, color: green]{
  id int pk
  registration_number varchar(20) unique
  model_name varchar(50)
  owner_name varchar(50)
  owner_phone char(15)
  category_id int fk

  createdAt timestamp
  updatedAt timestamp
}

// one category can have mulitple vehicle
vehicle_category.id < vehicle.category_id: [color: green]

parking_level[icon: building, color:blue]{
  id int pk
  level_number int 
  name varchar(10)

  createdAt timestamp
  updatedAt timestamp
}

parking_spot_category[icon: parking, color:purple]{
  id int pk
  level_id int fk
  name varchar(40)
  vehicle_category_id int fk
  capacity int 

  createdAt timestamp
}

// one level can have many categories parking spot
parking_level.id < parking_spot_category.level_id

// for One category of vehicle there can be many spot category
vehicle_category.id < parking_spot_category.vehicle_category_id


parking_spot [icon: parking,color:orange]{
  id int pk
  spot_category_id int fk 
  spot_number varchar(10) unique
  is_reserved boolean
  reserved_for enum['vip','staff','exhibitor']
  last_occupied_at timestamp
}

//one category can have multiple parking spots
parking_spot_category.id < parking_spot.spot_category_id

parking_ticket [icon: ticket, color:blue]{
  id int pk
  status enum['active','closed']
  access_type enum['vip','staff','exhibitor','normal']
  vehicle_id int fk
  entry_gate varchar(20)
  entry_time timestamp
}

//one vehicle can have many parking tickets [revisiting next day]
vehicle.id < parking_ticket.vehicle_id: [color: green]

parking_session[icon: hourglass, color:green]{
  id int pk
  ticket_id int fk
  spot_id int fk
  status enum['ongoing','completed']
  exit_time timestamp
  calculated_fare int 
  createdAt timestamp
}

// One ticket - one session
parking_ticket.id - parking_session.ticket_id: [color: green]

// One parking_spot can have multiple sessions throughout the day 
parking_spot.id < parking_session.spot_id

payment[icon:money, color:red]{
  id int pk
  session_id int fk
  status enum['pending','successful','failed']
  amount_paid int
  txn_number varchar(50)
  mode_of_payment varchar(20)
  bank_name varchar(20)
  createdAt timestamp
}

// one ticket session can have multiple payment if previous payment got failed 
parking_session.id  < payment.session_id: [color: red]

```
