# Project Overview

clcosu is a Serverpod-based system (version 3.0.1) designed to create an ecosystem for various construction industry participants.

## Key Stakeholders

The system caters to four primary roles:

*   **Client:** Individuals or entities requiring construction work.
*   **Contractor:** Executes construction work, managing manpower, tools, or subcontracting.
*   **Supplier:** Provides necessary materials to clients, contractors, or other parties.
*   **Labour:** Daily wage workers who can be assigned to contractors.

## Technology Stack

The clcosu system leverages a modern technology stack for robust and scalable development:

*   **Backend:** Serverpod (version 3.0.1) , with pgvector and embeddings
*   **Frontend:** Flutter (for the client application)
*   **State Management:** Signals (Signals-based state management)

## System Components

The architecture comprises three main Serverpod components:

*   `clcosu_server/`: The backend server logic and API.
*   `clcosu_client/`: The generated client-side code for interacting with the server.
*   `clcosu/`: The Flutter application, serving as the primary user interface.

## Agentic AI Role

An integrated Agentic AI will play a crucial role in the development and maintenance lifecycle, specifically handling:

*   Code generation
*   Architecture validation
*   Documentation
*   Consistency checks across all system layers
