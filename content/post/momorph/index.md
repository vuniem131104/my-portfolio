---
title: Agentic Coding System
description: An agentic coding system leveraging LangGraph and Agent mode in Copilot Chat VSCode. This system can autonomously plan, generate, and manage code tasks to enhance software development efficiency.
slug: agentic-coding-system
date: 2025-10-01 00:00:00+0000
image: cover.jpg
categories:
    - Company Projects
tags:
    - AI
    - Coding Agent
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

**Github Repository:** Private

# MoMorph VSCode Extension

**Sun* MoMorph** is an AI-powered VSCode extension that transforms Figma designs into production-ready code. Built on top of GitHub Copilot's language models and custom AI agents, MoMorph automates the journey from design to implementation with pixel-perfect accuracy.

## Key Features

### Figma-to-Code Generation
- **Design Integration**: Direct integration with Figma files linked to your Git repository
- **Visual Analysis**: Download and analyze Figma frames with detailed style extraction
- **Pixel-Perfect Output**: Generate UI components matching exact design specifications
- **Screenshot Comparison**: Automated visual regression testing against Figma designs

### AI-Powered Workflows
- **Specification Generation**: Automatically create detailed feature specs from Figma designs
- **Technical Planning**: Generate implementation plans with architecture decisions
- **Code Generation**: Create Next.js components, NestJS backends, and database schemas
- **Multi-Agent System**: Specialized agents for specification, planning, and implementation

### Developer Tools
- **Chat Participant**: Interact with `@momorph` in GitHub Copilot Chat
- **Language Model Tools**: Custom tools for Figma integration, screenshot comparison, and MCP protocol
- **MCP Support**: Model Context Protocol integration for extensibility
- **Tree View UI**: Browse Figma files, frames, and project contexts directly in VSCode

### Supported Use Cases
- Next.js frontend development with React/TypeScript
- NestJS backend API generation
- Database schema design from Figma prototypes
- UI component testing and validation

## Project Structure

This is a **monorepo** managed with Yarn workspaces and Turborepo:

```
morpheus-vscode/
├── packages/
│   ├── core/          # Core functionality & AI workflows
│   │   └── src/
│   │       ├── chat/         # LangGraph workflows & AI agents
│   │       ├── figma/        # Figma API client
│   │       ├── github/       # GitHub integration
│   │       ├── mcp/          # Model Context Protocol
│   │       └── morpheus/     # Morpheus AI client
│   ├── vscode/        # VSCode extension implementation
│   │   ├── src/
│   │   │   ├── chat/         # Chat participant & handlers
│   │   │   ├── commands/     # VSCode commands
│   │   │   ├── tree/         # Tree view providers
│   │   │   └── views/        # Webview panels
│   │   └── resources/
│   │       ├── copilot/      # AI agent definitions
│   │       ├── icons/        # Extension icons
│   │       └── prompts/      # System prompts
│   └── ui/            # Svelte-based webview UI
│       └── src/
│           ├── components/   # UI components
│           └── pages/        # Webview pages
├── libs/
│   ├── ipc/           # Inter-process communication
│   └── tree-sitter/   # Code parsing utilities
├── deploy/            # Docker deployment configs
└── docs/              # Documentation
```

### Package Overview

- **`core`**: Business logic, AI workflows using LangGraph, and integrations (Figma, GitHub, Morpheus AI)
- **`vscode`**: VSCode extension host, activation, commands, tree views, and chat participant
- **`ui`**: Svelte-based webview interface for browsing Figma files and contexts
- **`libs/ipc`**: Type-safe IPC messaging between extension and webview
- **`libs/tree-sitter`**: Language parsing for code understanding

## Prerequisites

- **Node.js**: Latest LTS version recommended (v20+)
- **VSCode**: Version 1.105.0 or higher
- **Yarn**: 4.6.0 (automatically installed via packageManager field)
- **GitHub Copilot**: Active subscription required for AI features
- **Figma Account**: For design file integration

## Getting Started

### 1. Clone the Repository

```bash
git clone <repository-url>
cd morpheus-vscode
```

### 2. Setup Registry Authentication

Since this project uses private packages from GitHub Packages:

1. **Accept invitation** to the npm packages repo ([sun-asterisk-research/morpheus-npm-pkgs](https://github.com/sun-asterisk-research/morpheus-npm-pkgs/invitations))
   - Skip if you're already a member of [sun-asterisk-research](https://github.com/sun-asterisk-research)
2. **Create a Personal Access Token** with `read:packages` permission ([Create Token](https://github.com/settings/tokens))
3. **Login to registry**:
   ```bash
   yarn npm login --scope @sun-asterisk-research
   ```
   - Username: Your GitHub username
   - Password: Your personal access token

### 3. Install Dependencies

```bash
yarn install
```

This will install all dependencies for all packages in the monorepo.

## Development Workflow

### Available Scripts

Run these commands from the root directory:

| Command | Description |
|---------|-------------|
| `yarn dev` | Start development mode with watch (all packages) |
| `yarn build` | Build all packages for production |
| `yarn build:dev` | Build all packages in development mode |
| `yarn build:types` | Generate TypeScript declaration files |
| `yarn typecheck` | Run TypeScript type checking |
| `yarn lint` | Run ESLint on all packages |
| `yarn test` | Run tests across all packages |
| `yarn package` | Create VSIX package for extension distribution |
| `yarn pre-commit` | Format, lint, and typecheck before committing |

### Working on the Extension

#### 1. Start Development Server

```bash
yarn dev
```

This starts watch mode for all packages. Changes will be automatically recompiled.

#### 2. Launch Extension in Debug Mode

Choose one of these methods:

**Method A**: Press `F5` (default debug shortcut)

**Method B**: Use Run & Debug panel
1. Open Run & Debug sidebar (`Ctrl+Shift+D`)
2. Select **"Run Extension (morpheus-vscode)"**
3. Click the green play button

This opens a new **Extension Development Host** window with MoMorph loaded.

#### 3. Test the Extension

In the Extension Development Host window:

1. **Open Figma Tree View**: Click the MoMorph icon in the Activity Bar
2. **Use Chat Participant**: Open GitHub Copilot Chat and type:
   ```
   @momorph Hi! What can you do?
   ```
3. **Test Figma Integration**: Ensure your workspace is linked to a Figma file

#### 4. Reload Changes

- **Extension code changes** (`core` or `vscode`):
  - Press `Ctrl+Shift+F5` in Extension Development Host
  - Or use the "Restart" button in the debug toolbar
  - See [VSCode Debugging Docs](https://code.visualstudio.com/docs/editor/debugging#_start-a-debugging-session)

- **Webview UI changes** (`ui` package):
  - Click on any tree item to reload the webview
  - Or use the "Reload" button in the webview toolbar

### Developing Chat Features

MoMorph uses **LangGraph** for AI workflows. Read the detailed guide:

[Chat Development Documentation](./docs/chat_development.md)

**Quick Overview**:
- **Workflows**: Define multi-step AI processes in `packages/core/src/chat/graphs/workflows/`
- **Agents**: Custom Copilot agents in `packages/vscode/resources/copilot/agents/`
- **Tools**: Language model tools registered in `packages/vscode/package.json`

### Code Quality & Pre-commit Checks

Before committing, run:

```bash
yarn pre-commit
```

This will:
1. Format code with Prettier
2. Run ESLint checks
3. Perform TypeScript type checking

### Project Configuration Files

| File | Purpose |
|------|---------|
| `.vscode/` | VSCode settings and launch configurations |
| `.yarnrc.yml` | Yarn package manager configuration |
| `turbo.json` | Turborepo build pipeline and caching |
| `tsconfig.base.json` | Shared TypeScript configuration |
| `eslint.config.mjs` | ESLint rules and plugins |
| `package.json` | Workspace configuration and scripts |

## Building and Packaging

### Create VSIX Package

To create a distributable `.vsix` file:

1. **Ensure tests pass**:
   ```bash
   yarn test
   ```

2. **Build the extension**:
   ```bash
   yarn build
   ```

3. **Create VSIX package**:
   ```bash
   yarn package
   ```

The `.vsix` file will be generated in `packages/vscode/dist/`.

### Installing the VSIX

To install the extension from VSIX:

```bash
code --install-extension packages/vscode/dist/vscode-momorph-*.vsix
```

Or use VSCode UI:
1. Open Extensions view (`Ctrl+Shift+X`)
2. Click `...` (Views and More Actions)
3. Select "Install from VSIX..."
4. Choose the generated `.vsix` file

## Testing

The project uses VSCode Extension Test framework:

```bash
yarn test
```

This runs tests across all packages that have test suites configured.

## Architecture

### Technology Stack

- **Language**: TypeScript
- **Package Manager**: Yarn 4 with workspaces
- **Build System**: Turborepo for monorepo orchestration
- **Bundler**: esbuild (extension) + Vite (UI)
- **UI Framework**: Svelte 5 with TypeScript
- **AI Framework**: LangGraph for multi-agent workflows
- **Testing**: VSCode Extension Test Runner

### Key Integrations

- **GitHub Copilot**: Base language models and chat participant API
- **Figma API**: Design file access and image rendering
- **Model Context Protocol**: Extensible tool system
- **Morpheus AI**: Custom code generation backend
- **GitHub API**: Repository and authentication management

### Communication Flow

```
VSCode Extension Host
    ↓
Chat Participant (@momorph)
    ↓
LangGraph Workflows (AI Agents)
    ↓
    ├─→ Language Model (GitHub Copilot)
    ├─→ Tools (Figma, GitHub, File Operations)
    ├─→ MCP Servers
    └─→ Morpheus AI Backend
```