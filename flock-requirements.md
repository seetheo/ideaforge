# flock.yaml - IdeaForge Multi-Agent Development Manifest
# Complete PRD Coverage - All Features Included
name: "ideaforge-flock"
description: "Complete parallel development of IdeaForge - AI-powered application planning tool with all features"

# Global settings
settings:
  max_agents: 6
  timeout_minutes: 180
  provider: "anthropic.default"
  max_concurrent_agents: 3

# Agent definitions
agents:
  # Phase 1: Foundation & Core Infrastructure
  - name: "foundation-agent"
    description: "Sets up Tauri project structure, database schema, project dashboard, and basic AI integration"
    working_dir: "src-tauri"
    priority: 1
    requirements: |
      Set up Tauri project with React + TypeScript + Tailwind CSS + shadcn/ui:
      
      ## Project Structure
      - Initialize Tauri project structure with proper folder hierarchy
      - Configure Prisma ORM with SQLite database
      - Create complete database schema (Project, BrainDump, AudioFile, Feature, Task, Version, Tag, ProjectTag, BrainDumpTag, FeatureTag)
      - Set up Zustand for client state management
      - Configure TanStack Query for server state
      - Set up file system operations for project storage (Projects/, Templates/, Backups/)
      - Create basic routing structure with React Router
      - Configure build system and development environment
      - Set up ESLint, Prettier, TypeScript strict mode
      
      ## Project Dashboard UI (Detailed Design)
      - Grid/List toggle view for project cards
      - Project cards showing: name, template type, last modified date, progress bar, status indicator, tags preview
      - Quick actions on cards: Open, Export, Delete, Archive
      - Statistics summary panel: Total projects, Active projects, Completed projects, Recent activity, Templates used breakdown
      - Search projects bar with real-time filtering
      - Filter by template dropdown (all 34 templates)
      - Filter by status (All, Active, Completed, Archived)
      - Sort options: Name (A-Z/Z-A), Date (Newest/Oldest), Progress (High/Low), Status
      - "New Project" button (prominent, top-right)
      - "Import PRD" button
      - "Create Template" button
      - "View Archived" link
      - Responsive design (mobile-friendly)
      - Loading states with skeleton cards
      - Empty state with illustration and "Create your first project" CTA
      
      ## Project Creation Flow (Detailed Design)
      - Multi-step wizard with progress indicator
      - Step 1: Basic Info (Project name required, Description optional, Tags optional with tag picker)
      - Step 2: Template Selection (Grid view with cards, each showing: name, description, typical features, recommended tech stack, preview icon)
      - Step 3: Preliminary Questions (template-specific form with checkboxes, dropdowns, text inputs)
      - Step 4: Docker Option (Yes/No toggle, if Yes shows docker configuration options)
      - "Create Project" button with loading state
      - "Cancel" and "Back" navigation
      - Form validation with error messages
      - Auto-save draft (if user closes mid-creation)
      
      ## Basic AI Integration
      - Implement z.ai API integration (primary AI provider)
      - Set up API key management in settings
      - Create base AI service layer for making requests
      - Implement request/response caching
      - Set up rate limiting and retry logic
      - Create error handling for API failures
      - Add fallback provider configuration
      
      ## File System Structure
      - Create Projects/ directory structure
      - Create Templates/ directory
      - Create Backups/ directory
      - Set up project folder template (Documents/, Brain Dump/, Versions/, Code/, Exports/, Backups/)
      - Implement file watching for auto-save
      - Create JSON schema for project data files

  # Phase 2: Brain Dump & AI Processing
  - name: "brain-dump-agent"
    description: "Implements brain dump editor, voice input, audio recording, and AI analysis"
    working_dir: "src"
    priority: 2
    depends_on: ["foundation-agent"]
    requirements: |
      Implement brain dump experience with AI-powered analysis:
      
      ## Brain Dump Editor UI (Detailed Design)
      - Large rich text editor with markdown support (react-markdown, react-markdown-editor)
      - Toolbar: Bold, Italic, Heading, Link, Code Block, List, Quote, Image
      - Voice input button (microphone icon, prominent, left side of toolbar)
      - Upload button (for images/sketches, cloud upload icon)
      - Auto-save indicator (shows "Saving..." / "Saved" in footer)
      - Character/word count display
      - Tags/labels editor panel (right side, collapsible)
      - Search within brain dump (Ctrl+F, highlights matches)
      - Undo/Redo buttons (Ctrl+Z/Ctrl+Y)
      - Full-screen mode toggle
      - Export brain dump as text/JSON
      - Audio playback with text highlighting (when audio file selected)
      - Transcription editing (editable transcript view)
      - AI suggestions panel (right side, shows clarifying questions)
      - Context-aware header (shows current section: Brain Dump)
      
      ## Voice Input & Audio Recording (Detailed Design)
      - Recording button with visual feedback (pulsing animation when recording)
      - Recording duration timer (MM:SS format)
      - Stop recording button
      - Playback controls for recorded audio
      - Auto-transcription using ASR (speech-to-text)
      - Audio recordings list (shows: filename, duration, transcript preview, delete button)
      - Transcription editing (editable text area with original audio reference)
      - Audio file storage in Brain Dump/audio-recordings/
      - Transcription storage in database (AudioFile model)
      - Support for multiple audio files per brain dump
      - Upload existing audio files (drag & drop, file picker)
      
      ## Image Upload & Management (Detailed Design)
      - Image upload button (opens file picker)
      - Drag & drop zone for images
      - Image preview grid (thumbnails with filename)
      - Image description editor (text area below each image)
      - Image storage in Brain Dump/images/
      - Support for PNG, JPG, SVG formats
      - Image annotations (optional future feature)
      - Delete images button
      
      ## AI Clarification Flow (Detailed Design)
      - AI analyzes brain dump content in real-time
      - Proactive clarifying questions appear in suggestions panel
      - Questions are context-aware based on template and preliminary answers
      - Max 5 questions at a time to avoid overwhelming
      - User can answer questions inline
      - Answers auto-update brain dump content
      - "Ask more questions" button
      - "I'm done" button to move to next phase
      - AI detects key concepts and suggests tags
      - AI identifies missing information gaps
      - Progress bar showing "Analysis: 0% → 100%"
      
      ## AI Processing Pipeline (Detailed Design)
      - Trigger: Automatic after sufficient content OR manual "Analyze" button
      - Processing steps shown to user:
        1. "Analyzing brain dump..." (progress bar)
        2. "Extracting features..." (spinner)
        3. "Breaking down tasks into phases..." (spinner)
        4. "Generating wireframes..." (spinner with image preview)
        5. "Creating architecture design..." (spinner)
        6. "Recommending tech stack..." (spinner)
        7. "Searching MCP Market..." (spinner)
        8. "Assessing risks..." (spinner)
        9. "Done!" (checkmark animation)
      - Each step updates progress bar
      - Cancel button available during processing
      - Error handling with retry option
      - Cache AI responses to reduce API calls
      - Invalidate cache on content changes
      
      ## Brain Dump Data Structure
      - Content stored as markdown text
      - Audio files linked via AudioFile model
      - Images stored in file system with references
      - Tags linked via BrainDumpTag model
      - Timestamps for all changes
      - Auto-save every 5 minutes (configurable)

  # Phase 3: Structured Plan & Mind Map
  - name: "structured-plan-agent"
    description: "Implements structured plan UI, mind map visualization, two-way sync, and version control"
    working_dir: "src"
    priority: 3
    depends_on: ["brain-dump-agent"]
    requirements: |
      Implement structured plan and mind map with two-way sync:
      
      ## Structured Plan UI (Detailed Design)
      - Tabbed interface: Features, Tasks, GUI/UX, Design
      - Each tab has sub-sections with expandable/collapsible items
      
      ### Features Tab (Detailed Design)
      - Toggle switch: Core MVP / Nice-to-Have
      - Feature list with cards showing: name, description (truncated with "Read more"), priority badge (CRITICAL=red, HIGH=orange, MEDIUM=yellow, LOW=green), status badge (PLANNED/IN_PROGRESS/DONE/BLOCKED), tech stack tag, estimate tag, risk level badge
      - Edit feature button (pencil icon)
      - Delete feature button (trash icon)
      - Add feature button (plus icon, top-right)
      - Reorder features (drag handles on each card)
      - Change priority dropdown
      - View tasks per feature (expandable section)
      - Filter by priority, status, tech stack
      - Sort by priority, name, status
      
      ### Tasks Tab (Detailed Design)
      - Phase-based tree view: Phase 1 (expandable) → 1.1, 1.2, 1.3 → Phase 2 → 2.1, 2.2
      - Each task shows: phase identifier, name, description (truncated), tech stack tag, estimate, status, dependencies count
      - Expand task to see full details:
        - Description (step-by-step instructions)
        - Tech stack specific to task
        - Estimated AI coding time
        - Dependencies (list of task IDs with links)
        - Test checklist (checkboxes, strikethrough when checked)
        - Debug checklist (checkboxes, strikethrough when checked)
        - Code snippet (syntax highlighted, copy button)
        - Completion criteria (bullet points)
      - Add task button (plus icon)
      - Edit task button
      - Delete task button
      - Mark as complete button (checkmark icon)
      - Reorder tasks (drag handles)
      - View dependencies (visual graph or list)
      - Filter by phase, status, tech stack
      - Progress indicator per phase (X/Y tasks complete)
      
      ### GUI/UX Tab (Detailed Design)
      - Toggle: MVP / Production
      - Wireframe gallery (grid of images with descriptions)
      - Each wireframe shows: screen name, image thumbnail, description, components list, order number
      - Click to view full-size image
      - Edit description button
      - Reorder screens (drag handles)
      - User flows section (list of flows with steps)
      - Component library list (all components used)
      - Export wireframes button (downloads all images + descriptions)
      
      ### Design Tab (Detailed Design)
      - System Architecture section:
        - High-level description (markdown)
        - Architecture diagram (image with expand/collapse)
        - Services list (cards with name, technology, responsibilities)
      - Database Schema section:
        - Schema description
        - Database diagram (image)
        - Tables list (with descriptions)
        - Relationships visualization
      - API Design section:
        - API description
        - Endpoint table (Method, Path, Description, Auth)
        - Request/response contracts (expandable)
      - Tech Stack section:
        - Core technologies table (Category, Recommended, Alternatives, Justification)
        - Alternatives comparison (pros/cons tables)
        - Polyglot components table (Component, Recommended Language, Alternative, Justification)
        - Anti-patterns list (Technology, Reason, Alternative)
      
      ## Mind Map UI (Detailed Design)
      - Interactive mind map using React Flow or D3
      - Full CRUD operations at all levels
      - Click to expand/collapse nodes
      - Drag nodes to reorganize (smooth animation)
      - Add node button (plus icon on selected node)
      - Edit node button (double-click or pencil icon)
      - Delete node button (trash icon on selected node)
      - Color coding by category (Features=blue, Tasks=green, Tech Stack=orange, MCP=purple, Risks=red)
      - Two-way sync with Structured Plan:
        - Changes in Structured Plan → Update Mind Map (auto-sync indicator)
        - Changes in Mind Map → Update Structured Plan (auto-sync indicator)
        - Conflict dialog if simultaneous edits (show diff, choose which to keep)
        - Sync status indicator (green checkmark = synced, yellow = pending, red = conflict)
      - Zoom in/out buttons (+/- icons)
      - Pan around (drag on empty space)
      - Filter by type/status (dropdown)
      - Search nodes (search bar with highlighting)
      - Collapse/expand all buttons
      - Export as image button (PNG download)
      - Export as JSON button
      - Last-write-wins with confirmation for conflicts
      - Soft delete (30-day recovery in Archive)
      
      ## Version Control UI (Detailed Design)
      - Versions tab in project view
      - Version history list (table with: version number, commit message, timestamp, size, actions)
      - Actions per version: View, Compare, Rollback, Delete, Export
      - "Create Version" button (prominent, top-right)
      - Commit message field (required, multi-line text area)
      - Optional tag/release name field
      - Diff viewer (before/after comparison with side-by-side view)
      - Rollback button with confirmation dialog
      - Branch version (save as new project) option
      - Version comparison (select 2 versions, show diff)
      - Export version snapshot button
      - Delete old versions (bulk delete with confirmation)
      - Archive versions (move to archive folder)
      - Storage in /Projects/{Project}/Versions/ directory
      - JSON format for state
      - Separate folders for each version (v{major}.{minor}_{YYYY-MM-DD}_{HHMMSS})
      - Auto-save on major changes (background, configurable interval default 5 min)
      - Shows "Saving..." / "Saved" indicator
      - Versioned content: Brain dump, Structured plan, Mind map, PRD, Wireframes, Generated images, Docker config, MCP selections, Tags/labels
      
      ## Chat Interface UI (Detailed Design)
      - Left panel, collapsible (width adjustable)
      - Always accessible from any section
      - Context-aware header (shows current section: Brain Dump, Features, Tasks, etc.)
      - Conversation history (scrollable message list)
      - User messages (right-aligned, blue background)
      - AI messages (left-aligned, gray background)
      - Quick action buttons (above input):
        - "Refine this section"
        - "Add task here"
        - "Generate code snippet"
        - "Compare tech stacks"
        - "Identify risks"
        - "Estimate timeline"
        - "Suggest MCPs"
        - "Create Docker setup"
      - Message input (multi-line text area with send button)
      - Send button (paper plane icon)
      - Typing indicator ("AI is thinking...")
      - Loading spinner for AI responses
      - AI clarification requests (highlighted in yellow)
      - Tech stack challenge dialog (modal)
      - MCP recommendations (cards with install buttons)
      - Code snippet suggestions (syntax highlighted, copy button)
      - Conversation history:
        - Persistent across sessions (stored in database)
        - Searchable (search bar in chat panel)
        - Exportable (download as JSON)
        - Deletable (clear history button with confirmation)
        - Organized by topic/section (auto-grouped)
        - Timestamps on all messages
      - AI Models:
        - Primary: z.ai API (DeepSeek/GLM)
        - Secondary: GLM 4.7 MCP Server (image generation)
        - Fallback: Backup provider (configurable in settings)
      
      ## Two-Way Sync Logic
      - Last-write-wins with confirmation for conflicts
      - Soft delete (30-day recovery in Archive)
      - Visual sync indicators (green=synced, yellow=pending, red=conflict)
      - Real-time updates (debounced, 500ms delay)
      - Conflict resolution dialog (show diff, choose which to keep, merge option)

  # Phase 4: Advanced Features & Integration
  - name: "advanced-features-agent"
    description: "Implements wireframes, MCP integration, Docker config, template management, best practice warnings"
    working_dir: "src-tauri/src"
    priority: 4
    depends_on: ["structured-plan-agent"]
    requirements: |
      Implement advanced AI-powered features and integrations:
      
      ## Wireframe Generation (Detailed Design)
      - Integrate GLM 4.7 MCP server for image generation
      - Wireframe generation prompt template:
        - Screen name, type (Login, Dashboard, etc.), platform, style (MVP/Production)
        - Generate: screen description, component list, layout description, user flow, visual wireframe (base64 image)
      - Wireframes tab in project view:
        - MVP section (key screens only)
        - Production section (complete wireframes)
        - Each wireframe: image thumbnail, description, components list, user flow, order number
        - Click to view full-size image
        - Edit description button
        - Reorder screens (drag handles)
        - Delete wireframe button
        - Export wireframes button (downloads all images + descriptions as markdown)
      - Storage in /Projects/{Project}/Documents/wireframes/mvp/ and /production/
      - Image format: PNG
      - Descriptive markdown files: screen-XX-name-desc.md
      - Loading state during generation (spinner + "Generating wireframe...")
      - Error handling with retry option
      
      ## Architecture Diagram Generation (Detailed Design)
      - Generate system architecture diagrams using GLM 4.7
      - Diagram types: System Architecture, Data Flow, Database Schema, API Flow
      - Each diagram: image thumbnail, description, labels
      - Storage in /Projects/{Project}/Documents/architecture-diagrams/
      - View full-size button
      - Export diagram button
      - Edit description button
      
      ## Tech Stack Comparison UI (Detailed Design)
      - Tech Stack tab in project view
      - Core technologies section:
        - Comparison table (Category, Recommended, Alternatives, Justification, Complexity)
        - Alternatives expandable (show pros/cons tables)
        - Performance metrics (when available)
        - Learning curve rating
        - Community support score
      - Polyglot components section:
        - Table showing components that benefit from different languages
        - For each: Component, Recommended Language, Alternative, Justification
      - Anti-patterns section:
        - List of technologies to avoid
        - For each: Technology, Reason, Alternative
        - Warning badges (red)
      - Complexity warnings (yellow badges)
      - "Challenge this recommendation" button (opens chat with context)
      - "Request alternative" button
      - Export tech stack report button
      
      ## MCP Market Integration (Detailed Design)
      - MCP Services tab in project view
      - Data source: Scan mcpmarket.com for relevant MCP servers (18,872+ servers)
      - Categories: Developer Tools, API Development, Data Science & ML, Database Management, Cloud Infrastructure, Deployment & DevOps, Analytics & Monitoring, Web Scraping & Data Collection, Security & Testing, Learning & Documentation, Collaboration Tools, Content Management, Design Tools, Browser Automation, Social Media Management, Game Development, Marketing Automation, E-commerce Solutions, Mobile Development, Official, Featured
      - AI Processing:
        - Analyzes project requirements
        - Searches MCP Market for relevant services
        - Categorizes by project type and features
        - Ranks by: relevance (50%), popularity (30%), ease of integration (20%)
      - MCP list view:
        - Cards showing: name, description, category, usage/popularity score, integration complexity badge (Low/Medium/High), status (planned/implemented)
        - Filter by category dropdown
        - Sort by relevance, popularity, complexity
        - Search across MCPs (search bar)
        - MCP details modal (click card):
          - Name and description
          - Category
          - Why it helps this project (AI-generated explanation)
          - Usage/popularity score
          - Integration complexity
          - Installation instructions (code block with copy button)
          - Usage example (code block)
          - Dependencies list
          - Known issues/limitations
          - "Add to project" button
          - "Mark as implemented" button
          - "Remove from project" button
      - MCP recommendation algorithm:
        - Calculate relevance score based on project features
        - Normalize popularity score
        - Calculate integration ease score
        - Total score = relevance*0.5 + popularity*0.3 + ease*0.2
        - Sort by total score descending
      - Option 1: Official API (preferred) - Check if MCP Market provides API
      - Option 2: Web Scraping (fallback) - Scrape HTML, parse listings, respect robots.txt and rate limits
      - Cache results locally
      - Periodic refresh (configurable, default daily)
      
      ## Docker Configuration UI (Detailed Design)
      - Docker Config tab in project view (only shown if Docker enabled)
      - Dockerfile editor (code editor with syntax highlighting):
        - Multi-stage builds
        - Layer caching optimization
        - Copy button
        - Download button
        - "Generate optimized Dockerfile" button (AI-generated)
      - Docker Compose editor (YAML editor with syntax highlighting):
        - All services configured
        - Volume mappings
        - Network configuration
        - Environment variables
        - Copy button
        - Download button
        - "Generate docker-compose.yml" button (AI-generated)
      - Environment variables section:
        - List of required env vars
        - Description for each
        - Default values
        - Example values
      - Build & Run instructions:
        - Code blocks for: Build, Run, View logs, Stop, Clean
        - Copy buttons
      - Preview generated configs button
      - Test locally button (runs docker-compose up in background)
      - Export configurations button (downloads Dockerfile + docker-compose.yml + .env.example)
      - Customization support (edit files directly)
      - Version control for configs (stored in git)
      - Development vs production configurations (tabs)
      - Health checks configuration
      - Logging configuration
      
      ## Risk Assessment UI (Detailed Design)
      - Risks tab in project view
      - AI Risk Identification:
        - Technical risks (complexity, compatibility, dependencies)
        - Timeline risks (unrealistic estimates, dependencies)
        - Resource risks (learning curve, skill gaps)
        - Integration risks (MCPs, third-party services)
        - Scale risks (performance, architecture)
      - Risk Levels: Critical (red), High (orange), Medium (yellow), Low (green)
      - Risk list view:
        - Cards showing: title, description (truncated), impact badge, probability badge, level badge, status (Open/Mitigated/Closed)
        - Expand card to see:
          - Full description
          - Impact level
          - Probability
          - Mitigation strategies (bullet points with checkboxes)
          - Owner (future collaboration)
        - Filter by level, status, type
        - Sort by impact, probability, level
        - Risk dashboard (summary charts)
        - Risk timeline view (Gantt-style)
        - Mitigation task creation button
        - Risk notifications (toast alerts)
        - Export risk report button (PDF + Markdown)
      
      ## Cost Estimation UI (Detailed Design)
      - Costs tab in project view
      - Cost Categories:
        - Hosting costs (servers, databases, storage)
        - API costs (AI, MCP services, third-party APIs)
        - Infrastructure costs (CDN, monitoring, logging)
        - Development costs (AI coding time converted to equivalent)
        - Maintenance costs
      - AI Estimates:
        - Based on project requirements
        - Based on selected tech stack
        - Based on scale expectations
        - Based on usage patterns
      - Cost breakdown table:
        - Category (expandable)
        - Monthly cost
        - Yearly cost
        - Breakdown (itemized list)
      - Cost optimization suggestions (AI-generated)
      - Comparison of options (if multiple tech stack choices)
      - Export cost report button (CSV + PDF)
      - Currency selection dropdown (USD, EUR, GBP, etc.)
      
      ## Timeline / Gantt Chart UI (Detailed Design)
      - Timeline tab in project view
      - Visual Gantt chart with:
        - Phases as horizontal bars
        - Task dependencies shown (arrows between bars)
        - Milestone markers (diamonds)
        - AI coding timeline (fast, highlighted)
        - Human equivalent timeline (reference, gray)
        - Critical path highlighted (red)
      - Features:
        - Drag to adjust task durations (snap to grid)
        - Zoom in/out buttons (day/week/month views)
        - Filter by phase, feature, assignee
        - Show/hide dependencies toggle
        - Milestone management (add/edit/delete)
        - Baseline comparison (show planned vs actual)
        - Export Gantt chart button (PNG + CSV)
        - Print timeline button
      - Timeline Views: Day, Week, Month, Quarter
      
      ## Template Management UI (Detailed Design)
      - Templates library in project dashboard
      - Template cards showing: name, description, category, typical features, recommended tech stack, preview icon
      - Create Template from Project:
        - "Create Template from Project" button in project menu
        - Save project structure as template
        - Include sections, questions, defaults
        - Custom preliminary questions
        - Custom tech stack suggestions
        - Custom feature patterns
      - Template Management:
        - Edit templates button
        - Delete templates button (with confirmation)
        - Share templates (future: export/import)
        - Import templates (future: from file)
      - Template Features:
        - Name and description
        - Category (General/Platform, API/Services, Data, Platform Extensions, Specialized, Language-Specific)
        - Default questions (form definition)
        - Default tech stack (JSON)
        - Default features (JSON)
        - Custom PRD sections (markdown)
      - All 34 templates defined in PRD:
        - General/Platform (10): Mobile App, Desktop App, Web Application, Web GUI, CLI Tool, Library/SDK, Game, Sports App, IoT Device, Embedded System
        - API/Services (10): REST API, Web API, GraphQL API, Microservice API, Linux Service, Windows Service, Web Service, Serverless Function, Auth Service, Notification Service
        - Data (1): Data Pipeline/ETL
        - Platform Extensions (2): Chrome/Edge Extension, VS Code/JetBrains Extension
        - Specialized (3): CMS/E-commerce, Real-time Collaboration, Smart Contract/Blockchain
        - Language-Specific (8): Python Module, Python Package, C++ Module, C++ Application, Rust Module, Rust Application, Go Module, Go Application
      
      ## Best Practice Warnings UI (Detailed Design)
      - Warning system integrated throughout the app
      - What Gets Flagged:
        - Deprecated technologies (red badge)
        - Anti-patterns (orange badge)
        - Security vulnerabilities (red badge with exclamation)
        - Performance issues (yellow badge)
        - Scalability concerns (yellow badge)
      - Warning Levels:
        - Error (must fix) - red, blocks action
        - Warning (should fix) - orange, allows action with confirmation
        - Info (consider) - blue, informational only
        - Suggestion (improvement) - green, optional
      - Warning Display:
        - Inline warnings (next to affected code/setting)
        - Warning panel (sidebar showing all warnings)
        - Fix suggestions (clickable, opens fix dialog)
        - Dismiss warnings button (with "Don't show again" option)
        - Filter by level (dropdown)
      - Warning Examples:
        - "Express.js is outdated - Consider Fastify or Hono instead"
        - "MongoDB without schema validation - Data integrity risk - Use PostgreSQL or add schema validation"
        - "Hardcoded API key detected - Security risk - Move to environment variables"
        - "Missing input validation - Security risk - Add validation middleware"
      - Warning persistence (saved in project state)
      - Warning export (download as JSON)

  # Phase 5: Polish & User Experience
  - name: "polish-agent"
    description: "Implements search, progress tracking, export, backup, tags, keyboard shortcuts, undo/redo, theme, accessibility, offline mode, onboarding"
    working_dir: "src"
    priority: 5
    depends_on: ["advanced-features-agent"]
    requirements: |
      Implement polish and enhancement features:
      
      ## Search & Navigation UI (Detailed Design)
      - In-Project Search:
        - Search bar (Ctrl+K shortcut) with real-time filtering
        - Search across all content: brain dump, features, tasks, docs, mind map nodes
        - Filters by type (checkboxes): Tasks, Features, Docs, Wireframes, etc.
        - Filters by status (dropdown): All, Planned, In Progress, Done, Blocked
        - Filters by priority (dropdown): All, Critical, High, Medium, Low
        - Filters by tags (tag picker)
        - Highlighted results (yellow background on matches)
        - Quick navigation to matches (click to jump)
        - Recent searches (dropdown in search bar)
        - Search history (persistent across sessions)
      - Cross-Project Search:
        - Search across all projects (from dashboard)
        - Filter by project (checkbox list)
        - Filter by date range (date picker)
        - Export search results button
        - Open in new tab option
      - Advanced Search:
        - Boolean operators support (AND, OR, NOT)
        - Wildcards (*, ?)
        - Phrase matching ("exact phrase")
        - Date range filters
        - Tag filters
        - Status filters
      - Navigation:
        - Quick jump to sections (Ctrl+K command palette)
        - Recently viewed items (sidebar)
        - Bookmarks (star items for quick access)
        - Back/Forward history (browser-style navigation)
        - Breadcrumbs (show current location path)
      
      ## Progress Tracking UI (Detailed Design)
      - Progress tab in project view
      - Visual Progress:
        - Overall progress bar (percentage + "X/Y items complete")
        - Feature progress breakdown (bar chart by status)
        - Phase completion status (list of phases with progress bars)
        - Task completion by phase (tree view with checkmarks)
        - Progress by team member (future collaboration feature)
      - Progress Indicators:
        - Percentage complete (large number)
        - Items completed / total
        - Time elapsed vs estimated (chart)
        - Blocked items count (red badge)
        - At-risk items (yellow badge)
      - Progress by Section:
        - Brain Dump: % complete (based on AI analysis)
        - Features: % defined / % completed
        - Tasks: % completed by phase
        - GUI: % wireframes generated
        - Design: % architecture defined
        - Testing: % tests planned
      - Progress Reports:
        - Daily summary (chart showing progress over time)
        - Weekly summary (velocity trends)
        - Blockage alerts (toast notifications)
        - Risk notifications (toast notifications)
        - Export progress report button (PDF + Markdown)
        - Velocity trends (burn-up/burn-down charts)
      
      ## Tags & Labels UI (Detailed Design)
      - Tags system:
        - Custom tags/labels for any item
        - Tag colors (color picker with preset colors)
        - Tag categories (dropdown: tech, priority, status, custom)
        - Bulk tag operations (select multiple items, apply tag)
        - Filter by tags (tag cloud with counts)
        - Tag management panel (create, edit, delete tags)
        - Popular tags list (top 10 most used)
        - Tag search (find tags by name)
      - Predefined Tags (by Template):
        - Technology tags (React, Python, Rust, etc.)
        - Priority tags (Critical, High, Medium, Low)
        - Status tags (Planned, In Progress, Done, Blocked)
        - Type tags (Frontend, Backend, Database, DevOps)
      - AI Suggestions:
        - Suggest tags based on content analysis
        - Auto-tag common items (configurable)
        - Detect tag conflicts (same tag with different colors)
      - Tag UI:
        - Tag badges (colored pills)
        - Click tag to filter
        - Right-click to edit/delete
        - Drag to reorder
        - Merge tags (select multiple, merge into one)
      
      ## Export & Import UI (Detailed Design)
      - Export Menu (top menu bar):
        - Export PRD (Markdown) - Downloads complete PRD.md
        - Export JSON - Exports project data as JSON
        - Export Mind Map - Downloads mind map as PNG + JSON
        - Export Wireframes - Downloads all wireframes as images
        - Export Architecture Diagrams - Downloads all diagrams as PNG
        - Export Docker Configuration - Downloads Dockerfile + docker-compose.yml
        - Export Timeline - Downloads Gantt chart as PNG + CSV
        - Export Progress Report - Downloads progress as PDF
        - Export Risk Report - Downloads risks as PDF + Markdown
        - Export Cost Report - Downloads costs as CSV + PDF
      - Export to IDE Structure:
        - Generate folder/file structure
        - Create README files
        - Create .gitignore
        - Create config files
        - Create initial code stubs
        - Set up package.json, requirements.txt, Cargo.toml, etc.
        - Download as ZIP file
      - Import:
        - Import existing PRD (Markdown, JSON, PDF)
        - Import from other tools (JIRA, Trello, Asana, etc.)
        - Import project structure (from ZIP)
        - Import code base (analyze and create plan)
      - Export Workflow:
        - User clicks "Export"
        - Export dialog opens with format options (checkboxes)
        - User selects formats to export
        - User selects destination folder
        - Progress indicator (percentage + "Exporting...")
        - Success message with export location
        - "Open in folder" button
      
      ## Backup & Restore UI (Detailed Design)
      - Backup:
        - Full project backup button
        - Export as zip file
        - Include all versions
        - Include all media files
        - Backup schedule (dropdown: daily, weekly, monthly, manual)
        - Automatic backup location (configurable in settings)
        - Progress indicator during backup
        - Backup verification (checksum validation)
      - Restore:
        - Restore from backup file button
        - File picker for ZIP files
        - Selective restore (checkbox tree: specific files/versions)
        - Restore to new project (copy) option
        - Backup verification before restore
        - Progress indicator during restore
        - Success/failure message
      - Backup Locations:
        - Local folder (configurable)
        - External drive (browse to select)
        - Cloud storage (future: Google Drive, Dropbox)
      - Backup History:
        - List of past backups (date, size, location)
        - Delete old backups
        - Restore from any backup
      
      ## Keyboard Shortcuts UI (Detailed Design)
      - Global Shortcuts (configurable in settings):
        - Ctrl/Cmd + N: New Project
        - Ctrl/Cmd + O: Open Project
        - Ctrl/Cmd + S: Save Project
        - Ctrl/Cmd + Shift + S: Save Version
        - Ctrl/Cmd + K: Quick Navigate (command palette)
        - Ctrl/Cmd + P: Print/Export
        - Ctrl/Cmd + F: Search
        - Ctrl/Cmd + ,: Settings
        - Ctrl/Cmd + ?: Help
      - Editor Shortcuts:
        - Ctrl/Cmd + Z: Undo
        - Ctrl/Cmd + Y / Ctrl/Cmd + Shift + Z: Redo
        - Ctrl/Cmd + B: Bold
        - Ctrl/Cmd + I: Italic
        - Ctrl/Cmd + K: Insert Link
        - Ctrl/Cmd + Shift + K: Insert Code Block
      - Navigation Shortcuts:
        - Ctrl/Cmd + 1: Brain Dump
        - Ctrl/Cmd + 2: Structured Plan
        - Ctrl/Cmd + 3: Mind Map
        - Ctrl/Cmd + 4: PRD Preview
        - Ctrl/Cmd + 5: Wireframes
        - Ctrl/Cmd + 6: Tech Stack
        - Ctrl/Cmd + 7: MCP Services
        - Ctrl/Cmd + 8: Docker Config
        - Ctrl/Cmd + 9: Timeline
        - Ctrl/Cmd + 0: Versions
      - Customization:
        - All shortcuts can be customized in settings
        - Keyboard shortcut editor (press key to assign)
        - Preset profiles (VS Code, IntelliJ, Sublime, etc.)
        - Export/import shortcut profiles
        - Reset to defaults button
      
      ## Undo/Redo UI (Detailed Design)
      - Undo/Redo buttons (toolbar, top-right)
      - Keyboard shortcuts (Ctrl+Z/Ctrl+Y)
      - Scope:
        - Per-section undo/redo (context-aware)
        - Global undo/redo across sections
        - Configurable history size (default: 100)
      - What's Tracked:
        - Text edits
        - Task additions/deletions
        - Node reorganizations
        - Tag changes
        - Status changes
        - Property modifications
      - Features:
        - History panel (view undo stack with timestamps)
        - Clear history button
        - Selective undo (future: choose which action to undo)
        - Undo/Redo indicators (disabled when no history)
      
      ## Theme Support UI (Detailed Design)
      - Themes:
        - Light mode (default, white background, dark text)
        - Dark mode (dark background, light text)
        - System preference (auto-switch based on OS)
        - Custom themes (future: user-defined color schemes)
      - Theme Components:
        - Background colors
        - Text colors
        - Accent colors (primary, secondary)
        - Border colors
        - Syntax highlighting (code blocks)
        - UI element styling (buttons, cards, inputs)
      - Implementation:
        - Uses next-themes (for React/Tauri)
        - Persists user preference in localStorage
        - Smooth transitions (CSS transitions)
        - Consistent across all components
      - Theme Switcher:
        - Settings > Theme dropdown
        - Preview theme (live preview card)
        - Apply theme button
        - Reset to default button
      
      ## Accessibility UI (Detailed Design)
      - Features:
        - Screen reader support (ARIA labels, roles, live regions)
        - Keyboard navigation for all features (tab index, focus management)
        - ARIA labels and roles on all interactive elements
        - Focus indicators (visible focus rings)
        - High contrast mode (toggle in settings)
        - Text scaling support (font size slider)
        - Color blind friendly color schemes (deuteranopia, protanopia, tritanopia)
        - Skip to main content links (hidden until focused)
        - Alt text for all images
        - Descriptive link text (no "click here")
      - Standards:
        - WCAG 2.1 AA compliant
        - Semantic HTML (proper heading hierarchy)
        - Proper heading hierarchy (h1, h2, h3, etc.)
      - Accessibility Testing:
        - Keyboard-only navigation test
        - Screen reader test (NVDA, JAWS)
        - Color contrast checker (WCAG contrast ratio)
        - Focus order test (logical tab order)
      
      ## Offline Mode UI (Detailed Design)
      - Capabilities:
        - View and edit projects without internet
        - Brain dump and plan offline
        - Local version control works offline
        - Sync when internet available
      - Limitations (Offline):
        - AI features unavailable (grayed out, tooltip "Requires internet")
        - MCP market scanning unavailable
        - Cloud backups unavailable
      - Features:
        - Offline indicator (status bar: "Offline" badge)
        - Queue AI requests for when online (show pending count)
        - Manual sync button (prominent when offline)
        - Conflict resolution when syncing (diff viewer)
        - Auto-sync when connection restored
        - Sync progress indicator
      - Offline Storage:
        - IndexedDB for local cache
        - Service Worker for offline assets
        - Cache AI responses when online
      
      ## Onboarding Tutorial UI (Detailed Design)
      - First-Time Experience:
        - Welcome screen with app name "IdeaForge" and tagline "Transform ideas into actionable plans"
        - "Get Started" button (prominent, centered)
        - "Skip tutorial" link (small, bottom-right)
        - Tutorial tour (step-by-step walkthrough):
          1. Dashboard overview (highlight dashboard, explain cards)
          2. Creating a project (walk through project creation flow)
          3. Brain dumping (show brain dump editor, explain voice input)
          4. Working with AI (show chat interface, explain AI capabilities)
          5. Using mind maps (show mind map, explain CRUD operations)
          6. Generating PRDs (show PRD preview, explain export)
          7. Version control (show versions tab, explain commits)
          8. Exporting (show export menu, explain formats)
      - Tutorial Topics (accessible from Help menu):
        - Creating a project
        - Brain dumping ideas
        - Working with AI
        - Using mind maps
        - Generating PRDs
        - Version control
        - Exporting
      - Features:
        - Skip tutorial option (don't show again)
        - Re-run tutorial anytime (Help > Tutorial)
        - Contextual help tooltips (hover over elements)
        - Video tutorials (future: embedded video player)
        - Interactive examples (sample project to explore)
        - Progress indicator (X/8 steps completed)
        - "Next" and "Previous" navigation
        - "Finish tutorial" button
      
      ## Citations & References UI (Detailed Design)
      - AI Transparency:
        - Where AI got information from (source links)
        - Documentation links (clickable)
        - Best practice sources (references)
        - Reference articles (links to articles)
        - RFCs and standards (links to RFCs)
      - Citation Format:
        - In-line citations in generated content ([1], [2], etc.)
        - Reference list at bottom (numbered list)
        - Clickable links (open in new tab)
        - Source reliability rating (stars: 1-5)
      - Features:
        - Filter by source type (dropdown: Documentation, Articles, RFCs, Standards)
        - Export references (download as JSON)
        - Add custom references (manual entry)
        - Verify links (check if links are valid)
        - Reference panel (sidebar showing all citations)
      
      ## Learning Resources UI (Detailed Design)
      - AI Suggestions:
        - Tutorial links for chosen tech stack
        - Documentation links (official docs)
        - Best practice guides (articles, blog posts)
        - Video tutorials (YouTube, Vimeo links)
        - Courses recommendations (Udemy, Coursera, etc.)
        - Books recommendations (Amazon links)
      - Organization:
        - Categorized by technology (React, Rust, Python, etc.)
        - Categorized by skill level (Beginner, Intermediate, Advanced)
        - Filter by type (video, article, course, book)
        - User favorites (star items)
        - Mark as completed (checkmark)
      - Resources Panel:
        - Resources tab in project view
        - List of resources with: title, type, skill level, source link, favorite star, completed checkmark
        - Click to open resource (new tab)
        - Add custom resource button
        - Delete resource button
        - Export resources list (JSON)
      
      ## Code Snippet Generation UI (Detailed Design)
      - Purpose:
        - For tricky patterns (authentication, database connections)
        - For common implementations (API endpoints, CRUD operations)
        - For boilerplate code (config files, setup code)
        - For configuration examples (Dockerfiles, docker-compose.yml)
      - Features:
        - Syntax highlighting (based on language)
        - Copy to clipboard button
        - Language-specific (auto-detected or manual selection)
        - Inline explanations (comments in code)
        - Export as code files (download as .py, .ts, .rs, etc.)
        - Insert into PRD button
      - Snippet Types:
        - Authentication patterns (OAuth, JWT, session management)
        - Database connection (Prisma, raw SQL, ORM queries)
        - API endpoints (Express, Fastify, Actix examples)
        - Configuration files (package.json, Cargo.toml, requirements.txt)
        - Dockerfiles (multi-stage, optimized)
        - CI/CD pipelines (GitHub Actions, GitLab CI)
        - Testing examples (Jest, PyTest, Rust tests)
      - Snippet UI:
        - Code block with line numbers
        - Language selector (dropdown)
        - Copy button (clipboard icon)
        - Download button
        - "Insert into PRD" button
        - Explanation panel (below code)

  # Phase 6: Testing & Quality Assurance
  - name: "testing-agent"
    description: "Writes comprehensive tests, performs QA, and implements quality gates"
    working_dir: "tests"
    priority: 6
    depends_on: ["polish-agent"]
    requirements: |
      Implement comprehensive testing and quality assurance:
      
      ## Testing Requirements (from PRD)
      - After Every Phase: Must create and run tests to ensure code works correctly
      - Test Coverage: Maintain comprehensive test coverage across all modules (>80%)
      - Integration Testing: Test all integrations (AI, database, file system)
      - E2E Testing: End-to-end testing for critical user workflows
      - Automated Tests: CI/CD pipeline with automated test execution
      
      ## Unit Tests (Frontend)
      - Jest configuration
      - React Testing Library setup
      - Test components: Dashboard, Project creation, Brain dump, Structured plan, Mind map, etc.
      - Test hooks: Zustand stores, React Query queries
      - Mock API calls
      - Snapshot testing for UI components
      - Coverage reporting (Istanbul/nyc)
      
      ## Unit Tests (Backend)
      - Rust test setup
      - Test database operations (Prisma models)
      - Test file system operations
      - Test AI API integration (mocked)
      - Test version control logic
      - Test MCP integration (mocked)
      - Test Docker config generation
      - Coverage reporting (tarpaulin)
      
      ## Integration Tests
      - Test API endpoints (Tauri commands)
      - Test database queries
      - Test file system operations
      - Test AI API calls (with test API key)
      - Test MCP integration (mocked servers)
      - Test export/import functionality
      - Test backup/restore
      
      ## E2E Tests (Playwright)
      - Critical user workflows:
        - Create new project
        - Brain dump with voice input
        - Generate structured plan
        - Create mind map
        - Generate PRD
        - Export project
        - Create version
        - Rollback to version
      - Test all UI interactions
      - Test keyboard shortcuts
      - Test accessibility (keyboard-only navigation)
      - Test offline mode
      - Cross-browser testing (if web version)
      
      ## Performance Tests
      - Load testing for large projects
      - Memory leak detection
      - Render performance (React DevTools profiler)
      - Database query performance
      - AI API response time monitoring
      
      ## Accessibility Audit
      - WCAG 2.1 AA compliance check
      - Screen reader testing (NVDA, JAWS)
      - Keyboard navigation testing
      - Color contrast verification
      - Focus order testing
      - ARIA attribute validation
      
      ## Security Review
      - API key storage review (never log or expose)
      - Input validation testing
      - SQL injection prevention
      - XSS prevention
      - CSRF protection (if web version)
      - File upload security
      - Dependency vulnerability scan (cargo-audit, npm audit)
      
      ## Bug Fixes
      - Fix bugs found during testing
      - Prioritize by severity (Critical, High, Medium, Low)
      - Regression testing after fixes
      - Update test cases for fixed bugs
      
      ## Test Coverage Reports
      - Generate coverage reports (HTML + JSON)
      - Enforce coverage thresholds (>80%)
      - Identify untested code
      - Create coverage badges (for README)
      
      ## Quality Gates
      - All tests must pass before merging
      - Code must be linted (ESLint, rust-analyzer)
      - Code must be formatted (Prettier, rustfmt)
      - No security vulnerabilities
      - Minimum test coverage met
      - Documentation updated for new features

  # Phase 7: Documentation & Release
  - name: "documentation-agent"
    description: "Writes documentation, creates release builds, and prepares distribution"
    working_dir: "."
    priority: 7
    depends_on: ["testing-agent"]
    requirements: |
      Complete documentation and prepare for release:
      
      ## User Documentation
      - Getting Started Guide:
        - Installation instructions (Linux, macOS, Windows)
        - First-time setup
        - Creating your first project
        - Brain dumping
        - Generating PRDs
        - Exporting
      - Features Documentation:
        - Complete feature reference (all 28+ features)
        - Screenshots and examples
        - Keyboard shortcuts reference
        - Tips and tricks
      - FAQ Section:
        - Common questions
        - Troubleshooting
        - Known issues
      - Video Tutorials (future):
        - YouTube channel links
        - Embedded video player
        - Step-by-step walkthroughs
      
      ## Developer Documentation
      - Architecture Documentation:
        - System architecture diagram
        - Component interactions
        - Data flow
        - Technology choices
      - API Documentation:
        - Tauri commands reference
        - Request/response formats
        - Error codes
        - Examples
      - Contributing Guide:
        - Development setup
        - Code style guidelines
        - Testing guidelines
        - Pull request process
      - Database Schema Documentation:
        - Complete schema reference
        - Relationships
        - Migrations
      - AI Integration Documentation:
        - z.ai API integration
        - GLM 4.7 MCP integration
        - Prompt templates
        - Error handling
      
      ## Installation Guides
      - Linux:
        - Package manager installation (pacman, apt, dnf)
        - Dependencies installation
        - Build from source
        - Troubleshooting
      - macOS:
        - Homebrew installation
        - DMG download
        - Code signing requirements
        - Troubleshooting
      - Windows:
        - MSI installer
        - Portable ZIP
        - Dependencies (Visual C++ Redistributable)
        - Troubleshooting
      
      ## README.md
      - Project overview
      - Features list (with badges)
      - Quick start guide
      - Installation instructions
      - Usage examples
      - Screenshots
      - Contributing section
      - License information
      - Links to documentation
      
      ## Onboarding Tutorial (in-app)
      - Interactive walkthrough (already implemented in Phase 5)
      - Context-sensitive help
      - Tooltips
      - Sample project
      
      ## Release Builds
      - Linux builds:
        - AppImage (universal)
        - DEB package (Debian/Ubuntu)
        - RPM package (Fedora/openSUSE)
        - Tarball (source)
      - macOS builds:
        - DMG (Intel)
        - DMG (Apple Silicon)
        - Universal binary
        - Code signed
        - Notarized
      - Windows builds:
        - MSI installer (x64)
        - MSI installer (ARM64)
        - Portable ZIP
        - Code signed
      - Build automation:
        - GitHub Actions workflows
        - Version tagging
        - Release notes generation
        - Asset upload
      
      ## Distribution Channels
      - GitHub Releases:
        - Upload release assets
        - Create release notes
        - Tag releases
      - Website (future):
        - Download page
        - Version history
        - Changelog
      
      ## Release Notes
      - CHANGELOG.md:
        - Version history
        - New features
        - Bug fixes
        - Breaking changes
        - Migration guides
      - GitHub Release Notes:
        - Summary of changes
        - Upgrade instructions
        - Known issues
      
      ## Documentation Quality
      - Clear and concise
      - Screenshots and diagrams
      - Code examples
      - Cross-references
      - Searchable
      - Accessible (WCAG 2.1 AA)
      
      ## Final Deliverables
      - Complete user documentation
      - Complete developer documentation
      - Installation guides for all platforms
      - README.md
      - CHANGELOG.md
      - Release builds (Linux, macOS, Windows)
      - Distribution channels set up
      - Release notes published
