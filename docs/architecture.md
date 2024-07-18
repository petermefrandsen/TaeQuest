# Architecture

## Overview

TaeQuest is a system that allows Taekwondo practitioners improve their skills within Taekwondo and instructors to manage their students and Censors to review and approve promotion tests.
The system is composed of a Single-Page App (SPA) frontend, a RESTful API backend, and a database.

## Context diagram

The context diagram shows the system boundary and the actors that interact with the system.

```mermaid
C4Context
    title System Context diagram for TaeQuest
    Enterprise_Boundary(b0, "TaeQuest0") {
        Person_Ext(PractitionerExt, "Taekwondo Practitioner unregistered", "A person who practices Taekwondo")
        Person(Practitioner, "Taekwondo Practitioner", "A person who practices Taekwondo")
        Person(Instructor, "Taekwondo Instructor", "A person who teaches Taekwondo")
        Person(Censor, "Censor", "A person who reviews and approves promotion tests")
        Person(Admin, "Admin", "A person who manages the system")

        System(TaeQuest, "TaeQuest", "TaeQuest")

    }

    Rel(PractitionerExt, TaeQuest, "Interacts with")
    Rel(Practitioner, TaeQuest, "Interacts with")
    Rel(Instructor, TaeQuest, "Interacts with")
    Rel(Censor, TaeQuest, "Interacts with")
    Rel(Admin, TaeQuest, "Manages")
```

# Container diagram

The container diagram shows the containers that make up the system and the actors that interact with them.

```mermaid
C4Container
    title Container diagram for TaeQuest
    Person_Ext(PractitionerExt, "Taekwondo Practitioner unregistered", "A person who practices Taekwondo")
    Person(Practitioner, "Taekwondo Practitioner", "A person who practices Taekwondo")
    Person(Instructor, "Taekwondo Instructor", "A person who teaches Taekwondo")
    Person(Censor, "Censor", "A person who reviews and approves promotion tests")
    Person(Admin, "Admin", "A person who manages the system")
    
    System_Boundary(TaeQuest, "TaeQuest", "TaeQuest") {
        Container(Frontend, "Single-Page App", "Angular Frontend")
        Container(API, "RESTful API", "RESTful API")
        Container(Database, "Database", "Database")
    }

    
    Rel(PractitionerExt, Frontend, "Interacts with", "HTTPS")
    Rel(Practitioner, Frontend, "Interacts with", "HTTPS")
    Rel(Instructor, Frontend, "Interacts with", "HTTPS")
    Rel(Censor, Frontend, "Interacts with", "HTTPS")
    Rel(Admin, Frontend, "Manages", "HTTPS")

    BiRel(Frontend, API, "Makes requests to", "HTTPS")
    BiRel(API, Database, "Manages data with", "JDBC")
```

# Component diagram

The component diagram shows the components that make up the system and the interactions between them.

For starters we are focusing on the user journey of a common user who wants to learn more about Taekwondo. The user can view the techniques available in the system, view the questions available in the system, and practice answering questions.

```mermaid
C4Component
    title Component diagram for TaeQuest
    System_Boundary(TaeQuest, "TaeQuest", "TaeQuest") {
        Container_Boundary(Frontend, "Single-Page App", "Angular Frontend") {
            Component(FrontendTechniques, "Techniques", "View the techniques available in the system")
            Component(FrontendQuestions, "Questions", "View the questions available in the system")
            Component(FrontendPractice, "Practice", "Practice answering questions")
        }
        Container_Boundary(API, "RESTful API", "RESTful API") {
            Component(APITechniques, "Techniques", "Retrieves the techniques available in the system")
            Component(APIQuestions, "Questions", "Retrieves the questions available in the system")
            Component(APIPractice, "Practice", "Provides and validates questions")
        }
        
        Container_Boundary(Database, "Database", "Database") {
            SystemDb(PostgreSQL, "PostgreSQL", "Database")
        }
    }

    Rel(FrontendTechniques, APITechniques, "Requests techniques", "HTTPS")

    Rel(FrontendQuestions, APIQuestions, "Requests questions", "HTTPS")

    Rel(FrontendPractice, APIPractice, "Requests practice questions", "HTTPS")

    BiRel(APITechniques, PostgreSQL, "Manages data with")
    BiRel(APIQuestions, PostgreSQL, "Manages data with")
    BiRel(APIPractice, PostgreSQL, "Manages data with")
    UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```