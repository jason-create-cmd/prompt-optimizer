# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Prompt Optimizer is a powerful AI prompt optimization tool that helps users write better AI prompts and improve AI output quality. It supports four deployment methods: Web application, desktop application, Chrome extension, and Docker deployment.

## Key Commands

### Development Commands
```bash
# Install dependencies
pnpm install

# Web development (builds core/ui and runs web app)
pnpm dev

# Web development with full reset
pnpm dev:fresh

# Desktop development (builds core/ui, runs web and desktop)
pnpm dev:desktop

# Desktop development with full reset
pnpm dev:desktop:fresh

# Extension development
pnpm dev:ext
```

### Build Commands
```bash
# Build all packages (core → ui → web/ext/desktop parallel)
pnpm build

# Build specific packages
pnpm build:core        # Core package
pnpm build:ui          # UI components
pnpm build:web         # Web application
pnpm build:ext         # Browser extension
pnpm build:desktop     # Desktop application (includes packaging)
```

### Testing
```bash
# Run all tests
pnpm test

# Run specific package tests
pnpm -F @prompt-optimizer/core test
pnpm -F @prompt-optimizer/ui test
pnpm -F @prompt-optimizer/web test
```

### Clean Commands
```bash
# Clean all build artifacts
pnpm clean

# Clean only dist folders
pnpm clean:dist

# Clean Vite cache
pnpm clean:vite
```

### MCP Server Commands
```bash
# Build MCP server
pnpm mcp:build

# Develop MCP server
pnpm mcp:dev

# Start MCP server
pnpm mcp:start

# Test MCP server
pnpm mcp:test
```

## Architecture

### Monorepo Structure
This project uses a monorepo architecture with the following packages:

- **`@prompt-optimizer/core`**: Core logic package containing AI model integrations, prompt optimization algorithms, and business logic
- **`@prompt-optimizer/ui`**: UI components package with reusable Vue components and Element Plus integrations
- **`@prompt-optimizer/web`**: Web application package that combines core and UI for web deployment
- **`@prompt-optimizer/extension`**: Browser extension package for Chrome
- **`@prompt-optimizer/desktop`**: Desktop application package built with Electron
- **`@prompt-optimizer/mcp-server`**: MCP (Model Context Protocol) server package

### Build Order
Packages must be built in dependency order: `core` → `ui` → (`web`/`extension`/`desktop` parallel)

### Key Technologies
- **Frontend**: Vue 3 + TypeScript + Element Plus
- **Desktop**: Electron (with auto-update support)
- **Build Tool**: Vite
- **Package Manager**: pnpm (required)
- **Testing**: Vitest
- **AI Integration**: OpenAI, Gemini, DeepSeek, Zhipu, SiliconFlow APIs

## Development Workflow

### Branch Strategy
- **`main`**: Production branch (triggers Vercel auto-deployment)
- **`develop`**: Development branch (no Vercel deployment)
- **`feature/*`**: Feature branches (forked from develop)

### Commit Convention
```bash
<type>(<scope>): <subject>

# Examples
feat(ui): add new prompt editor component
fix(core): resolve API call timeout issue
docs: update development guide
```

### Development Process
1. Checkout from `develop` branch
2. Create feature branch
3. Develop and test locally
4. Merge back to `develop` (no deployment)
5. When ready, merge `develop` to `main` (triggers Vercel deployment)

## Environment Configuration

### Local Development
Create `.env.local` in project root:
```env
VITE_OPENAI_API_KEY=your_openai_key
VITE_GEMINI_API_KEY=your_gemini_key
VITE_DEEPSEEK_API_KEY=your_deepseek_key
VITE_ZHIPU_API_KEY=your_zhipu_key
VITE_SILICONFLOW_API_KEY=your_siliconflow_key
```

### Multi Custom Models
The project supports unlimited custom model configurations:
```env
# Custom model 1
VITE_CUSTOM_API_KEY_ollama=dummy_key
VITE_CUSTOM_API_BASE_URL_ollama=http://localhost:11434/v1
VITE_CUSTOM_API_MODEL_ollama=qwen2.5:7b

# Custom model 2
VITE_CUSTOM_API_KEY_custom2=your_key
VITE_CUSTOM_API_BASE_URL_custom2=your_base_url
VITE_CUSTOM_API_MODEL_custom2=your_model
```

## Deployment

### Vercel Deployment
1. Fork the repository
2. Import to Vercel
3. Configure environment variables
4. Deploy (automatic on push to `main` branch)

### Docker Deployment
```bash
# Build and run
docker build -t prompt-optimizer .
docker run -d -p 8081:80 --name prompt-optimizer

# With environment variables
docker run -d -p 8081:80 \
  -e VITE_OPENAI_API_KEY=your_key \
  -e ACCESS_PASSWORD=your_password \
  --name prompt-optimizer \
  prompt-optimizer
```

### Desktop Application Release
```bash
# Prepare version
pnpm version:prepare minor
git commit -m "chore: bump version"

# Create and push tag (triggers GitHub Actions build)
pnpm run version:tag
pnpm run version:publish
```

## Important Notes

### CORS Considerations
- The web application may encounter CORS issues when connecting to local AI services
- Use desktop version for local model connections (no CORS restrictions)
- Vercel deployment includes proxy functionality for API calls

### Security
- All API keys are client-side only (no backend server)
- Data flows directly from browser to AI service providers
- Access password protection available for deployments

### Testing Requirements
- Run `pnpm test` after any code changes
- Ensure all tests pass before committing
- New features should include corresponding tests

## File Structure References

- `docs/` - Comprehensive project documentation
- `.cursor/rules/` - Cursor IDE rules and code review guidelines
- `packages/` - Monorepo packages
- `scripts/` - Build and utility scripts

## MCP Server Integration

The project includes MCP (Model Context Protocol) server support:
- Available at `/mcp` endpoint when deployed
- Supports Claude Desktop integration
- Tools: optimize-user-prompt, optimize-system-prompt, iterate-prompt

## Cursor Rules

The project includes comprehensive Cursor rules in `.cursorrules` and `.cursor/rules/`:
- Code review guidelines
- Development environment standards
- Testing requirements
- Documentation management practices