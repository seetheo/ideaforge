# IdeaForge - Complete Project Requirements

## Project Overview

IdeaForge is an AI-powered application planning and development tool that helps developers transform unstructured "brain dumps" into comprehensive, actionable Project Requirements Documents (PRDs) ready for AI agents to systematically build applications.

## Technology Stack

- **Frontend**: React + TypeScript + Tailwind CSS + shadcn/ui
- **Backend**: Rust (Tauri)
- **Database**: SQLite (Prisma ORM)
- **State Management**: Zustand (client), TanStack Query (server)
- **AI Providers**: Multi-provider support with:
  * zai GLM 4.7 (primary - general reasoning, code, vision)
  * deepseek-reasoner (advanced reasoning tasks)
  * OpenAI-compatible endpoints (customizable base URL)
  * Fallback provider system with automatic retry

## Core Requirements

### Phase 1: Foundation & Core Infrastructure

Set up Tauri project with React + TypeScript + Tailwind CSS + shadcn/ui:

- Initialize Tauri project structure with proper folder hierarchy
- Configure Prisma ORM with SQLite database
- Create complete database schema (Project, BrainDump, AudioFile, Feature, Task, Version, Tag, ProjectTag, BrainDumpTag, FeatureTag)
- Set up Zustand for client state management
- Create basic project dashboard UI
- Implement multi-provider AI system:
  * Provider configuration UI (select active provider, configure API keys, set custom endpoints)
  * Provider capabilities display (text, code, vision, reasoning)
  * Request routing (primary → fallback chain)
  * API key management (secure storage, environment variables: ZAI_API_KEY, DEEPSEEK_API_KEY, OPENAI_API_KEY)
  * Connection testing per provider
  * Rate limiting and retry logic
  * Request/response caching
  * Error handling with graceful degradation
- Set up file system operations for project storage
- Create basic routing structure with React Router
- Configure build system and development environment
- Set up ESLint, Prettier, TypeScript strict mode

### Phase 2: Brain Dump & AI Processing

Implement brain dump experience with AI-powered analysis:

- Brain dump editor (rich text + markdown support)
- Audio recording and transcription interface
- Structured plan UI (Features, Tasks, GUI/UX, Design tabs)
- Interactive mind map visualization (React Flow or D3)
- Two-way sync between plan and mind map
- GitHub-style version control (auto + manual commits)
- Basic chat interface (context-aware)
- AI clarification flow for brain dump
- Project creation flow with template selection
- Preliminary questions dialog

### Phase 3: Structured Plan & Mind Map

Implement structured plan and mind map with two-way sync:

- Structured Plan UI with tabs for Features, Tasks, GUI/UX, Design
- Features tab with Core MVP and Nice-to-Have sections
- Tasks tab with phase-based breakdown (1, 1.1, 1.2, 2, 2.1, etc.)
- GUI/UX tab with MVP and Production wireframes
- Design tab with system architecture, database schema, API design, tech stack
- Interactive mind map with full CRUD operations
- Two-way sync between structured plan and mind map
- Version control with history, diff viewer, rollback
- Chat interface with context-aware capabilities

### Phase 4: Advanced Features & Integration

Implement advanced AI-powered features and integrations:

- Wireframe generation (GLM 4.7 MCP server integration)
- Architecture diagram generation
- Tech stack comparison UI with pros/cons tables
- MCP Market integration (scan mcpmarket.com)
- MCP recommendation algorithm
- Docker configuration generator (Dockerfile, docker-compose.yml)
- Risk assessment system (AI identification + mitigation)
- Cost estimation system (hosting, APIs, infrastructure)
- Timeline/Gantt chart visualization
- Code snippet generation

### Phase 5: Polish & User Experience

Implement polish and enhancement features:

- In-project and cross-project search (advanced filters)
- Progress tracking dashboard (overall, by section)
- Export functionality (PRD, JSON, mind map, wireframes, Docker, etc.)
- Export to IDE structure generator
- Backup and restore system (zip export)
- Tags and labels system (custom colors, categories)
- Keyboard shortcuts (global, editor, navigation)
- Undo/redo system (per-section and global)
- Theme support (light/dark/system)
- Accessibility features (WCAG 2.1 AA, screen readers, keyboard nav)
- Offline mode support
- Onboarding tutorial (interactive walkthrough)
- Citations & references (AI transparency)
- Learning resources (tutorials, docs, courses)
- Code snippet generation

### Phase 6: Testing & Quality Assurance

Implement comprehensive testing and quality assurance:

- Unit tests for frontend (Jest, React Testing Library)
- Unit tests for backend (Rust tests)
- Integration tests (API, database, file system)
- E2E tests (Playwright for critical workflows)
- Performance tests and optimization
- Accessibility audit
- Security review
- Fix bugs found during testing
- Create test coverage reports

### Phase 7: Documentation & Release

Complete documentation and prepare for release:

- User documentation (getting started, features, FAQ, video tutorials)
- Developer documentation (architecture, API, contributing, database schema)
- Installation guides (Linux, macOS, Windows)
- Onboarding tutorial (in-app interactive walkthrough)
- README.md with quick start guide
- Release builds (Linux, macOS, Windows)
- Distribution channels set up
- Release notes published

## Additional Requirements

### Template Management

- All 34 templates defined:
  - General/Platform (10): Mobile App, Desktop App, Web Application, Web GUI, CLI Tool, Library/SDK, Game, Sports App, IoT Device, Embedded System
  - API/Services (10): REST API, Web API, GraphQL API, Microservice API, Linux Service, Windows Service, Web Service, Serverless Function, Auth Service, Notification Service
  - Data (1): Data Pipeline/ETL
  - Platform Extensions (2): Chrome/Edge Extension, VS Code/JetBrains Extension
  - Specialized (3): CMS/E-commerce, Real-time Collaboration, Smart Contract/Blockchain
  - Language-Specific (8): Python Module, Python Package, C++ Module, C++ Application, Rust Module, Rust Application, Go Module, Go Application

### Best Practice Warnings

- Deprecated technologies (red badge)
- Anti-patterns (orange badge)
- Security vulnerabilities (red badge with exclamation)
- Performance issues (yellow badge)
- Scalability concerns (yellow badge)
- Warning levels: Error, Warning, Info, Suggestion

### Quality Gates

- All tests must pass before merging
- Code must be linted (ESLint, rust-analyzer)
- Code must be formatted (Prettier, rustfmt)
- No security vulnerabilities
- Minimum test coverage met (>80%)
- Documentation updated for new features

### Documentation Requirements

- Clear and concise
- Screenshots and diagrams
- Code examples
- Cross-references
- Searchable
- Accessible (WCAG 2.1 AA)

## Implementation Notes

- This is a complete specification for building IdeaForge
- All features from the PRD are included
- UI design specifications are detailed for each feature
- Use G3 flock mode for parallel development with 7 agents
- Each agent works on a specific phase with defined working directories
- Agents execute sequentially based on dependencies
- Progress is tracked in .g3/flock/status.json
