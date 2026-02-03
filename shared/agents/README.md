# Agents

This directory contains configurations and instructions for specialized AI agents.

## What are Agents?

Agents are AI assistants configured with specific roles, knowledge, and behaviors for particular tasks. They provide:

- Specialized expertise
- Consistent behavior
- Reusable configurations
- Task-specific instructions

## Types of Agents

### Code Agents
- **Backend Developer**: API and server-side logic
- **Frontend Developer**: UI/UX and client-side code
- **Full-Stack Developer**: End-to-end application development
- **DevOps Engineer**: Infrastructure and deployment

### Review Agents
- **Code Reviewer**: Quality and best practices
- **Security Auditor**: Security vulnerabilities
- **Performance Analyst**: Optimization opportunities

### Documentation Agents
- **Technical Writer**: User documentation
- **API Documenter**: API reference documentation
- **Tutorial Creator**: Learning materials

### Testing Agents
- **Test Engineer**: Test creation and strategy
- **QA Specialist**: Quality assurance and testing

## Agent Configuration

Each agent configuration includes:

1. **Role Definition**: What the agent does
2. **Expertise**: Areas of knowledge
3. **Instructions**: How the agent should behave
4. **Examples**: Sample interactions
5. **Limitations**: What the agent shouldn't do

## Using Agents

### In Cursor

Create a `.cursorrules` file with agent instructions:

```
You are a [AGENT ROLE].

Expertise:
- [AREA 1]
- [AREA 2]

Instructions:
- [BEHAVIOR 1]
- [BEHAVIOR 2]
```

### In VS Code

Configure in your workspace or user settings with specific instructions for different file types.

### With Claude

Start your conversation by establishing the agent role:

```
I need you to act as a [AGENT ROLE].
[PASTE AGENT INSTRUCTIONS]
```

## Contributing

Share your agent configurations! Include:
- Agent role and purpose
- Configuration instructions
- Example usage
- Expected behavior
