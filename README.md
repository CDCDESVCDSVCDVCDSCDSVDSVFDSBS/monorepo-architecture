# Monorepo Template

A production-ready Nx monorepo template for building modern full-stack applications with React, NestJS, GraphQL, TypeORM and modern development tools.

## 🏗️ Architecture Overview

This monorepo follows a modular architecture with clear separation of concerns:

```
monorepo-template/
├── packages/
│   ├── marketplace-front/      # React frontend application
│   ├── marketplace-server/     # NestJS backend application  
│   ├── marketplace-ui/         # Shared UI component library
│   └── marketplace-shared/     # Shared utilities and types
├── docker-compose.yml          # Development infrastructure
├── Dockerfile                  # Production container
├── vercel.json                 # Frontend deployment config
└── nx.json                     # Nx workspace configuration
```

## 🚀 Technology Stack

### Core Framework
- **Nx 21.2.0**: Build system and development tools
- **Yarn 4.4.0**: Package manager with workspaces
- **TypeScript 5.8.2**: Type-safe development
- **Node.js 20**: Runtime environment

### Frontend (`marketplace-front`)
- **React 19.0.0**: Modern React with latest features
- **Vite 6.0.0**: Fast development server and build tool
- **React Router DOM 6.29.0**: Client-side routing
- **Apollo Client 3.13.8**: GraphQL client with caching
- **React Helmet Async 2.0.5**: Document head management

### Backend (`marketplace-server`)
- **NestJS 11.0.0**: Scalable Node.js server framework
- **Apollo Server 4.12.2**: GraphQL server implementation
- **TypeORM 0.3.25**: Database ORM with migrations
- **PostgreSQL**: Primary database (via Docker)
- **Redis/BullMQ**: Job queue and caching
- **GitHub Integration**: Octokit GraphQL client

### UI Library (`marketplace-ui`)
- **shadcn/ui**: Modern component library
- **Tailwind CSS 3.4.3**: Utility-first CSS framework
- **Custom Design System**: Consistent theming and components

### Shared (`marketplace-shared`)
- **Utility Functions**: Common helpers and validators
- **Type Definitions**: Shared TypeScript interfaces
- **Constants**: Application-wide constants

### Development Tools
- **ESLint 9.8.0**: Code linting with TypeScript support
- **Prettier 2.6.2**: Code formatting
- **Vitest 3.0.0**: Unit testing framework
- **GraphQL Code Generator**: Type-safe GraphQL operations

### Infrastructure
- **Docker & Docker Compose**: Development environment
- **PostgreSQL 15**: Database with custom schemas
- **Redis Stack**: Caching and job queues
- **Vercel**: Frontend deployment platform

## 📦 Package Structure

### 🎨 Frontend Application (`marketplace-front`)
- **Type**: Application (`type:app`, `scope:front`)
- **Framework**: React with Vite
- **Features**:
  - GraphQL integration with code generation
  - Modern React Router setup
  - Responsive design with Tailwind CSS
  - Component-driven architecture

### 🔧 Backend Application (`marketplace-server`)
- **Type**: Application (`type:app`, `scope:server`)
- **Framework**: NestJS with GraphQL
- **Features**:
  - Modular architecture with feature modules
  - Database integration with TypeORM
  - Redis-backed job queues
  - GitHub API integration
  - Health monitoring endpoints
  - Database migrations system

### 🎨 UI Component Library (`marketplace-ui`)
- **Type**: Library (`type:lib`, `scope:ui`)
- **Framework**: shadcn/ui + Tailwind CSS
- **Features**:
  - Reusable component library
  - Custom design system
  - Dark/light theme support
  - Accessibility-first components

### 📚 Shared Library (`marketplace-shared`)
- **Type**: Library (`type:lib`, `scope:shared`)
- **Purpose**: Cross-package utilities and types
- **Features**:
  - Type-safe utility functions
  - Shared constants and enums
  - Common validation logic

## 🎯 Available Targets

### Core Targets (All Packages)
```bash
# Type checking
nx typecheck <package>

# Code linting  
nx lint <package>

# Build for production
nx build <package>
```

### Frontend-Specific Targets (`marketplace-front`)
```bash
# Development server
nx dev marketplace-front

# Build for production
nx build marketplace-front

# Preview production build
nx preview marketplace-front

# Generate GraphQL types
nx graphql:codegen marketplace-front
```

### Backend-Specific Targets (`marketplace-server`)
```bash
# Start development server
nx start marketplace-server

# Start production server
nx start:production marketplace-server

# Database migrations
nx migration:deploy marketplace-server
nx migration:revert marketplace-server
nx migration:generate marketplace-server --args.name=CreateUsers
nx migration:create marketplace-server --args.name=AddIndexes
nx migration:show marketplace-server
nx migration:schema:sync marketplace-server
```

### UI Library Targets (`marketplace-ui`)
```bash
# Add shadcn/ui components
nx shadcn:component:add marketplace-ui --args.component=button
nx shadcn:component:add marketplace-ui --args.component=dialog
```

### Multi-Package Operations
```bash
# Build all packages
nx run-many -t build

# Type check all packages
nx run-many -t typecheck

# Lint all packages
nx run-many -t lint

# Run affected tasks only
nx affected -t build,test,lint
```

## 🛠️ Development Setup

### Prerequisites
- Node.js 20+ (specified in `.nvmrc`)
- Yarn 4.4.0+ 
- Docker & Docker Compose

### Quick Start
```bash
# Clone the repository
git clone <repository-url>
cd monorepo-template

# Install dependencies
yarn install

# Start development infrastructure
docker-compose up -d

# Start backend development server
nx start marketplace-server

# Start frontend development server (in another terminal)
nx dev marketplace-front
```

### Environment Configuration
Create `.env` file in the root directory:
```env
# Database
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_NAME=postgres

# Redis
REDIS_HOST=127.0.0.1
REDIS_PORT=6378
REDIS_DB=0

# Server
SERVER_PORT=3000
SERVER_URL=http://localhost:3000

# GitHub Integration (optional)
GITHUB_PERSONAL_TOKEN=your_github_token

# Environment
NODE_ENV=development
```

## 🔧 Architecture Patterns

### Module Boundaries
The workspace enforces strict module boundaries:
- **Frontend** (`scope:front`): Can depend on `ui`, `shared`
- **Backend** (`scope:server`): Can depend on `shared` only
- **UI Library** (`scope:ui`): Can depend on `shared` only  
- **Shared** (`scope:shared`): No external dependencies

### Database Schema Organization
- **`core`**: Application core entities
- **`discovery_source`**: Data discovery and integration
- **`public`**: Default PostgreSQL schema

### GraphQL Architecture
- Code-first approach with NestJS
- Automatic schema generation
- Type-safe client code generation
- Apollo Client with caching

## 🐳 Docker & Deployment

### Development Environment
```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down
```

### Production Deployment
```bash
# Build production image
docker build -t monorepo-template .

# Run production container
docker run -p 3000:3000 monorepo-template
```

### Frontend Deployment (Vercel)
The frontend is configured for automatic deployment to Vercel:
- Build command: `yarn nx build marketplace-front`
- Output directory: `packages/marketplace-front/dist`
- Framework: Vite
- Supports SPA routing with rewrites

## 🧪 Testing

```bash
# Run all tests
nx run-many -t test

# Run tests for specific package
nx test marketplace-server
nx test marketplace-front

# Run tests in watch mode
nx test marketplace-server --watch

# Run tests with coverage
nx run-many -t test --coverage
```

## 📈 Performance & Optimization

### Build Optimization
- **SWC**: Fast TypeScript compilation
- **Vite**: Optimized frontend builds
- **Nx Caching**: Intelligent build caching
- **Tree Shaking**: Unused code elimination

### Development Experience
- **Hot Module Replacement**: Fast development iterations
- **TypeScript**: Type safety across the stack
- **ESLint**: Code quality enforcement
- **Prettier**: Consistent code formatting

## 🔄 Code Generation

### Adding New Components
```bash
# Generate React component
nx g @nx/react:component MyComponent --project=marketplace-front

# Generate NestJS module
nx g @nx/nest:module my-feature --project=marketplace-server

# Add shadcn/ui component
nx shadcn:component:add marketplace-ui --args.component=card
```

### Database Migrations
```bash
# Generate migration from entity changes
nx migration:generate marketplace-server --args.name=AddUserTable

# Create empty migration
nx migration:create marketplace-server --args.name=CustomMigration

# Run migrations
nx migration:deploy marketplace-server
```

## 📝 Customization Guide

### Adding New Packages
1. Create package directory: `packages/your-package/`
2. Add `project.json` with appropriate configuration
3. Update `tsconfig.json` references
4. Configure module boundaries in `eslint.config.mjs`

### Technology Replacements
- **Database**: Replace TypeORM configuration in `packages/marketplace-server/src/database/`
- **Styling**: Modify Tailwind configuration in `tailwind.preset.js`
- **UI Components**: Customize shadcn/ui in `packages/marketplace-ui/components.json`
- **Build Tool**: Update Vite configuration for frontend changes

## 🚀 Production Considerations

### Security
- Environment variable validation
- CORS configuration
- Authentication middleware ready
- Database connection security

### Scalability  
- Horizontal scaling with Redis
- Database connection pooling
- CDN-ready static assets
- Microservice architecture support

### Monitoring
- Health check endpoints
- Structured logging
- Error tracking ready
- Performance monitoring hooks

## 📚 Additional Resources

- [Nx Documentation](https://nx.dev)
- [NestJS Documentation](https://docs.nestjs.com)
- [React Documentation](https://react.dev)
- [TypeORM Documentation](https://typeorm.io)
- [shadcn/ui Documentation](https://ui.shadcn.com)
- [Tailwind CSS Documentation](https://tailwindcss.com)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Make your changes and test thoroughly
4. Commit your changes: `git commit -m 'Add amazing feature'`
5. Push to the branch: `git push origin feature/amazing-feature`
6. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
