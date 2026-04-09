# This is the eraser code if you are facing any issues related to images then please user this code for more understanding

vehicle_category [icon: tag, color: purple] {
id int pk
name varchar
description varchar
}

vehicle [icon: truck, color: purple] {
id int pk
license_plate varchar
owner_name varchar
owner_phone varchar
vehicle_category_id int fk
created_at timestamp
}

access_category [icon: shield, color: blue] {
id int pk
name varchar
description varchar
}

parking_zone [icon: map, color: teal] {
id int pk
zone_code varchar
zone_name varchar
level_number int
description varchar
}

spot_category [icon: tag, color: teal] {
id int pk
name varchar
description varchar
}

parking_spot [icon: map-pin, color: teal] {
id int pk
spot_code varchar
parking_zone_id int fk
spot_category_id int fk
is_reserved boolean
is_active boolean
}

parking_ticket [icon: file-text, color: orange] {
id int pk
ticket_number varchar
vehicle_id int fk
access_category_id int fk
issued_at timestamp
status varchar
}

parking_session [icon: clock, color: orange] {
id int pk
parking_ticket_id int fk
parking_spot_id int fk
entry_time timestamp
exit_time timestamp
duration_minutes int
session_status varchar
}

payment [icon: credit-card, color: green] {
id int pk
parking_session_id int fk
amount decimal
payment_method varchar
payment_status varchar
paid_at timestamp
transaction_ref varchar
}

spot_availability_log [icon: activity, color: gray] {
id int pk
parking_spot_id int fk
parking_session_id int fk
event_type varchar
logged_at timestamp
}

vehicle_category.id < vehicle.vehicle_category_id
vehicle.id < parking_ticket.vehicle_id
access_category.id < parking_ticket.access_category_id
parking_ticket.id - parking_session.parking_ticket_id
parking_zone.id < parking_spot.parking_zone_id
spot_category.id < parking_spot.spot_category_id
parking_spot.id < parking_session.parking_spot_id
parking_session.id - payment.parking_session_id
parking_spot.id < spot_availability_log.parking_spot_id
parking_session.id < spot_availability_log.parking_session_id
