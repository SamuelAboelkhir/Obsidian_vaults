# Tasks
```SQL
Table Tasks {
  id number // 7 digits numeric ID
  subTask number // short repeatable sequence of numbers (0, 1, 10). Meaning unclear
  sequence number // should represent the order in which the tasks are to be performed. Needs clarification
  TASK_LINK number //meaning???
  categoryCode string // top level organization of data, including: Pickup, Delivery, Transit, and Shunting
  groupCode string // a lower level organization of data, where categories are split a bit more
  typeCode string // a code representing the type of activity. This is the lowest level representation of the data, where every task is explicitly mentioned. Codes starting with 10XX are pickups, 30XX are pickup-delivery, 40XX are deliveries
  deliveryFlag DeliveryFlag
}
```

| id           | TSDFF01.TASK_NUMBER                                       | workflow.name  |
| ------------ | --------------------------------------------------------- | -------------- |
| subTask      | TSDFF01.TASK_SUB_NUMBER                                   |                |
| sequence     | TSDFF01.TASK_SEQUENCE                                     |                |
| TASK_LINK    | TSDFF01.TASK_LINK                                         |                |
| categoryCode | TSDFF01.TASK_CAT_CODE                                     |                |
| groupCode    | TSDFF01.TASK_TGRP_CODE                                    |                |
| typeCode     | TSDFF01.TASK_TYPE_CODE                                    |                |
| deliveryFlag | TSDFF01.ORDER_LTL_FLAG/LOAD_AND_GO_FLAG/JIT_FLAG/OSD_FLAG |                |
| customer     | TSDFF01.CUSTOMER_CODE                                     |                |
| order        | TSDFF01.ORDER_NUMBER                                      |                |
| actions      | TSDFF01.ACTION_NUMBER                                     |                |
| routes       | TSDFF01.ROUTE_NUMBER                                      |                |
| status       | TSDFF01.TASK_STATUS_CODE                                  | workflow.state |

```SQL
Table Orders {
  id number // 7 digits numeric ID
  typeCode string
}

Table Actions {
  id number
  subAction number
  typeCode string
}

Table Customers {
  CUSTOMER_CODE string
  dockID number
  CUSTOMER_CLASS number //What is it exactly?
  CUSTOMER_TEXT string //What is it exactly?
  phoneNumber string
  frenchPhoneNumber string
  PHONE_NUMBER_800 string
  fax string
  frenchFAX string
  typeCode string
  customerTerminalCode string
  customerInvoiceType string
}

Table Address {
  ADDRESS_TEXT string
  ADDRESS_TEXT1 string
  CITY_CODE string
  CITY_TEXT string
  PROV_CODE string
  POSTAL_CODE string
  ZIP_CODE string
  COUNTRY_CODE_2 string
}

Table Routes {
  ROUTE_NUMBER number
  DISP_ROUTE_NUMBER number
  ROUTE_TERMINAL string
}

Enum DeliveryFlag {
  ORDER_LTL_FLAG
  LOAD_AND_GO_FLAG
  JIT_FLAG
  OSD_FLAG
}
```
# Trucks
```SQL
Table Truck {
id string [ref: - TKDFF01.TRUCK_CODE, ref: - vehicles.vehicleNo]
owner_code string [ref: - TKDFF01.TRUCK_OWNER_CODE]
carrier_code string [ref: - TKDFF01.TRUCK_CARRIER_CODE]
terminal_code string [ref: - TKDFF01.TERMINAL_CODE, ref: - vehicles.siteId]
brand string [ref: - TKDFF01.TRUCK_BRAND]
model string [ref: - TKDFF01.TRUCK_MODEL, ref: - vehicles.modelYear]
year number [ref: - TKDFF01.TRUCK_YEAR, ref: - vehicles.modelYear]
axle_count number [ref: - TKDFF01.TRUCK_AXLE]
engine string [ref: - TKDFF01.TRUCK_ENGINE, ref: - vehicles.engineTypeId]
license_number string [ref: - TKDFF01.TRUCK_LICENSE_NUMBER, ref: - vehicles.licensePlate]
license_province string [ref: - TKDFF01.LICENSE_PROVINCE_CODE]
gps_latitude number [ref: - TKDFF01.GPS_LATITUDE, ref: - vehicle_positions.latitude]
gps_longitude number [ref: - TKDFF01.GPS_LONGITUDE, ref: - vehicle_positions.longitude]
status_code string [ref: - TKDFF01.TRUCK_STATUS_CODE]
}
```

```SQL
Table TruckMaintenance {
truck_id string [ref: - TKDFF01.TRUCK_CODE]
type_code string [ref: - TKDFF01.TRUCK_TYPE_CODE]
date_next date [ref: - TKDFF01.MAINTENANCE_DATE_1_NEXT]
date_last date [ref: - TKDFF01.MAINTENANCE_DATE_1_LAST]
odometer_last number [ref: - TKDFF01.MAINTENANCE_ODOMETER_1_LAST]
}
```

| id            | TKDFF01.TRUCK_CODE         | vehicles.vehicleNo                               |
| ------------- | -------------------------- | ------------------------------------------------ |
| owner_code    | TKDFF01.TRUCK_OWNER_CODE   |                                                  |
| carrier_code  | TKDFF01.TRUCK_CARRIER_CODE |                                                  |
| terminal_code | TKDFF01.TERMINAL_CODE      | vehicles.siteId                                  |
| brand         | TKDFF01.TRUCK_BRAND        |                                                  |
| model         | TKDFF01.TRUCK_MODEL        |                                                  |
| year          | TKDFF01.TRUCK_YEAR         | vehicles.modelYear                               |
| axle_count    | TKDFF01.TRUCK_AXLE         |                                                  |
| engine        | TKDFF01.TRUCK_ENGINE       | engineType.engineTypeId == vehicles.engineTypeId |
|               |                            |                                                  |
