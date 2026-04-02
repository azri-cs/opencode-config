---
description: Protocols and patterns for effective multi-agent coordination
tools:
  read: true
temperature: 0.1
mode: subagent
---

# Agent Coordination Protocols

## Overview

This document defines protocols and patterns for effective agent coordination in Claude Code's multi-agent ecosystem.

## 1. Agent Handoff Protocol

### Standard Handoff Sequence
```yaml
handoff_sequence:
  1. prepare_handoff:
    - Summarize current state
    - Document progress
    - Identify next requirements
  2. initiate_handoff:
    - Clear context transfer
    - Agent-specific notes
    - Required tools/data
  3. acknowledge_handoff:
    - Confirm receipt
    - Validate understanding
    - Identify gaps
```

### Handoff Message Template
```json
{
  "handoff_type": "sequential|parallel|delegation",
  "source_agent": "agent_name",
  "target_agent": "agent_name",
  "context_summary": "brief description of work done",
  "current_state": {
    "completed": ["task1", "task2"],
    "in_progress": "task3",
    "blocked": []
  },
  "deliverables": {
    "artifacts": ["path/to/file1", "path/to/file2"],
    "data": {"key": "value"},
    "environment": "dev|staging|prod"
  },
  "next_steps": ["specific next tasks"],
  "dependencies": ["external requirements"],
  "notes": "additional context"
}
```

## 2. Parallel Agent Execution

### Parallel Execution Patterns

#### 1. Independent Tasks
```yaml
pattern: "split_and_conquer"
description: "Divide work into independent parallel streams"
example:
  task: "Build full-stack application"
  parallel_agents:
    - build: "Implement code in the current layer"
    - api-reviewer: "Review contracts and integration safety"
    - frontend-qa: "Review UI, accessibility, and responsiveness"
  merge_point: "Integration testing"
```

#### 2. Staged Pipeline
```yaml
pattern: "pipeline"
description: "Sequential handoffs with validation gates"
example:
  stages:
    - agent: "build"
      output: "Initial implementation"
      validation: "Code review and targeted tests"
    - agent: "api-reviewer"
      output: "API contract review"
      validation: "Consumer safety check"
    - agent: "frontend-qa"
      output: "UI quality review"
      validation: "Accessibility and responsiveness check"
```

#### 3. Collaborative Development
```yaml
pattern: "hub_and_spoke"
description: "Central coordinator with specialist contributors"
example:
  hub: "build"
  spokes:
    - ui-ux-designer: "Design system"
    - security-auditor: "Security review"
    - api-security-audit: "API security"
    - qa-automation: "Test automation"
```

## 3. Shared Context Management

### Context Types
```yaml
context_categories:
  project_context:
    - Project goals and requirements
    - Architecture decisions
    - Coding standards
    - Environment details

  session_context:
    - Current working directory
    - Active branches/commits
    - Running services
    - Recent changes

  agent_context:
    - Agent-specific state
    - Local variables
    - Tool connections
    - Progress tracking
```

### Context Sharing Patterns
```json
{
  "global_context": {
    "project_root": "/path/to/project",
    "git_branch": "feature/new-auth",
    "environment": "development"
  },
  "agent_contexts": {
    "backend-architect": {
      "api_version": "v2",
      "auth_method": "JWT"
    },
    "frontend-developer": {
      "ui_framework": "React",
      "state_management": "Redux"
    }
  }
}
```

## 4. Agent Communication Channels

### Direct Communication
```yaml
methods:
  message_passing:
    - Direct agent-to-agent messages
    - Structured data exchange
    - Acknowledgment required

  shared_memory:
    - File-based communication
    - Database tables
    - Environment variables
```

### Indirect Communication
```yaml
methods:
  artifact_based:
    - Shared files
    - Code repositories
    - Documentation

  event_driven:
    - Git hooks
    - CI/CD triggers
    - Webhook notifications
```

## 5. Conflict Resolution

### Conflict Types
1. **Resource Conflicts**: Multiple agents needing same resource
2. **Approach Conflicts**: Different implementation approaches
3. **Priority Conflicts**: Divergent task priorities
4. **Dependency Conflicts**: Circular dependencies

### Resolution Strategies
```yaml
resolution_protocols:
  resource_conflicts:
    strategy: "time_sharing|preemption|duplication"
    arbiter: "project-supervisor-orchestrator"

  approach_conflicts:
    strategy: "consensus|expertise|experimentation"
    documentation: "decision_log.md"

  priority_conflicts:
    strategy: "product_strategist|business_analyst"
    criteria: "business_value|dependencies|risk"

  dependency_conflicts:
    strategy: "refactor|delegation|deferral"
    visualizer: "dependency_graph"
```

## 6. Performance Optimization

### Agent Pool Management
```yaml
pool_strategies:
  lazy_initialization:
    description: "Initialize agents on demand"
    benefits: ["memory_efficiency", "faster_startup"]

  persistent_agents:
    description: "Keep frequently used agents active"
    benefits: ["reduced_latency", "state_preservation"]

  agent_recycling:
    description: "Reuse agent instances after cleanup"
    benefits: ["resource_efficiency", "faster_warmup"]
```

### Task Distribution
```yaml
load_balancing:
  strategies:
    round_robin:
      description: "Distribute tasks evenly"
      use_case: "Similar complexity tasks"

    capability_based:
      description: "Assign based on agent expertise"
      use_case: "Specialized requirements"

    workload_aware:
      description: "Consider current agent load"
      use_case: "Mixed complexity tasks"
```

## 7. Error Handling and Recovery

### Error Propagation
```yaml
error_handling:
  local_handling:
    - Agent handles own errors
    - Retry mechanisms
    - Fallback strategies

  escalation:
    - Handoff to specialized agent
    - Human intervention trigger
    - Rollback procedures
```

### Recovery Patterns
```json
{
  "recovery_strategies": {
    "checkpoint": {
      "description": "Save progress at key points",
      "implementation": "state_snapshots"
    },
    "rollback": {
      "description": "Revert to last known good state",
      "implementation": "version_control_revert"
    },
    "degrade": {
      "description": "Continue with reduced functionality",
      "implementation": "feature_flags"
    }
  }
}
```

## 8. Best Practices

### Do's
1. **Clear handoff documentation**: Always document state and next steps
2. **Structured communication**: Use defined message formats
3. **Regular synchronization**: Periodic status updates
4. **Conflict prevention**: Early identification of potential issues
5. **Resource cleanup**: Clean up after task completion

### Don'ts
1. **Assume context**: Never assume shared understanding
2. **Silent failures**: Always communicate errors and blockers
3. **Resource hoarding**: Release resources when done
4. **Skipping validation**: Always validate handoffs
5. **Parallel conflicts**: Avoid modifying same resources simultaneously

## 9. Implementation Guidelines

### When to Use Multi-Agent Coordination
```yaml
good_candidates:
  - Large, complex tasks
  - Multiple domains of expertise
  - Parallelizable work
  - Sequential dependencies
  - Quality requirements (multiple reviews)

poor_candidates:
  - Simple, quick tasks
  - Single domain problems
  - Tight integration requirements
  - Resource constrained environments
```

### Agent Selection Guidelines
1. **Match capabilities to requirements**
2. **Consider agent availability and load**
3. **Plan for coordination overhead**
4. **Have backup agents ready**
5. **Define clear success criteria**

## 10. Monitoring and Metrics

### Key Metrics
```yaml
coordination_metrics:
  handoff_efficiency:
    - Time between handoffs
    - Success rate of handoffs
    - Context loss percentage

  agent_utilization:
    - Active time vs idle time
    - Task completion rate
    - Error rate per agent

  overall_performance:
    - Total task completion time
    - Quality metrics
    - Resource usage
```

This coordination framework ensures effective collaboration between agents while maintaining clarity, efficiency, and reliability in multi-agent workflows.
