# Airbnb Clone - Data Flow Diagrams

## Context Level (Level 0)

```mermaid
graph TD
    Users[External Users]
    PaymentGW[Payment Gateway]
    EmailSvc[Email Service]
    Storage[File Storage]

    subgraph Airbnb Clone System
        MainProcess((Airbnb<br/>Backend<br/>System))
    end

    Users -->|User Data/Requests| MainProcess
    MainProcess -->|Responses/Notifications| Users
    MainProcess <-->|Payment Processing| PaymentGW
    MainProcess -->|Send Emails| EmailSvc
    MainProcess <-->|Store/Retrieve Files| Storage
```

## Level 1 (Main Processes)

```mermaid
graph TD
    %% External Entities
    Users[Users]
    PaymentGW[Payment Gateway]
    EmailSvc[Email Service]

    %% Main Processes
    UserMgmt((User<br/>Management))
    PropMgmt((Property<br/>Management))
    BookingMgmt((Booking<br/>Management))
    PaymentProc((Payment<br/>Processing))
    ReviewSys((Review<br/>System))

    %% Data Stores
    UsersDB[(Users DB)]
    PropertiesDB[(Properties DB)]
    BookingsDB[(Bookings DB)]
    PaymentsDB[(Payments DB)]
    ReviewsDB[(Reviews DB)]

    %% Data Flows
    Users -->|Registration/Login| UserMgmt
    UserMgmt <-->|Store/Retrieve| UsersDB
    
    Users -->|Create/Search Listings| PropMgmt
    PropMgmt <-->|Store/Retrieve| PropertiesDB
    
    Users -->|Make Bookings| BookingMgmt
    BookingMgmt <-->|Store/Retrieve| BookingsDB
    
    BookingMgmt -->|Process Payment| PaymentProc
    PaymentProc <-->|Payment Data| PaymentGW
    PaymentProc <-->|Store/Retrieve| PaymentsDB
    
    Users -->|Submit Reviews| ReviewSys
    ReviewSys <-->|Store/Retrieve| ReviewsDB
    
    %% Notifications
    UserMgmt -->|Send Notifications| EmailSvc
    BookingMgmt -->|Send Confirmations| EmailSvc
    PaymentProc -->|Send Receipts| EmailSvc
```

## Level 2 (Detailed Process: Booking Management)

```mermaid
graph TD
    %% External Entities
    Guest[Guest]
    Host[Host]
    
    %% Processes
    SearchProps((Search<br/>Properties))
    CheckAvail((Check<br/>Availability))
    CreateBook((Create<br/>Booking))
    UpdateStatus((Update<br/>Status))
    NotifyUsers((Send<br/>Notifications))
    
    %% Data Stores
    PropertiesDB[(Properties DB)]
    BookingsDB[(Bookings DB)]
    UsersDB[(Users DB)]
    
    %% Data Flows
    Guest -->|Search Criteria| SearchProps
    SearchProps <-->|Retrieve Listings| PropertiesDB
    
    Guest -->|Select Dates| CheckAvail
    CheckAvail <-->|Check Dates| BookingsDB
    
    CheckAvail -->|Available| CreateBook
    CreateBook <-->|Store Booking| BookingsDB
    
    CreateBook -->|Update| UpdateStatus
    UpdateStatus -->|Notify| NotifyUsers
    
    NotifyUsers -->|Send to Guest| Guest
    NotifyUsers -->|Send to Host| Host
    
    NotifyUsers <-->|Get User Info| UsersDB
```

## Document Key

### Notes

1. All sensitive data flows are encrypted
2. Authentication required for all user operations
3. Data validation occurs at each process
4. Error handling implemented at all stages
