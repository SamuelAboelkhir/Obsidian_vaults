---
tags: 
- Other
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]

## ByteByteGo guides
![[Linkedin_Posts_2024_Blue.pdf]]
## Simplest login flow
```plantuml
title User Login Flow

actor User
participant Browser
participant "Auth Server" as Auth
participant Database

User->Browser: Enter credentials
Browser->Auth: POST /api/login
Auth->Database: SELECT * FROM users
Database-->Auth: User record
Auth->Auth: Verify password hash
Auth-->Browser: JWT token + refresh token
Browser-->User: Redirect to dashboard
```

## Expanded authentication flow
```plantuml
@startuml
title User Authentication Flow

actor User
participant Browser
participant "Auth API" as API
participant Database

User -> Browser : Enter email + password
Browser -> API : POST /auth/login
note over Browser,API : HTTPS encrypted

API -> Database : SELECT user BY email
Database --> API : User record

alt Valid credentials
  API -> API : Verify password hash
  API --> Browser : 200 OK + JWT token
  Browser -> User : Redirect to dashboard
else Invalid credentials
  API --> Browser : 401 Unauthorized
  Browser -> User : Show error message
end

loop Session keepalive (every 10 min)
  Browser -> API : GET /auth/refresh
  API --> Browser : New JWT token
end

opt Remember me
  Browser -> Browser : Store token in localStorage
end

@enduml
```
