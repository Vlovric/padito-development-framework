[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-yellow.svg)]([https://opensource.org/licenses/MIT](https://www.apache.org/licenses/LICENSE-2.0))
[![Framework: Design-First](https://img.shields.io/badge/Framework-Design--First-blue.svg)]()

# padito-development-framework
A structured, local, design-first development framework and template for solo engineers working with AI coding agents.

# 🗁 PADITO Framework (Local Agentic Development)
**P**laniranje (Planning) • **A**naliza (Analysis) • **D**izajn (Design) • **I**mplementacija (Implementation) • **T**estiranje (Testing) • **O**državanje (Maintenance)

PADITO is a highly disciplined, **design-first local development framework** optimized for solo engineers building with **AI coding agents** that ensures minimal token and context bloat.

By using this framework, you eliminate the biggest flaws of LLM coding agents: **context amnesia** and **context bloat**

## Mindset
> ⚠︎ **Core Rule:** Treat the AI agent as a **black-box junior developer**

Left unguided, an agent *will* take shortcuts,*will* write unclean code, *will* skip tests, etc... It is important to verify every agent delivery ***as if reading a junior developer PR***.
- **Deconstruct Complexity:** If a task is too complex, break it down into tiny, sequential tasks
- **Context Limits:** Agents have a finite context window. They will experience context poisoning and hallucinate if overloaded no matter how well your docs are structured.

## ⚓︎ Core Philosophy: Single Source of Truth (SSOT)
To remove friction in development, this framework enforces a strict **Single Source of Truth** layout with specific artifacts in each phase:

### **General Onboarding & Project Context** -> **Planning**
*Alignment and project scope*
- `1 Planning` - index for all artifacts
-  `1_1 Project brief` - everything the agent needs to know about the general context of the project when starting a new session

### **High-Level Business Logic & Functional Requirements** -> **Analysis**
*What the system must do from a business standpoint, independent of data types*
- `2 Analysis` - index for all artifacts
- `2_1 Data Dictionary` - defines business level recognized entities with their atributes, purpose of recognized atribute and validation rules **without** diving into data type level specifics
-  `2_2 External Dependencies` - describes each external service or library, in what scenario it's used and needed (high-level), also describes what data has to be **sent** and **received** (high-level) and what API keys are required for it
-  `features/FRXX - name` - single place for all functional requirements, details **happy paths**, **edge cases**, **entities** involved, **wireframes** and **high-level diagrams**
-  `diagrams/` - single place for all high-level diagrams
-  `wireframes/` - single place for all wireframes

### **Technical Layouts, Flow Controls & Types** -> **Design**
*Exact technical blueprints, translation of business rules into technical structures*
- `3 Design` - index for all artifacts
- `3_1 schema.sql` - physical SQL (or other) DB code as a SSOT for data types, tables and constraints
- `3_2 Arhitecture and folder structure` - defines the **arhitectural style** of the system and rules for **folder structure**
- `3_3 screen_flow.jpg` - visual screen flow diagram intended for the **engineer**
- `3_3_1 Screen flow table` - table describing screen flow intended for **agents**, combines ID-s of elements on screens with events and destinations
- `3_4 API specification` - High-Level API spec which defines the purpose of the endpoint, code returns and which data it needs to return **without** diving into code (***OpenAPI + Swagger is technical API documentation***)

### **Project Tracking & Framework Memory** -> **Implementation**
*Active state tracking and long-term memory protection for agent sessions*
- `4 Implementation` - index for all artifacts
- `4_1 Todo` - List of **tasks** (issues) and notes for **planned changes**
- `4_1 issues/X - Title` - A single place for all issues, used to track work like regular GitHub issues
- `4_2 Changelog` - Log of N recent changes (**active history**) and a compressed log of all important changes (**archived history**) for token savings, intended for **agent memory**
- `4_3 Insights` - Technical documentation of learned "gotchas" and past mistakes, **critical for agent memory**
- `4_4 Setup instructions` - Step by step instructions for setting up a local development environment
- `4_5 Code style and Conventions` - Preferences for code style for the **agent**

### **Quality Assurance & Validation** -> **Testing**
*Verification and future-proofing*
- `5 Testing` - index for all artifacts
- `FRXX` - template file for tracking all **unit**, **integration** and **E2E** tests and their **status** for each test case (*the test code itself is documentation*), per functional requirement

### **Post-Deployment & Handover** -> **Maintenance**
*System documentation*
- `6 Maintenance` - index for all artifacts
- `6_1 Usage Documentation` - user manual
- `6_2 Technical Documentation` - tehnical documentation of arhitectural decisions, class responsibilities, etc...
- `6_3 Readme` The working copy for the GitHub readme file

## 𓊍 Vertical Slice Development: AI Context
When instructing an agent to build a **Vertical Slice** (e.g. implementing a specific feature from database schema up to the UI components) **do not throw the whole repository context at it**. That wastes tokens and bloats context.

Instead, feed the agent this exact documentation package. It represents the minimum required context to execute a flawless vertical slice without any prior context present:

``` Context bundle
my-project/
├── 📄 2 analysis/features/FRXX - name.md   <- Business constraints (Happy/Edge paths)
├── 📄 3 design/3_3_1 Screen flow table.md  <- UI Interactions & element IDs
├── 📄 3 design/3_1 schema.sql.md           <- Technical DB types & layouts
├── 📄 3 design/3_4 API specification.md    <- High-level API contracts
├── 📄 4 implementation/4_2 Changelog.md    <- Context of the very last session
├── 📄 4 implementation/4_3 Insights.md     <- System traps and engineering gotchas
├── 📄 4 implementation/4_5 Code style...   <- Formatting & coding conventions
└── 📄 4 implementation/4_1 issues/X...     <- The target isolated task assignment
```

## ⚙︎ How to Start a Session (Prompt Blueprint)
*Example of what a initial prompt with the agent would look like*

```
Act as an expert software engineer. We are developing a vertical slice for our application. 

I have attached the following design and analysis files:
- [FRXX - Feature Specification]
- [3_3_1 Flow Table]
- [3_1 schema.sql]
- [3_4 API Specification]
- [4_2 Changelog] -> READ THIS FIRST to get previous session context
- [4_3 Insights] -> READ THIS SECOND to avoid past engineering gotchas.
- [4_5 Code Style and Conventions]

Your task is outlined in [Issue X - Title]. Review the data schemas and API endpoints carefully. Write the fully functional, defensive code required. Do not leave placeholder comments like "// TODO: implement later".
```
After implementation and code verification, let the agent write the tests and specify them in the `5 testing/FRXX` doc.

## ⟳ The Wrap-Up Protocol
Before you close the session, run the agent through the following housekeeping steps:
1. **Log the Work**: Document what was altered inside `4_2 Changelog` under the current date
2. **Capture Lessons**: If any unexpected bugs were resolved during the session or mistakes were made and solved, write down the lesson/conclusion/solution in `4_3 Insights` so future chat sessions don't make the same mistake

## ✍︎ Note
Since this template is written purely in markdown, a good markdown editor is ***heavily recommended***. I personally use [Obsidian](https://obsidian.md/).
