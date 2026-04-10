https://app.eraser.io/workspace/T8n6E3ksgB1hRzz1gz2p?diagram=wK3ZZGY0DapxihWVq8-Ii


entity-relationship-diagram
title LiftGrid Systems – Elevator Control Platform

buildings [icon: building, color: blue] {
  building_id serial pk
  name varchar(100) not null
  city varchar(100) not null
  total_floors int not null
  status varchar(20) default "active"
  created_at timestamp
}

floors [icon: layers, color: blue] {
  floor_id serial pk
  building_id int fk
  floor_number int not null
  floor_label varchar(20) not null
  is_lobby boolean default false
}

elevators [icon: trending-up, color: teal] {
  elevator_id serial pk
  building_id int fk
  elevator_code varchar(50) not null
  capacity_persons int default 10
  current_floor int default 0
  current_status varchar(20) default "idle"
  is_active boolean default true
  created_at timestamp
}

elevator_floor_map [icon: git-merge, color: teal] {
  map_id serial pk
  elevator_id int fk
  floor_id int fk
}

floor_requests [icon: arrow-up-circle, color: orange] {
  request_id serial pk
  building_id int fk
  origin_floor_id int fk
  destination_floor_id int fk
  direction varchar(10) not null
  status varchar(20) default "pending"
  requested_at timestamp
}

ride_allocations [icon: shuffle, color: orange] {
  allocation_id serial pk
  request_id int fk
  elevator_id int fk
  allocated_at timestamp
}

ride_logs [icon: list, color: green] {
  ride_id serial pk
  elevator_id int fk
  allocation_id int fk
  pickup_floor_id int fk
  dropoff_floor_id int fk
  status varchar(20) default "completed"
  pickup_at timestamp
  dropoff_at timestamp
}

maintenance_records [icon: tool, color: red] {
  record_id serial pk
  elevator_id int fk
  type varchar(30) not null
  status varchar(20) default "scheduled"
  description text
  scheduled_at timestamp
  completed_at timestamp
}

buildings.building_id < floors.building_id
buildings.building_id < elevators.building_id
buildings.building_id < floor_requests.building_id

elevators.elevator_id < elevator_floor_map.elevator_id
floors.floor_id < elevator_floor_map.floor_id

floors.floor_id < floor_requests.origin_floor_id
floors.floor_id < floor_requests.destination_floor_id

floor_requests.request_id - ride_allocations.request_id
elevators.elevator_id < ride_allocations.elevator_id

ride_allocations.allocation_id < ride_logs.allocation_id
elevators.elevator_id < ride_logs.elevator_id
floors.floor_id < ride_logs.pickup_floor_id
floors.floor_id < ride_logs.dropoff_floor_id

elevators.elevator_id < maintenance_records.elevator_id