---
description: Guide for selecting the appropriate agent for different tasks
tools:
  read: true
temperature: 0.1
mode: subagent
---

# Agent Selection Guide

## Quick Reference Matrix

| Task Type | Recommended Agent | Alternative Agents | Key Capabilities |
|-----------|-------------------|-------------------|------------------|
| **Web Development** | | | |
| Full-stack application | `build` + `fullstack-development` skill | `platform-maintainer` for setup | End-to-end implementation with stack guidance |
| Backend API only | `build` + `backend-architecture` skill | `api-reviewer` | API design and service boundaries |
| Frontend UI only | `build` + `frontend-development` skill | `frontend-qa` | UI implementation and QA |
| **Mobile Development** | | | |
| React Native app | `build` + `mobile-development` skill | `frontend-qa` | Cross-platform mobile implementation |
| **Data & Analytics** | | | |
| ML model development | `ml-engineer` | `data-engineer` | TensorFlow, PyTorch |
| Data pipelines | `data-engineer` | `ml-engineer` | ETL, streaming |
| **Infrastructure** | | | |
| Cloud setup | `build` + `devops-deployment` skill | `platform-maintainer` | Cloud and deployment workflows |
| Container orchestration | `build` + `devops-deployment` skill | `platform-maintainer` | Containers and operational automation |
| **Security** | | | |
| Code security review | `security-auditor` | `api-security-audit` | OWASP, vulnerability scan |
| API security | `api-security-audit` | `security-auditor` | JWT, OAuth, CORS |
| **Quality Assurance** | | | |
| Automated testing | `qa-automation` | `code-reviewer` | Test frameworks |
| Code review | `code-reviewer` | `architect-reviewer` | Best practices |
| **Specialized Domains** | | | |
| Game development | `game-developer` | N/A | Unity, Unreal |
| Blockchain/dApps | `blockchain-developer` | N/A | Solidity, Web3 |
| Medical devices | `medical-expert-advisor` | N/A | SaMD, regulations |
| Financial audit | `audit-domain-expert` | N/A | Compliance, risk |

## Decision Tree

### 1. Start with the Domain
```
What type of project are you working on?
├── Web/Mobile Application
│   ├── Full stack? → build + fullstack-development skill
│   ├── Backend only? → build + backend-architecture skill
│   └── Frontend only? → build + frontend-development skill
├── Data/Machine Learning
│   ├── ML models? → ml-engineer
│   └── Data pipelines? → data-engineer
├── Infrastructure/DevOps
│   └── → build + devops-deployment skill
├── Security
│   ├── API security? → api-security-audit
│   └── General security? → security-auditor
├── Gaming
│   └── → game-developer
└── Specialized Domain
    ├── Medical → medical-expert-advisor
    ├── Financial → audit-domain-expert
    └── Blockchain → blockchain-developer
```

### 2. Consider Complexity and Scale
```
Project scale:
├── Small (1-2 weeks, single developer)
│   └── Use generalist agents (fullstack-engineer, general-purpose)
├── Medium (1-3 months, small team)
│   └── Mix of specialist and generalist agents
└── Large (3+ months, multiple teams)
    └── Multiple specialist agents with coordination
```

### 3. Identify Required Capabilities
```
Key capabilities needed:
├── Database design → database-architect
├── API design → backend-architect
├── UI/UX design → ui-ux-designer
├── Testing → qa-automation
├── Documentation → documentation-expert
├── Performance → monitoring-specialist
└── Security → security-auditor
```

## Common Agent Combinations

### 1. Typical Web Application Team
```yaml
core_team:
  - backend-architect: "Lead API and database design"
  - frontend-developer: "Build user interface"
  - qa-automation: "Set up testing framework"

support_team:
  - security-auditor: "Security review"
  - documentation-expert: "API docs"
  - deployment-engineer: "CI/CD setup"
```

### 2. Data Science Project
```yaml
core_team:
  - data-engineer: "Build data pipelines"
  - ml-engineer: "Develop models"
  - fullstack-engineer: "Create dashboard"

support_team:
  - monitoring-specialist: "Model performance"
  - security-auditor: "Data privacy"
```

### 3. Startup MVP
```yaml
solo_team:
  - fullstack-engineer: "Handle everything"

optional_support:
  - code-reviewer: "Code quality"
  - security-auditor: "Quick security check"
```

### 4. Enterprise Application
```yaml
leadership:
  - backend-architect: "System architecture"
  - database-architect: "Data architecture"

development_teams:
  frontend:
    - frontend-developer: "UI components"
  backend:
    - backend-architect: "Microservices"
  data:
    - data-engineer: "Analytics"

quality_assurance:
  - qa-automation: "Test automation"
  - security-auditor: "Security testing"
  - architect-reviewer: "Architecture compliance"
```

## Agent Selection Heuristics

### Use Generalist Agents When:
- Project is small and simple
- Timeline is tight
- Single developer can handle all aspects
- Budget constraints

### Use Specialist Agents When:
- Domain expertise is critical
- Complex technical challenges
- High quality/security requirements
- Multiple team members

### Use Multiple Agents When:
- Tasks can be parallelized
- Different skill sets required
- Large codebase
- Need for multiple perspectives

## Coordination Patterns

### 1. Sequential Handoff
```
ui-ux-designer → frontend-developer → qa-automation
```

### 2. Parallel Development
```
backend-architect ←→ frontend-developer
      ↓                    ↓
   api-security-audit   code-reviewer
```

### 3. Hub and Spoke
```
           fullstack-engineer
           /       |       \
          /        |        \
    backend   frontend  database
   architect  developer   architect
```

## Best Practices

1. **Start Small**: Begin with 1-2 agents, add more as needed
2. **Clear Boundaries**: Define clear responsibilities for each agent
3. **Regular Sync**: Use project-supervisor-orchestrator for coordination
4. **Quality Gates**: Always include code-reviewer or security-auditor
5. **Documentation**: Use documentation-expert for complex projects

## Avoid These Mistakes

1. **Too Many Agents**: Don't use specialists for simple tasks
2. **Unclear Roles**: Avoid overlapping responsibilities
3. **No Coordination**: Don't let agents work in isolation
4. **Skipping Reviews**: Always include quality assurance
5. **Ignoring Dependencies**: Plan agent handoffs carefully

## Cost Optimization

| Agent Tier | Cost Efficiency | Best For |
|------------|-----------------|----------|
| Generalist | High | Simple projects, MVPs |
| Specialist | Medium | Complex features, compliance |
 Multiple Specialists | Low (per agent) | Large enterprise projects |

## Example Scenarios

### Scenario 1: Building a SaaS Application
```
Initial phase:
- fullstack-engineer (MVP development)
- security-auditor (security review)

Growth phase:
- backend-architect (scale APIs)
- database-architect (optimize queries)
- monitoring-specialist (add observability)
```

### Scenario 2: ML-Powered Feature
```
Team:
- ml-engineer (model development)
- data-engineer (data pipeline)
- backend-architect (API integration)
- qa-automation (ML testing)
```

This guide helps optimize agent selection for maximum efficiency and quality in your projects.
