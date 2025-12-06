---
title: Software Development Life Cycle
draft: false
tags: [dev]
date: 2025-12-06
---
- What is a software
    
    A set of instructions that the system understands and makes it to perform some operations.
    
- **SDLC** stands for **S**oftware **D**evelopment **L**ife **C**ycle.
    
    Broadly 6 phases of development
    
    *// the ‘Who’ of each phase depend on the scope of entire project*
    
    - **Requirement Gathering and Analysis Phase**
        - **Goal**
            
            Document **functional** and **non-functiona**l requirements
            
            - Functional requirements specify What the software/product should do.
            - Non-functional requirements specify how the software/product should behave like
                - Response Time
                - Accessibility
                - Availability
                - Scalability
                - Maintainability
                - Extensibility …
        - **Who**
            - Business Analyst
            - Business user or Functional Users
            - Architect(s)
            - Lead Developer
            - Project Manager or Scrum Master
                - They coordinate the project / application
                - A project will have a definite start and definite end.
        - **Process**
            - Techniques
                - Understand the existing system or process
                - Discussion with the stake holders
                - Interview end users and stake holders
                - Questionnaire
                - Workshops
                - SMART // *certain indicators on how the requirements should be*
                    - Specific
                    - Measurable
                    - Agreed by all stakeholders
                    - Realistic
                    - Time bound
                - Screen / Visual Prototypes
        - **Deliverables**
            - Use Cases
            - Identified Actors/Users
            - Screen Prototyping
            - UX designs
            - Reviewed and Approved Requirements
    - **Design Phase**
        - **Goal**
            
            Develop a system architecture/design based on the reviewed and approved deliverables. i.e., functional and non-functional requirements.
            
        - **Who**
            - Lead developer(s)
            - Architect(s)
        - **Process**
            - Design discussion
            - Refer phase one deliverables
            - Design Patterns references - general reusable solutions to recurring problems
            - Architecture references
            
            *// experience and knowledge of persons involved plays a key role in this phase.*
            
            *// design should be within the scope of the functional and non-functional requirements*
            
        - **Deliverables**
            - Database designs
                - Conceptual
                - Logical
                - Physical
            - Integration / API / Endpoint designs
                - example of optional google login in site login page to integrate login.
            - Application design
                - language used, databases ..
            - Infrastructure design
                - configuration of server
            - Reviewed and Approved design
    - **Development of Coding Phase**
        - **Goal**
            
            Develop actual software based on the design deliverables
            
        - **Who**
            - Developers(s)
            - Lead Developer(s)
        - **Process**
            - review the design by developers
                - address doubts and clarify design before implementation
                - have an open mind
                - have a positive attitude and be open to ideas and recommendations
                - create a harmonious environment
            - Code based on design(s)
            - Unit test the code
        - **Deliverables**
            - completed software development
            - reviewed and approved software programs / coding.
    - **Testing or Quality Assurance Phase**
        - **Goal**
            
            Perform software testing based on functional and non-functional requirements.
            
        - **Who**
            - Tester(s)
            - Lead tester(s)
            - Load and performance testers
        - **Process**
            - inputs are functional and non-functional requirements and developed code
            - develop and execute **unit test** cases
                - manual use case execution
            - develop and execute integration test cases (if there is any)
            - develop and execute load and performance test cases
            - user acceptance testing
                - Business / functional users will be given beta access and test the product
        - **Deliverables**
            - Completed development and execution of test cases
            - Reviewed and approved test results
    - **Deployment Phase**
        - **Goal**
            
            Moving the developed software to production environment so that end users can access it.
            
        - **Who**
            - Software configuration manager (SCM) // will deploy
            - Lead Developer
            - Infrastructure team
            - Project Manager / Scrum Master
            - User
        - **Process**
            - Project manager / Scrum master coordinates the deployment plan with all stakeholders
            - set up deployment date
            - move the software to production
            - test the live application by end user
            
            *//* *the best practice is always to have a **backup** plan*
            
            *// also perform one type of testing in production*
            
        - **Deliverables**
            - software application available in production to end users
    - **Maintenance or Support Phase**
        - **Goal**
            
            fix software bugs or issues that are identified in live software during usage by end user. 
            
        - **Who**
            - Developer(s)
            - Lead developer(s)
            - Users
        - **Process**
            - recreate the issue and review
            - update and test the code / configuration / software program and resolve the issue
            - document the issue and fix
        - **Deliverables**
            - Updated production build
