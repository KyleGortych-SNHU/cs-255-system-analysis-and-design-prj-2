# Project 2

This project showcases the use of plantuml in the process of diagram creation for the project. 

<details>
<summary>Click to view</summary>

<div align="center">
  <img src="https://raw.githubusercontent.com/KyleGortych-SNHU/cs-255-system-analysis-and-design-prj-2/refs/heads/main/activity1.png" width="100%" alt="img">
</div>

```bash
plantuml activity1.puml
```

```text
@startuml
skinparam dpi 300
start

:Customer logs into system;
:Selects "Manage Reservations";

if (Existing reservation?) then (yes)
    :View reservation details;
    if (Modify?) then (yes)
        :Modify reservation details;
        :Select driver and car;
    else (no)
        if (Cancel?) then (yes)
            :Cancel reservation;
        endif
    endif
else (no)
    :Create new reservation;
    :Select lesson package;
    :Choose lesson date and time;
    :Select driver and car;
endif

:System validates availability;
:System saves reservation;
:Confirmation sent to customer;

stop
@enduml
```

</details>

<details>
<summary>Click to view</summary>

<div align="center">
  <img src="https://raw.githubusercontent.com/KyleGortych-SNHU/cs-255-system-analysis-and-design-prj-2/refs/heads/main/activity2.png" width="100%" alt="img">
</div>

```bash
plantuml activity2.puml
```

```text
@startuml
skinparam dpi 300
start

:Customer accesses registration page;

if (Existing account?) then (yes)
    :Login with username and password;
    if (Forgot password?) then (yes)
        :Request password reset;
        :System sends reset link;
        :Customer sets new password;
    endif
else (no)
    :Fill in personal details (name, address, phone, etc.);
    :Provide credit card info;
    :Set username and password;
    :Submit registration;
endif

:System validates input;
:Account created or updated;
:Confirmation sent to customer;

stop
@enduml
```

</details>

<details>
<summary>Click to view</summary>

<div align="center">
  <img src="https://raw.githubusercontent.com/KyleGortych-SNHU/cs-255-system-analysis-and-design-prj-2/refs/heads/main/class.png" width="100%" alt="img">
</div>

```bash
plantuml class.puml
```

```text
@startuml
skinparam dpi 300
!theme reddress-darkblue
'=============================
' Users
'=============================
class User {
    +userID: int
    +username: String
    +password: String
    +role: String
    +login()
    +logout()
}

class Admin {
    +resetUserAccount(userID: int)
    +disablePackage(packageID: int)
}

class ITOfficer {
    +modifySystemData()
    +manageUserAccounts()
}

class Secretary {
    +scheduleReservation()
    +cancelReservation()
    +modifyReservation()
}

User <|-- Admin
User <|-- ITOfficer
User <|-- Secretary

'=============================
' Customers & Accounts
'=============================
class Customer {
    +customerID: int
    +firstName: String
    +lastName: String
    +address: String
    +phone: String
    +state: String
    +creditCardNumber: String
    +expirationDate: Date
    +securityCode: String
    +pickUpLocation: String
    +dropOffLocation: String
    +register()
    +login()
    +resetPassword()
}

Customer --> "1..*" Reservation

'=============================
' Lessons & Packages
'=============================
class Lesson {
    +lessonID: int
    +date: Date
    +startTime: Time
    +endTime: Time
    +driverComments: String
}

class Package {
    +packageID: int
    +name: String
    +description: String
    +hours: int
    +includeOnlineClass: Boolean
    +includeInPersonClass: Boolean
    +isEnabled: Boolean
}

Customer --> "0..*" Lesson
Lesson --> "1" Package

'=============================
' Reservations
'=============================
class Reservation {
    +reservationID: int
    +status: String
    +createDate: Date
    +modifyDate: Date
    +cancelDate: Date
    +assignDriver(driverID: int)
    +assignCar(carID: int)
}

Reservation --> "1" Customer
Reservation --> "1" Driver
Reservation --> "1" Car

'=============================
' Drivers & Cars
'=============================
class Driver {
    +driverID: int
    +name: String
    +licenseNumber: String
    +assignCar(carID: int)
}

class Car {
    +carID: int
    +plateNumber: String
    +model: String
    +capacity: int
}

Driver --> "1..*" Reservation
Car --> "1..*" Reservation

'=============================
' Tracking & Reports
'=============================
class ActivityLog {
    +logID: int
    +userID: int
    +action: String
    +timestamp: DateTime
    +reservationID: int
}

class Report {
    +reportID: int
    +generateActivityReport()
    +generateReservationReport()
}

ActivityLog --> User
ActivityLog --> Reservation
Report --> Reservation
Report --> ActivityLog

@enduml
```

</details>

<details>
<summary>Click to view</summary>

<div align="center">
  <img src="https://raw.githubusercontent.com/KyleGortych-SNHU/cs-255-system-analysis-and-design-prj-2/refs/heads/main/usecase.png" width="100%" alt="img">
</div>

```bash
plantuml usecase.puml
```

```text
@startuml
skinparam dpi 300
!theme cyborg
title DriverPass Use Case Diagram
left to right direction

' Actors
actor Customer
actor Secretary
actor Driver
actor "IT Officer" as IT
actor Owner
actor DMV

' DriverPass System
rectangle "DriverPass System" {

    'Registration & Package Purchase
    rectangle "Registration & Purchase" {
        usecase "Register / Provide Info" as UC1
        usecase "Buy Package" as UC15
        usecase "Schedule Appointment" as UC2
        usecase "Modify Appointment" as UC3
        usecase "Cancel Appointment" as UC4
    }

    'Online Learning & Student Progress
    rectangle "Online Learning & Progress" {
        usecase "Take Online Class" as UC5
        usecase "Take Practice Test" as UC6
        usecase "View Test Progress / Results" as UC7
        usecase "Reset Password" as UC8
    }

    'Internal Operations & DMV Updates
    rectangle "Internal Operations & Updates" {
        usecase "Enter Driver Notes" as UC9
        usecase "View Assigned Lessons" as UC10
        usecase "Manage User Accounts" as UC11
        usecase "Generate Activity Report" as UC12
        usecase "Receive DMV Updates" as UC14
    }
}

'Customer interactions
Customer --> UC1
Customer --> UC15
Customer --> UC2
Customer --> UC3
Customer --> UC4
Customer --> UC5
Customer --> UC6
Customer --> UC7
Customer --> UC8

'Secretary interactions
Secretary --> UC1
Secretary --> UC15
Secretary --> UC2
Secretary --> UC3
Secretary --> UC4

'Driver interactions
Driver --> UC9
Driver --> UC10

'IT Officer interactions
IT --> UC8
IT --> UC11

'Owner interactions
Owner --> UC7
Owner --> UC12

'DMV interactions
DMV --> UC14
DMV --> UC7

'Dependencies
UC15 ..> UC2 : <<include>>
@enduml
```

</details>

## Reflection
### Briefly summarize the DriverPass project. Who was the client? What type of system did they want you to design?

The project entail creating diagrams and an LMS system for the client DriverPass. The type of LMS they required needed to have role based permitions and security. 

### What did you do particularly well?

The creation of the diagrams I felt were done alright with the exception of the use case diagram. I feel my choice in Moodle as the LMS to use for the client works well with their goals. 

### If you could choose one part of your work on these documents to revise, what would you pick? How would you improve it?

I would go more indepth in the design and implementation for the security roles and features. The diagrams were a simple overview and could be more in-depth. 

### How did you interpret the user’s needs and implement them into your system design? Why is it so important to consider the user’s needs when designing?

I wrote down the users needs as an orginized list of the types of requirements. First are the functional requirements, then non-functional, buisness, and technical. This is important and also is used to look at the perspective of the diffrent users ie. client, customers, and admin users. 

It is important to ensure the system's post condtions meets the clients expectations not only for company integrity but also for security reasons. If the postconditions are diffrent or are not well understood by the client it will lead to possible incorrect workflows and insecure practices. 

### How do you approach designing software? What techniques or strategies would you use in the future to analyze and design a system?

Before consider if a system is to be created from scratch I look at exsiting tools that can be used first. This often can speed up development time and minimize costs.
