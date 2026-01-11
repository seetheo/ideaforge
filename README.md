# IdeaForge - AI-Powered Application Planning Tool

## Overview

IdeaForge is an AI-powered application planning and development tool that helps developers transform unstructured "brain dumps" into comprehensive, actionable Project Requirements Documents (PRDs) ready for AI agents to systematically build applications.

## Features

- **Brain Dump Experience**: Rich text editor with voice input, audio recording, and image upload
- **AI Processing**: Automatic feature extraction, task breakdown, wireframe generation
- **Structured Planning**: Features, tasks, GUI/UX, and design specifications
- **Interactive Mind Maps**: Full CRUD operations with two-way sync
- **Version Control**: GitHub-style versioning with full state snapshots
- **Context-Aware Chat**: AI assistant that understands your current context
- **Tech Stack Recommendations**: Polyglot optimization with pros/cons analysis
- **MCP Integration**: Automatic MCP Market scanning and recommendations
- **Docker Support**: Complete Dockerfile and docker-compose.yml generation
- **Project Dashboard**: Overview with statistics, filters, and quick actions
- **Template Management**: 34 pre-built templates for various project types
- **Progress Tracking**: Visual progress indicators and reports
- **Export Options**: Multiple formats including PRD, JSON, mind maps, and IDE structure

## Technology Stack

- **Frontend**: React + TypeScript + Tailwind CSS + shadcn/ui
- **Backend**: Rust (Tauri)
- **Database**: SQLite (Prisma ORM)
- **State Management**: Zustand (client), TanStack Query (server)
- **AI Providers**: z.ai API (primary), GLM 4.7 MCP Server (images)

## Development

This project is set up for **G3 Flock Mode** - parallel multi-agent development with 7 specialized agents:

1. **foundation-agent**: Tauri setup, database schema, project dashboard
2. **brain-dump-agent**: Brain dump editor, voice input, AI analysis
3. **structured-plan-agent**: Structured plan UI, mind map, version control, chat
4. **advanced-features-agent**: Wireframes, MCP integration, Docker config, templates
5. **polish-agent**: Search, progress tracking, export, backup, UX improvements
6. **testing-agent**: Comprehensive testing and quality assurance
7. **documentation-agent**: User documentation, developer docs, release builds

### Quick Start

```bash
# Run G3 flock mode
cd ~/Documents/repos && ~/Documents/repos/g3-cachyos-hyprland/target/release/g3 --flock-workspace . --segments 7 --project ideaforge --auto --requirements "$(cat ideaforge/flock.yaml)"
```

## Project Status

- **Phase**: Planning and Setup
- **Progress**: 0% complete
- **Next Steps**: Execute G3 flock mode to begin implementation

## License

MIT License - See LICENSE file for details

## Contributing

Contributions welcome! Please read CONTRIBUTING.md for guidelines.
