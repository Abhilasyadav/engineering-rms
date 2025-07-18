# Engineering Resource Management System (RMS)

## Project Overview

This is a full-stack web application designed to help organizations efficiently manage their engineering resources, projects, and assignments. It provides dashboards for insights, implements role-based access control, and tracks engineer capacity.

## Features

* **User Authentication & Authorization (JWT & RBAC):**
    * Secure user login/logout.
    * Roles: `ADMIN`, `PROJECT_MANAGER`, `ENGINEER`, `VIEWER`.
    * API endpoints protected based on user roles.
    * Frontend UI elements conditionally rendered based on roles.
* **Engineer Management:**
    * CRUD operations for engineer profiles (Name, Email, Department, Skills, Capacity).
    * View individual engineer details.
* **Project Management:**
    * CRUD operations for projects (Name, Description, Dates, Status, Project Manager).
    * View project details and assigned engineers.
* **Assignment Management:**
    * Assign engineers to projects with specified allocation percentages and dates.
    * Track active, completed, and removed assignments.
* **Capacity Tracking & Dashboards:**
    * Overall system statistics (e.g., total engineers, active projects).
    * Engineer capacity dashboard showing allocated vs. remaining hours.
    * Project status overview.
    * (Future/Enhancement: Skill gap analysis, project forecasting).

## Technology Stack

* **Frontend:**
    * **React:** JavaScript library for building user interfaces.
    * **Vite:** Fast build tool for modern web projects.
    * **JavaScript (JSX):** The programming language for the frontend.
    * **Tailwind CSS:** Utility-first CSS framework for rapid UI development.
    * **Recharts:** Composable charting library for React for data visualization.
* **Backend:**
    * **Spring Boot (Java 17+):** Framework for building robust RESTful APIs.
    * **Spring Data JPA & Hibernate:** ORM for database interaction.
    * **Spring Security:** Comprehensive security framework for authentication (JWT) and authorization (RBAC).
    * **JJWT:** Java library for JSON Web Token creation and validation.
* **Database:**
    * **MySQL:** Popular open-source relational database.
* **Containerization:**
    * **Docker & Docker Compose:** For consistent development environments and easy deployment.

## AI Tools Usage

This project extensively utilized AI tools to accelerate development, generate boilerplate, and assist with complex logic and debugging.

### GitHub Copilot

* **Entity/Model Generation:** Copilot assisted in generating boilerplate for `Engineer`, `Project`, `Assignment`, and `User` entities (fields, constructors, getters/setters, `@Entity` annotations).
    * *Example:* When defining `Engineer.java`, Copilot suggested common fields like `firstName`, `lastName`, `email`, and `department`.
* **Spring Data JPA Repositories:** Helped in creating standard `JpaRepository` interfaces and suggesting custom query methods (e.g., `findByUsername` in `UserRepository`).
* **Service Layer Boilerplate:** Provided skeletons for `getAll`, `findById`, `save`, `delete` methods in `EngineerService`, `ProjectService`, etc.
* **JWT Utility Methods:** Assisted in structuring the `JwtUtil` class methods for token generation and validation.
* **React Component Scaffolding:** Suggested initial functional component structures, `useState` and `useEffect` hooks for data fetching in pages like `Engineers.jsx` and `Projects.jsx`.
* **Tests (Initial drafts):** Offered basic unit test methods for services and controllers, providing a starting point for comprehensive testing.

### Gemini / ChatGPT

* **Database Schema Design:** Prompted for an initial MySQL schema for `engineers`, `projects`, `and assignments`, including table relationships and data types. This saved significant time in initial data modeling.
    * *Prompt Example:* "Design a MySQL database schema for an engineering resource management system, including tables for engineers, projects, and their assignments. Consider roles and user authentication."
* **Spring Security Configuration:** Assisted in understanding and configuring `Spring Security` for JWT authentication and role-based authorization, providing code snippets for `SecurityConfig`, `AuthTokenFilter`, and `AuthEntryPointJwt`.
    * *Prompt Example:* "How to implement JWT authentication and RBAC (Admin, PM, Engineer roles) in Spring Boot 3 with `@PreAuthorize`?"
* **Complex Logic Elaboration:** Asked for explanations on how to calculate engineer capacity considering multiple concurrent assignments and allocation percentages, which helped in designing the backend service logic.
* **Frontend Component Logic:** Provided guidance and initial code for complex React UI components, such as a flexible data table structure with sorting/filtering or a basic charting integration.
    * *Prompt Example:* "Write a basic React component for a dashboard using Tailwind CSS and Recharts to display project status distribution."
* **Debugging Assistance:** Provided solutions for common issues, such as the `net::ERR_SSL_PROTOCOL_ERROR` when connecting frontend to backend on localhost, suggesting common causes and fixes.
* **README Generation:** Utilized to draft the initial structure and key sections of this README, including features, tech stack, and setup instructions.

## Getting Started

### Prerequisites

* Java 17+ (JDK)
* Node.js (LTS version recommended) & npm
* Docker & Docker Compose (recommended for easy setup)
* A Git client

### Setup Instructions

1.  **Clone the repository:**
    ```bash
    git clone <your-github-repo-link>
    cd engineering-rms
    ```

2.  **Using Docker Compose (Recommended for Development):**
    This will build and run all services (MySQL, Spring Boot Backend, React Frontend).
    ```bash
    docker-compose up --build
    ```
    * The backend will be available at `http://localhost:8080`.
    * The frontend will be available at `http://localhost:5173`.
    * The MySQL database will be running on `localhost:3306`.

3.  **Manual Setup (Alternative):**

    #### Backend (Spring Boot)
    a.  **Navigate to the backend directory:**
        ```bash
        cd backend
        ```
    b.  **Set up MySQL:**
        * Ensure MySQL is running.
        * Create a database: `rms_db`
        * Create a user: `rms_user` with password: `password` (or adjust `application.properties`).
    c.  **Build and Run:**
        ```bash
        mvn clean install
        mvn spring-boot:run
        ```
        The backend will start on `http://localhost:8080`.

    #### Frontend (React)
    a.  **Navigate to the frontend directory:**
        ```bash
        cd ../frontend
        ```
    b.  **Install dependencies:**
        ```bash
        npm install
        ```
    c.  **Create `.env` file for API URL:**
        In the `frontend` directory, create a file named `.env` with the following content:
        ```env
        VITE_API_BASE_URL=http://localhost:8080/api
        ```
    d.  **Run the development server:**
        ```bash
        npm run dev
        ```
        The frontend will start on `http://localhost:5173` (or another port if 5173 is in use).

### Initial Users (for Testing)

You will need to manually register an ADMIN user or use a tool like Postman to hit your `/api/auth/signup` endpoint to create initial users.
* **Example Signup Request (POST `http://localhost:8080/api/auth/signup`)**:
    ```json
    {
        "username": "adminuser",
        "password": "password"
    }
    ```
    (You'll need to update `UserService` to assign `ROLE_ADMIN` for this initial user, or add an admin function in the future).
* **Example Login Request (POST `http://localhost:8080/api/auth/signin`)**:
    ```json
    {
        "username": "adminuser",
        "password": "password"
    }
    ```
    Use the returned JWT token in subsequent requests (`Authorization: Bearer <token>`).

## Working Demo

[Provide Link to Live Demo Here - e.g., Railway, Render, Netlify for frontend + backend service]
[Provide Link to a video demo (if applicable)]

## Future Enhancements

* Dedicated Engineer/Project detail pages with more information and charts.
* Advanced filtering, sorting, and pagination for all lists.
* Reporting features (e.g., engineer utilization reports).
* Notification system for assignment changes or project status updates.
* Integration with external systems (e.g., HR, Jira).
* Automated testing (unit, integration, end-to-end).
* More robust error handling and user feedback.
* Dark mode.
* Better UI/UX for assigning engineers (drag-and-drop, scheduling view).
