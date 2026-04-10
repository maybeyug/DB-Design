# Comic con Parking System

Web Dev Cohort 2026

Hi!<br>
This is my approach to designing the DB for given task. <br>

[Smart_Elevator_Design](./smart_elevator_design.png)

<br>

## Entity_Relation Diagram 


- buildings
- floors
- elevators
- elevator shaft
- floor requests
- ride assignments
- ride logs
- elevator status tracking
- maintenance tracking

- JuctionTable -> elevator_floor

### DBML Code
```
title 05_smart_elevator_control
colorMode pastel
styleMode shadow

building[color:blue]{
  id int pk
  name varchar(40)
  location varchar(20)
  type enum['mall','hospital','office']
  total_floors int 

  createdAt timestamp
}

floor[color: Orange]{
  id int pk
  buidling_id int fk
  floor_number int
  is_accessable boolean deafult true
}

building.id < floor.buidling_id: [color: orange]


elevator [color: Green]{
  id int pk
  buidling_id int fk
  model_name varchar(30)
  max_capacity int
  max_weight int
  shaft_id int fk

  installed_at timestamp
}
building.id < elevator.buidling_id: [color: green]


elevator_shaft[color: Purple] {
  id int pk
  buidling_id int fk
  shaft_info varchar(20)
}
building.id < elevator_shaft.buidling_id: [color: purple]
elevator_shaft.id - elevator.shaft_id: [color: purple]

floor_request[color: Yellow] {
  id int pk
  floor_id int fk
  direction enum['up','down']
  status enum['pending','assigned']
  request_time timestamp
}
floor.id < floor_request.floor_id: [color: yellow]

ride_assigned[color: Orange] {
  id int pk
  request_id int fk
  elevator_id int fk
  status enum['in_progress','completed','failed']
  assigned_time timestamp
}
floor_request.id - ride_assigned.request_id: [color: orange]
elevator.id < ride_assigned.elevator_id: [color: orange]

elevator_status_tracking[color: Red] {
  id int pk
  elevator_id int fk
  status enum['idle','moving','maintenance']
  floor_id int fk
  last_updated timestamp
}

elevator.id < elevator_status_tracking.elevator_id: [color: black]
floor.id < elevator_status_tracking.floor_id: [color: orange]

elevator_floor[color: Yellow] {
  id int pk
  floor_id int fk
  elevator_id int fk
}
floor.id > elevator_floor.floor_id: [color: yellow]
elevator.id > elevator_floor.elevator_id: [color: yellow]

ride_logs[color: Yellow] {
  id int pk
  assigned_id int fk
  log_details varchar(300)
  duration int
  updated_at timestamp
}
ride_assigned.id < ride_logs.assigned_id

maintenance_tracking [color: Purple] {
  id int pk
  elevator_id int fk
  issue varchar(250)
  last_maintenance timestamp
  start_time timestamp
  end_time timestamp
}

elevator.id < maintenance_tracking.elevator_id: [color: green]

```
