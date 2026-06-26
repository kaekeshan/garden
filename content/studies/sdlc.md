---
title: Software Development Life Cycle
draft: false
tags: [notes]
date: 2025-12-06
---

A software is a set of instructions that the system understands and makes it perform some operations. **SDLC** — **S**oftware **D**evelopment **L**ife **C**ycle — broadly describes the six phases a piece of software moves through, from idea to live system to ongoing fixes.

The *who* of each phase depends on the scope of the entire project.

## Overview

| # | Phase | Outcome |
|---|-------|---------|
| 1 | [Requirement Gathering & Analysis](#requirement-gathering--analysis) | Approved functional & non-functional requirements |
| 2 | [Design](#design) | Architecture & design documents, approved |
| 3 | [Development / Coding](#development--coding) | Working code, unit-tested |
| 4 | [Testing / Quality Assurance](#testing--quality-assurance) | Approved test results |
| 5 | [Deployment](#deployment) | Live production application |
| 6 | [Maintenance & Support](#maintenance--support) | Updated production build |

---

## Requirement Gathering & Analysis

**Goal**

Document **functional** and **non-functional** requirements.

- **Functional requirements** specify *what* the software/product should do.
- **Non-functional requirements** specify *how* it should behave:
  - Response time
  - Accessibility
  - Availability
  - Scalability
  - Maintainability
  - Extensibility
  - …

**Who**

- Business Analyst
- Business user / Functional users
- Architect(s)
- Lead Developer
- Project Manager or Scrum Master — they coordinate the project/application. *A project has a definite start and a definite end.*

**Process**

- Understand the existing system or process
- Discussion with stakeholders
- Interview end users and stakeholders
- Questionnaire
- Workshops

>[!info] SMART indicators for requirements
>
> - **S**pecific
> - **M**easurable
> - **A**greed by all stakeholders
> - **R**ealistic
> - **T**ime-bound

- Screen / Visual prototypes

**Deliverables**

- Use cases
- Identified actors / users
- Screen prototypes
- UX designs
- Reviewed and approved requirements

---

## Design

**Goal**

Develop a system architecture/design based on the reviewed and approved deliverables — i.e. the functional and non-functional requirements.

**Who**

- Lead Developer(s)
- Architect(s)

**Process**

- Design discussion
- Refer phase-one deliverables
- Design patterns — general reusable solutions to recurring problems
- Architecture references

>[!note] Two caveats for the design phase
>
> - Experience and knowledge of the persons involved plays a key role.
> - The design should stay within the scope of the functional and non-functional requirements.

**Deliverables**

- **Database design**
  - Conceptual
  - Logical
  - Physical
- **Integration / API / Endpoint design** — e.g. optional Google login on the site login page
- **Application design** — language used, databases, …
- **Infrastructure design** — server configuration
- Reviewed and approved design

---

## Development / Coding

**Goal**

Develop the actual software based on the design deliverables.

**Who**

- Developer(s)
- Lead Developer(s)

**Process**

- Review of the design by developers
  - Address doubts and clarify the design before implementation
  - Keep an open mind
  - Stay open to ideas and recommendations
  - Create a harmonious environment
- Code based on the design(s)
- Unit-test the code

**Deliverables**

- Completed software development
- Reviewed and approved software programs / code

---

## Testing / Quality Assurance

**Goal**

Perform software testing based on the functional and non-functional requirements.

**Who**

- Tester(s)
- Lead Tester(s)
- Load and performance testers

**Process**

- Inputs: functional and non-functional requirements, plus developed code
- Develop and execute **unit test** cases — manual use-case execution
- Develop and execute integration test cases (if any)
- Develop and execute load and performance test cases
- User acceptance testing — business / functional users get beta access and test the product

**Deliverables**

- Completed development and execution of test cases
- Reviewed and approved test results

---

## Deployment

**Goal**

Move the developed software to the production environment so that end users can access it.

**Who**

- Software Configuration Manager (SCM) — runs the deployment
- Lead Developer
- Infrastructure team
- Project Manager / Scrum Master
- User

**Process**

- PM / Scrum Master coordinates the deployment plan with all stakeholders
- Set the deployment date
- Move the software to production
- End user tests the live application

>[!warning] Two deployment best practices
>
> - Always have a **backup plan**.
> - Perform at least one round of testing in production.

**Deliverables**

- Software application available in production to end users

---

## Maintenance & Support

**Goal**

Fix software bugs or issues identified in the live software during end-user usage.

**Who**

- Developer(s)
- Lead Developer(s)
- Users

**Process**

- Recreate the issue and review
- Update and test the code / configuration / software program, then resolve
- Document the issue and the fix

**Deliverables**

- Updated production build