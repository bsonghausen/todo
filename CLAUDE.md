# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Azure Todo application - a full-stack React + C# sample demonstrating Azure Developer CLI (azd) deployment patterns. React frontend with Fluent UI, ASP.NET Core 8 Minimal API backend, Azure SQL Database.

## Build & Run Commands

### Frontend (src/web/)
```bash
npm run dev          # Start Vite dev server (port 5173)
npm run build        # Production build
npm run lint         # ESLint (strict, zero warnings allowed)
```

### Backend (src/api/)
```bash
dotnet build         # Build C# project
dotnet run           # Run API (port 3100)
dotnet watch run     # Hot-reload development
```

### Azure Deployment
```bash
azd up               # Provision + deploy to Azure
azd down             # Tear down all resources
azd monitor          # View Application Insights dashboards
```

### E2E Tests (tests/)
```bash
npm i && npx playwright install   # One-time setup
npx playwright test               # Run tests
npx playwright test --headed      # Run with visible browser
```

## Architecture

```
React Frontend (src/web/)     →   ASP.NET Core API (src/api/)   →   Azure SQL Database
- Fluent UI components            - Minimal APIs (.NET 8)            - TodoLists table
- Context + useReducer state      - Entity Framework Core            - TodoItems table
- Axios HTTP client               - Swagger at root endpoint
- Vite bundler                    - CORS enabled
```

### Frontend State Pattern
Uses React Context API with useReducer pattern (Redux-like). Key files:
- `src/web/src/components/todoContext.ts` - Application context
- `src/web/src/reducers/` - State reducers for lists, selectedList, selectedItem
- `src/web/src/actions/` - Action creators

### Backend Data Layer
- `TodoDb.cs` - EF Core DbContext
- `ListsRepository.cs` - Data access layer (repository pattern)
- `TodoEndpointsExtensions.cs` - All API route definitions

### API Endpoints
```
/lists                           GET, POST
/lists/{listId}                  GET, PUT, DELETE
/lists/{listId}/items            GET, POST
/lists/{listId}/items/{itemId}   GET, PUT, DELETE
/lists/{listId}/state/{state}    GET (filter items by state)
```

## Infrastructure

Bicep templates in `infra/` provision:
- App Service Plan + App Services (web/api)
- Azure SQL Database
- Key Vault (secrets)
- Application Insights + Log Analytics

## Environment Variables

Frontend requires `VITE_API_BASE_URL` pointing to API endpoint (auto-configured by azd).

## VS Code Tasks

Pre-configured tasks available:
- "Start API" / "Start Web" - Run individual services
- "Start API and Web" - Run both concurrently
- "Watch API" - Hot-reload C# development
