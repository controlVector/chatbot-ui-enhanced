# Chatbot UI Enhanced - ControlVector Frontend

## Purpose & Integration
- **Primary Role**: Enhanced Next.js frontend for the ControlVector platform
- **Integration Strategy**: Preserve existing chatbot-ui functionality while adding infrastructure management capabilities
- **Key Enhancements**: 
  - Infrastructure management panels in sidebar
  - Real-time agent status and task monitoring
  - Natural language infrastructure operations through chat
  - Multi-agent interaction visualization

## Technical Stack
- **Framework**: Next.js 14 with App Router (preserved from original)
- **Runtime**: React 18 with TypeScript (existing stack maintained)
- **Database**: PostgreSQL via Supabase (existing integration enhanced)
- **UI Framework**: Tailwind CSS with shadcn/ui components (preserved)
- **Real-time**: Socket.io client for infrastructure updates

## Enhanced Features

### New Sidebar Content Types
Building on the existing 8 content types, we add infrastructure-focused panels:
- **Infrastructure**: Resource management and provisioning status
- **Deployments**: Application deployment monitoring and history  
- **Agents**: Multi-agent status, tasks, and performance monitoring
- **Monitoring**: System health, alerts, and performance dashboards

### Enhanced Context Integration
Extending the existing `ChatbotUIContext` with infrastructure state:
```typescript
interface EnhancedChatbotUIContext extends ChatbotUIContext {
  // Infrastructure Management
  infrastructure: Tables<"infrastructure">[]
  setInfrastructure: Dispatch<SetStateAction<Tables<"infrastructure">[]>>
  
  deployments: Tables<"deployments">[]
  setDeployments: Dispatch<SetStateAction<Tables<"deployments">[]>>
  
  agents: Tables<"agents">[]
  setAgents: Dispatch<SetStateAction<Tables<"agents">[]>>
  
  // Agent Interaction
  selectedAgent: Tables<"agents"> | null
  setSelectedAgent: Dispatch<SetStateAction<Tables<"agents"> | null>>
  agentTasks: AgentTask[]
  setAgentTasks: Dispatch<SetStateAction<AgentTask[]>>
  
  // Real-time Infrastructure State
  isInfrastructureLoading: boolean
  setIsInfrastructureLoading: Dispatch<SetStateAction<boolean>>
  infrastructureError: string | null
  setInfrastructureError: Dispatch<SetStateAction<string | null>>
}
```

## Integration Points

### API Integration (via cv-api-gateway:3000)
- **Orchestrator API**: `/api/v1/orchestrator/*` - Watson agent coordination
- **Infrastructure API**: `/api/v1/infrastructure/*` - Atlas resource management
- **Deployment API**: `/api/v1/deployments/*` - Phoenix deployment operations
- **Monitoring API**: `/api/v1/monitoring/*` - Sentinel system metrics
- **Security API**: `/api/v1/security/*` - Sherlock security status
- **Agent API**: `/api/v1/agents/*` - Multi-agent management

### Real-time Updates
- **WebSocket Connection**: Real-time updates from notification service
- **Infrastructure Events**: Live resource provisioning status
- **Deployment Progress**: Real-time deployment and rollback status
- **Agent Communication**: Live agent task execution and results

### Enhanced Chat Integration
- **Natural Language Infrastructure**: Infrastructure commands through existing chat interface
- **Agent Responses**: Specialized agent personalities in chat responses
- **Context-Aware Suggestions**: Infrastructure-aware autocomplete and suggestions
- **File Integration**: Infrastructure configuration file management

## Development Setup

### Prerequisites (Enhanced from Original)
- Node.js 18+ (preserved requirement)
- PostgreSQL via Supabase (existing setup)
- Access to ControlVector API Gateway (new requirement)
- Redis for real-time features (new requirement)

### Environment Configuration (Extended)
```env
# Existing chatbot-ui configuration preserved
DATABASE_URL=postgresql://...existing...
SUPABASE_URL=...existing...
SUPABASE_ANON_KEY=...existing...

# New ControlVector Integration
NEXT_PUBLIC_CV_API_URL=http://localhost:3000
NEXT_PUBLIC_CV_WS_URL=ws://localhost:3000
NEXT_PUBLIC_CV_ENABLE_INFRASTRUCTURE=true

# Agent Integration
NEXT_PUBLIC_WATSON_ENABLED=true
NEXT_PUBLIC_ATLAS_ENABLED=true
NEXT_PUBLIC_PHOENIX_ENABLED=true
NEXT_PUBLIC_SHERLOCK_ENABLED=true
NEXT_PUBLIC_SENTINEL_ENABLED=true
NEXT_PUBLIC_SHELL_ENABLED=true
NEXT_PUBLIC_NAVIGATOR_ENABLED=true
NEXT_PUBLIC_ARCHITECT_ENABLED=true

# Real-time Configuration
NEXT_PUBLIC_WS_RECONNECT_ATTEMPTS=5
NEXT_PUBLIC_WS_RECONNECT_DELAY=1000
NEXT_PUBLIC_ENABLE_REAL_TIME_UPDATES=true
```

### Local Development Commands (Enhanced)
```bash
# Existing commands preserved
npm install
npm run dev

# New infrastructure development commands
npm run dev:infrastructure  # Start with infrastructure features enabled
npm run test:infrastructure # Test infrastructure integration
npm run build:enhanced     # Build with all ControlVector features
```

## Architecture Preservation Strategy

### Existing Structure Maintained
- **App Router**: All existing routes preserved (`/[locale]/[workspaceid]/...`)
- **Component Architecture**: Existing component hierarchy maintained
- **Database Schema**: Original Supabase schema preserved and extended
- **Authentication**: Existing Supabase Auth integration maintained

### Enhancement Integration Points
1. **Sidebar Extension**: `components/sidebar/sidebar-switcher.tsx`
   - Add infrastructure content types to existing switcher
   - Preserve existing 8 content types (chats, presets, prompts, etc.)

2. **Context Enhancement**: `context/context.tsx`
   - Extend existing ChatbotUIContext with infrastructure state
   - Maintain all existing context properties and methods

3. **Layout Integration**: `app/[locale]/[workspaceid]/layout.tsx`
   - Integrate infrastructure data fetching with existing workspace data
   - Maintain existing workspace-centric architecture

### Database Schema Extensions
Building on existing Supabase tables, we add:
```sql
-- New infrastructure tables (extend existing schema)
CREATE TABLE infrastructure (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    workspace_id UUID REFERENCES workspaces(id),
    -- ... infrastructure fields
);

CREATE TABLE deployments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    workspace_id UUID REFERENCES workspaces(id),
    -- ... deployment fields
);

CREATE TABLE agents (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    workspace_id UUID REFERENCES workspaces(id),
    -- ... agent fields
);
```

## Enhanced User Experience

### Infrastructure Chat Integration
- **Natural Language**: "Deploy my React app to DigitalOcean"
- **Agent Responses**: Watson coordinates with Atlas and Phoenix for deployment
- **Visual Feedback**: Real-time deployment progress in chat interface
- **Error Handling**: Clear error messages with suggested solutions

### Sidebar Enhancements
- **Infrastructure Panel**: Resource status, cost tracking, performance metrics
- **Deployment Panel**: Deployment history, rollback options, environment management
- **Agent Panel**: Agent status, task queues, performance analytics
- **Monitoring Panel**: System health, alerts, SLA tracking

### Real-time Features
- **Live Updates**: Infrastructure changes reflected immediately in UI
- **Progress Tracking**: Visual progress indicators for long-running operations
- **Notifications**: Toast notifications for important infrastructure events
- **Status Indicators**: Real-time agent and service status indicators

## Migration Strategy

### Phase 1: Core Integration
1. Extend context with basic infrastructure state
2. Add infrastructure sidebar panels
3. Integrate with API gateway for basic operations

### Phase 2: Agent Integration
1. Implement multi-agent chat responses
2. Add agent status monitoring
3. Integrate real-time agent communication

### Phase 3: Advanced Features
1. Implement natural language infrastructure operations
2. Add comprehensive monitoring dashboards
3. Integrate advanced deployment features

## Testing Strategy

### Preserved Testing
- All existing chatbot-ui tests maintained
- Existing user workflows preserved
- Backward compatibility ensured

### Enhanced Testing
- Infrastructure integration tests
- Agent communication tests
- Real-time feature tests
- Performance tests with enhanced features

## Deployment Considerations

### Existing Deployment Preserved
- Existing deployment process maintained
- Environment variable compatibility preserved
- Existing CI/CD pipelines enhanced, not replaced

### Enhanced Deployment
- Additional environment variables for ControlVector integration
- WebSocket proxy configuration for real-time features
- CDN configuration for infrastructure assets

This enhanced frontend preserves the excellent user experience and proven architecture of the original chatbot-ui while seamlessly integrating comprehensive infrastructure management capabilities through the ControlVector agent ecosystem.