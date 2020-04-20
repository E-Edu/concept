[Home](../../README.md) > Changelog

# Conept Meeting - 20. April 2020

## 1. Login

- Credentials
  - Students
    - Email & Password
  - Teacher
    - Email & Password
    - Teacher Token (Register)

## 2. Profile

- No profile pictures (yet)
- No nicknames (yet)
- Option to set real first- and lastname for certificates etc.

## 3. Referral link

- Bonus points for the person
  - We should save the relations between these persons
- Fake Account Protection
  - Community points legitimate a specific user

## 4. Role

- Admins can create tasks
- Everyone else can try these tasks

## 5. Authentication

- We wanted to refine the authentication concept - postponed
  - User MS
    - The user ms shows inactivity since 25days (state 20. April)
    - The service itself lacks a lot of features
  - The gateway currently is more capable of authenticating users than the user ms
  - We had some thoughts about implementing an email user micro service
  - JWT
    - userId
      - iss ("e-edu")
      - iat (64bit Unix Timestamp UTC as String)
      - exp (64bit Unix Timestamp UTC as String | 15min lifetime)
      - scopes (string[])
        - structure: microService.topic.mode.extra
        - "task.subject.create"
          - Gives the user/role permission to create subjects
        - "task.subject.read"
          - Gives the user/role permission to read subjects
        - "task.subject.read.beta"
          - Gives the user/role permission to read beta subjects
        - "task.task.write.verify"
          - Gives the user/role permission to verify tasks
  - JWTs should never contain sensitive information
    - status ("banned" f.e.)
  - Rough concept
    - Browser sends login/register request to backend (with deviceId)
    - backend responds with refreshToken (http only; secure) | frontend has to do nothing in case of 200
    - on every request there is the chance, that the CSRF token is not yet available or expired
      - server responds with "csrf expired"
      - browser starts a request to the jwt mutation
      - the backend responds with the csrf in the body which is valid for 15 minutes and will be set by the javascript (never store it in a cookie) in a header

> TODO: RefreshToken, Frontend- & Backend Hashing, DeviceTokens, Protections

## 6. PubSub

- Encoding
  - JSON (not pretty printed)
- Topic
  - microservice.topic.extra.extra2.extraN
- Example
  - user.user.ban
    - userId
    - reason (string)
    - scopes // see #5 -> JWT
    - author (user who banned the user)
      - (authorId)
      - (authorName) // only used for intern names
        - auto ban = "system"

## 7. Next meetings

- The concept team tries to have meetings every monday 8pm UTC in Discord
  - You need something to be refined? -> Create an issue in the concept repository or write into the #Konzept-doku channel in Discord

## 8. Roles & Permissions

- scopes (string[])
  - structure: microService.topic.mode.scope.extra
    - concetination is allowed
      - "task.subjects+module+lecture.read.all+beta"
        - should be stored seperately in the database
        - only use this format for data transfer
    - mode
      - read
      - write
      - delete
    - scope
      - none (default)
      - own
      - self (only applicable to topics regarding the user itself)
      - all
      - beta
    - extra
      - extra information (if needed)
  - "task.subject.create"
    - Gives the user/role permission to create subjects
  - "task.subject.read"
    - Gives the user/role permission to read subjects
  - "task.subject.read.beta"
    - Gives the user/role permission to read beta subjects
  - "task.task.write.verify"
    - Gives the user/role permission to verify tasks

- roles
  - student
    - user.user.write.self
    - user.user.delete.self
    - task.subject.read.all
    - task.module.read.all
    - task.lecture.read.all
    - task.lectureGroup.read.all
    - task.task.read
  - admin
    - user.user.read+write+delte.all+beta
    - task.subject.read+write+delte.all+beta
    - task.module.read+write+delte.all+beta
    - task.lecture.read+write+delte.all+beta
    - task.lectureGroup.read+write+delte.all+beta
    - task.task.read+write+delte.all+beta

## 9. DSGVO

- export data
  - json
  - pretty printing not needed
- document why, where and which data is collected/processed
- delete data

> TODO: Do we have to delete or is anonymization enough?

[Home](../../README.md)
