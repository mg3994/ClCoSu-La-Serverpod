This document specifies the architectural blueprint for a highly customizable, AI-integrated Flutter application. The design strictly follows Clean Architecture, utilizes MVVM with the Flutter Signals package for state management, and adheres to SOLID and DRY principles to ensure maximum scalability, testability, and maintainability.

## 1. 🏗️ Core Architectural Principles (Foundation)

The application is structured into three concentric layers as per Clean Architecture, ensuring dependencies flow inwards (Presentation -> Domain, Data -> Domain). MVVM with Flutter Signals provides a reactive and performant state management solution across these layers.

| Layer       | Responsibility                                                                | State Management      | Principle Focus                                            |
| :---------- | :---------------------------------------------------------------------------- | :-------------------- | :--------------------------------------------------------- |
| **Presentation** | User Interface (Views), UI Logic (ViewModels), and Navigation.                | Flutter Signals (MVVM) | Testability, Reactivity, User Experience.                  |
| **Domain**  | Core Business Rules (Entities, Use Cases, Repository Interfaces).             | Pure Dart Logic       | Independent of frameworks or data sources (SOLID 'I' & 'D'). |
| **Data**    | External Data Sources (API calls, Local Storage, Caching).                    | FlavorConfig for dynamic endpoints. | Data integrity, Repository Implementation.                 |

**State Management: MVVM with Flutter Signals**

We adopt the MVVM pattern where Views are decoupled from business logic using the `signals` package.

| Component   | Role                                          | Implementation                                                                                                    |
| :---------- | :-------------------------------------------- | :---------------------------------------------------------------------------------------------------------------- |
| **Views**   | Stateless Widgets.                            | Listen to Signals using `context.watch()` or `signal.value`.                                                      |
| **ViewModels** | Business/Presentation Logic.                  | Hold reactive state using `final state = signal<T>(initialValue)`. Calls Domain Use Cases.                         |
| **Signals** | Reactive State Primitives.                    | Provides granular, performant state updates, minimizing widget rebuilds (DRY).                                    |

## 2. 🤖 Feature: AI-Generated UI (GenUI) Integration

This feature enables the application to dynamically generate UI components based on user prompts by leveraging an external AI service. The AI Service (GenUI) acts as a data source, with its business logic managed by Use Cases, fully adhering to Clean Architecture principles.

**Architecture Flow for AI-Generated UI:**

1.  **Presentation (ViewModel):** Triggers a `GenerateUIDesignUseCase` in response to a user prompt.
2.  **Domain (Use Case):** Executes the core logic for AI interaction, coordinating calls to the `GenUIRepository` interface.
3.  **Data (Remote Source):** `GenUIRemoteDataSource` makes the network call to the AI endpoint (`FlavorConfig.baseUrl<whateverserverpod endpoint>`).
4.  **Response:** The AI returns a structured configuration (e.g., JSON schema for a Widget tree).
5.  **Presentation (View):** The View renders the dynamic UI based on an updated Signal holding the received AI configuration.

**Benefits:** This design ensures the core logic for handling AI responses resides in the Domain layer, while the Data layer manages the specifics of AI API communication. This promotes SOLID principles (Single Responsibility and Dependency Inversion) and maintains high testability.

## 3. 🎨 Feature: Comprehensive Customization System

All application customization features (Font, Color, Theme Mode, Text Scale) are managed centrally, providing a consistent and dynamic user experience.

**Customization Implementation:**

| Configuration Item | Implementation                                | Dependency                                       |
| :----------------- | :-------------------------------------------- | :----------------------------------------------- |
| **Theme Mode**     | `signal<ThemeMode>`                           | `MaterialApp`                                    |
| **Color Scheme**   | `signal<FlexScheme>`                          | `AppTheme` utility (`flex_color_scheme`)         |
| **Font Family**    | `signal<TextTheme Function(TextTheme)>`       | `AppTheme` utility (`Google Fonts`)              |
| **Text Scale**     | `signal<double>`                              | `MediaQuery` Wrapper                             |

**Theming Utility (AppTheme):**
The provided `AppTheme` utility (leveraging `flex_color_scheme`) ensures robust and dynamic theme application. It centralizes styling logic, promoting DRY code by preventing repeated theme configurations across the app and reacting to global customization Signals.

## 4. ⚙️ Global Architectural Components

### 4.1. Configuration Management

**Flavor / Environment Configuration:**
The `FlavorConfig` abstracts environment details. `FlavorConfig.fromString(flavorName)` is called during app startup. The Data layer accesses `FlavorConfig.baseUrl` to determine the correct API environment. `BuildMode.current` provides visibility into the running Flutter mode (debug, profile, release) for conditional logic or feature enablement.

### 4.2. Navigation

**`go_router`:**
`go_router` is the mandatory library for application navigation. It defines a single, centralized route map (Router File) and handles navigation, deep linking, and separation of routing logic from the ViewModels, adhering to Clean Architecture principles.