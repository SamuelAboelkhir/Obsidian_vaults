```SQL
Table TaskInstance {
  id number // 7 digits numeric ID
  subTaskId number // short repeatable sequence of numbers (0, 1, 10). Meaning unclear
  executionSequence number // should represent the order in which the tasks are to be performed. Needs clarification
 
  categoryCode string // top level organization of data, including: Pickup, Delivery, Transit, and Shunting
  groupCode string // a lower level organization of data, where categories are split a bit more
  typeCode string // a code representing the type of activity. This is the lowest level representation of the data, where every task is explicitly mentioned. Codes starting with 10XX are pickups, 30XX are pickup-delivery, 40XX are deliveries
 
  orderId number [null, ref: > Orders.id]

  actions Actions [ref: > Actions.id]
  taskFlags TaskFlags [ref: > TaskFlags.taskId]
  status TaskStatus
  startTime datetime
  actualStartTime datetime
  endDate datetime
  actualEndDate datetime

  stopId uuid [ref: > Stops.stopId]
}

Table TaskAssignment {
  id number
  taskId TaskInstance [ref: < TaskInstance.id]
  startingTerminal string
  endTerminal string
  route Rotes [ref: - Routes.ROUTE_NUMBER]
  assignedTruck Trucks [ref: - Trucks.id]
  assignedDriver Drivers [ref: - Drivers.id]
}

Table TaskLocationSnapshot {
  taskId TaskInstance [ref: < TaskInstance.id]
  customerCode number
  customerName string
  addressText string
  city string
  postalCode string
  country string
}

Table Drivers {
  id number
}

Table Trucks {
  id number
}

Table Orders {
  id number [pk] // 7 digits numeric ID
  typeCode string
}

Table Actions {
  id number [pk]
  subAction number
  typeCode string
}

Table Trips {
  tripId uuid [pk]
  driverId  Drivers [ref: > Drivers.id]
  state                int
  FDR FDR [ref: > FDR.id]
}

Table Stops {
  stopId               uuid        [pk]
  type                 varchar
  client               varchar
  addressName          varchar
  street               varchar
  city                 varchar
  district             varchar
  country              varchar
  postalCode           varchar
  latitude             decimal
  tripId Trips [ref: > Trips.tripId]
}

Table TrucksDrivers {
  truckId number [ref: <> Trucks.id]
  driverId number [ref: <> Drivers.id]
}

Table Routes {
  ROUTE_NUMBER number
  DISP_ROUTE_NUMBER number
  ROUTE_TERMINAL string
}

Table Activities {
  id number [pk]
  FDRId FDR [ref: > FDR.id]
  driverId Drivers [ref: > Drivers.id]
}

Table FDR {
  id number [pk]
  driverId Drivers [ref: > Drivers.id]
  start Activities [ref: - Activities.id]
  end Activities [ref: - Activities.id]
}

Table TaskFlags {
  taskId number

  isLTL boolean
  isLoadAndGo boolean
  isJIT boolean
  isOSD boolean
}

Enum TaskStatus {
  InCreation
  New
  Assigned
  InProgress
  Canceled
  Completed
}
```