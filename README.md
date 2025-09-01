# UIx Code Agents

11 specialized agents for comprehensive UIx ClojureScript development with React 19 support.

**[UIx](https://uix-cljs.dev/)** is an idiomatic ClojureScript interface to modern React that enables building efficient, reactive user interfaces with functional programming principles and seamless JavaScript interop. UIx embraces modern React patterns including hooks, concurrent rendering, and contemporary development practices.

> **Note**: This documentation assumes you are using **React 19** and its latest features including the React Compiler, improved concurrent rendering, and modern hook patterns.

## Available Agents

### Core Architecture Agents
- **[uix-setup-specialist](/.claude/agents/uix-setup-specialist.md)** - Project initialization, toolchain setup with Shadow-cljs/Leiningen, and development environment configuration for UIx
- **[uix-component-architect](/.claude/agents/uix-component-architect.md)** - Component design patterns with `defui`, `$` macro HyperScript syntax, and React hooks integration
- **[uix-state-manager](/.claude/agents/uix-state-manager.md)** - React hooks state management, local/global state patterns, and modern data flow design
- **[uix-lifecycle-coordinator](/.claude/agents/uix-lifecycle-coordinator.md)** - React effect hooks (`useEffect`, `useLayoutEffect`), lifecycle management, and side effect coordination

### Development & Integration Agents
- **[uix-react-integrator](/.claude/agents/uix-react-integrator.md)** - Modern React 19 features integration, advanced hooks usage, and React ecosystem interoperability
- **[uix-routing-navigator](/.claude/agents/uix-routing-navigator.md)** - React Router v6+ patterns, client-side navigation, and modern routing architectures
- **[uix-async-handler](/.claude/agents/uix-async-handler.md)** - Modern async patterns, HTTP requests with fetch API, WebSockets, and asynchronous operation management
- **[uix-interop-specialist](/.claude/agents/uix-interop-specialist.md)** - Modern JavaScript interop, ES6+ modules, npm library integration, and third-party React component usage

### UI & Styling Agents
- **[uix-ui-designer](/.claude/agents/uix-ui-designer.md)** - Modern UI component libraries integration, contemporary styling approaches (CSS-in-JS, styled-components), and design systems
- **[uix-forms-expert](/.claude/agents/uix-forms-expert.md)** - Modern form handling with React Hook Form, validation patterns, input components, and user interaction management
- **[uix-animation-coordinator](/.claude/agents/uix-animation-coordinator.md)** - Modern animation libraries integration (Framer Motion, React Spring), transition management, and performance-optimized animations

### Migration & Quality Agents
- **[uix-migration-specialist](/.claude/agents/uix-migration-specialist.md)** - Comprehensive migration assistance from Reagent to UIx, automated transformation tools, and migration best practices

## Platform Setup

### Claude Code Setup

**Installation**:
1. Copy agent files from `/.claude/agents/` to your project's `/.claude/agents/` directory
2. Append contents from `CLAUDE.md` to your project's `CLAUDE.md` file

**Usage**:
- Use `@agent-name` syntax to invoke specific agents (e.g., `@uix-setup-specialist`)
- Agents work directly in Claude Code's conversation interface

### opencode.ai Setup

**Installation**:
1. Copy agent files from `/.opencode/agent/` to your project's `/.opencode/agent/` directory
2. The `AGENTS.md` file provides automatic agent discovery

**Usage**:
- Agents are automatically discovered from `.opencode/agent/` directory
- Use agent names directly in opencode.ai interface
- All agents include proper opencode.ai frontmatter with tool configurations

## Batch Installation

### One-liner Setup

```bash
# Setup for Claude Code (appends to existing CLAUDE.md)
mkdir -p .claude/agents && \
for agent in uix-animation-coordinator uix-async-handler uix-component-architect \
             uix-forms-expert uix-interop-specialist uix-lifecycle-coordinator \
             uix-migration-specialist uix-react-integrator uix-routing-navigator \
             uix-setup-specialist uix-state-manager uix-ui-designer; do \
  curl -s https://raw.githubusercontent.com/mozinator/agents/main/code-agents-for-uix/.claude/agents/${agent}.md \
       -o .claude/agents/${agent}.md; \
done && \
curl -s https://raw.githubusercontent.com/mozinator/agents/main/code-agents-for-uix/CLAUDE.md \
     >> CLAUDE.md && \

# Setup for opencode.ai (appends to existing AGENTS.md)
mkdir -p .opencode/agent && \
for agent in uix-animation-coordinator uix-async-handler uix-component-architect \
             uix-forms-expert uix-interop-specialist uix-lifecycle-coordinator \
             uix-migration-specialist uix-react-integrator uix-routing-navigator \
             uix-setup-specialist uix-state-manager uix-ui-designer; do \
  curl -s https://raw.githubusercontent.com/mozinator/agents/main/code-agents-for-uix/.opencode/agent/${agent}.md \
       -o .opencode/agent/${agent}.md; \
done && \
curl -s https://raw.githubusercontent.com/mozinator/agents/main/code-agents-for-uix/AGENTS.md \
     >> AGENTS.md
```

## Agent Architecture

Each agent follows both Claude Code and opencode.ai conventions:
- **Focused Expertise**: Single-responsibility specialization for specific UIx development areas
- **Comprehensive Background**: Deep UIx and modern React knowledge with ClojureScript context
- **Practical Implementation**: Real-world patterns, best practices, and contemporary tooling integration
- **Quality Focus**: Modern testing strategies, performance optimization, and debugging techniques

## Contributing

This is a comprehensive agent collection covering all aspects of UIx development. The agents are designed to work independently or in combination to support complex ClojureScript web application development workflows with modern React integration patterns.

## Resources

- [UIx Documentation](https://uix-cljs.dev/)
- [ClojureScript](https://clojurescript.org/)
- [React Documentation](https://react.dev/)
- [Shadow CLJS](https://shadow-cljs.github.io/docs/UsersGuide.html)
- [Learn ClojureScript](https://www.learn-clojurescript.com/)
- [Modern React Patterns](https://react.dev/learn)
- [React Router](https://reactrouter.com/)
- [Framer Motion](https://www.framer.com/motion/)

---

*Compatible with both Claude Code and opencode.ai platforms*
