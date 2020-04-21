# PubSub for E-Edu Microservices

THIS STILL IS WIP

## General
Here are the Channels that every MS should have. These display status inforamtions aboud the MS

### Service Start/Stop

#### Path 

{MS name}.general.status

#### Payload

msName:string \
timestamp:string \
action:string [contains ether "start" or "stop"]

## Report Microservice
These are the Channels where the ReportMS is the Publisher

### User Ban

#### Path
report.user.ban

#### Payload
userId:string (format:uuid) \
reason:string \
scope:string [contains string defined [here](https://github.com/E-Edu/concept/blob/master/changelog/meeting/20200420-conceptmeeting.md#5-Authentication) ] \
author:object [contains the authorId and the authorName]


## Task Microservice
These are the Channels where the TaskMS is the Publisher

### Publish Freestile

#### Path
task.task.solution.freestyle.user
#### Payload
taskId:int \
authorId:string (fromat:uuid) \
userId:string (format:uuid) \
solution:string

### Publish Image Solution
#### Path
task.task.solution.image.user
#### Payload
taskId:int \
authorId:string (fromat:uuid) \
userId:string (format:uuid) \
solution:string (fromat:url)

## User Microservice
These are the Channels where the UserMS is the Publisher