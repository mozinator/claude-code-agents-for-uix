# UIx Agents for opencode.ai

Specialized agents for comprehensive UIx ClojureScript development with React 19 support.

## Available Agents

### Core Architecture

- **uix-component-architect** - Specialized in component design patterns, `defui` macro architecture, and `$` macro HyperScript opti...
- **uix-lifecycle-coordinator** - Specialized in React effect hooks, lifecycle management patterns, and side effect coordination with ...
- **uix-setup-specialist** - Specialized in project initialization, toolchain setup with Shadow-cljs/Leiningen, and development e...
- **uix-state-manager** - Specialized in React hooks state management, local/global state patterns, and reactive data flow des...

### Development & Integration

- **uix-async-handler** - Specialized in modern async patterns, HTTP requests, WebSockets, and asynchronous operation manageme...
- **uix-interop-specialist** - Specialized in modern JavaScript interop, npm library integration, and third-party React component u...
- **uix-react-integrator** - Specialized in modern React features integration, advanced hooks usage, and seamless interoperabilit...
- **uix-routing-navigator** - Specialized in modern React Router patterns, client-side navigation, and routing architectures for U...

### UI & Styling

- **uix-animation-coordinator** - Specialized in modern animation libraries integration, transition management, and performance-optimi...
- **uix-forms-expert** - Specialized in modern form handling, validation patterns, input components, and user interaction man...
- **uix-migration-specialist** - Specialized in migrating existing Reagent applications to UIx, providing comprehensive guidance, aut...
- **uix-ui-designer** - Specialized in modern UI component libraries integration, contemporary styling approaches (CSS-in-JS...

## Usage

These agents work together to provide comprehensive UIx development support, from initial project setup through advanced React integration and performance optimization.

### Getting Started
1. Use `uix-setup-specialist` for new project initialization with Shadow-cljs or Leiningen
2. Employ `uix-component-architect` and `uix-state-manager` for core UI architecture and modern state management
3. Leverage `uix-lifecycle-coordinator` and `uix-react-integrator` for React hooks and modern features
4. Apply modern testing and debugging patterns for quality assurance

### UI & Interaction Development
- Use `uix-ui-designer` for modern component library integration and contemporary styling approaches
- Apply `uix-forms-expert` for modern form handling with React Hook Form and validation
- Employ `uix-animation-coordinator` for smooth animations with Framer Motion and React Spring

### Advanced Development
- Use `uix-routing-navigator` for modern React Router v6+ navigation patterns
- Apply `uix-async-handler` for modern HTTP requests with fetch API and async patterns
- Employ `uix-interop-specialist` for modern JavaScript interop and ES6+ module integration
- Leverage `uix-migration-specialist` for comprehensive migration assistance from Reagent to UIx

## Agent Architecture

Each agent follows opencode.ai sub-agent conventions:
- **Focused Expertise**: Single-responsibility specialization for specific UIx development areas
- **Comprehensive Background**: Deep UIx and modern React knowledge with ClojureScript context
- **Practical Implementation**: Real-world patterns, best practices, and contemporary tooling integration
- **Quality Focus**: Modern testing strategies, performance optimization, and debugging techniques

## React 19 Support

All agents are designed to work with **React 19** and its latest features:
- React Compiler for automatic optimization
- Improved concurrent rendering
- Modern hook patterns and best practices
- Enhanced developer experience

## Installation

1. Copy the agent files from `.claude/agents/` to your project's `.opencode/agent/` directory
2. Run the conversion script: `node scripts/convert-claude-agents.js`
3. The script will generate opencode.ai compatible agents and this AGENTS.md file

## Migration from Reagent

This collection has been comprehensively refactored from Reagent to UIx. UIx provides:

- **Modern React Patterns**: Full support for React 19 concurrent features, hooks, and contemporary patterns
- **Enhanced Developer Experience**: Better hot reloading, improved debugging, and modern tooling integration
- **Performance Optimizations**: Smaller bundle size, improved tree shaking, and optimized rendering
- **Future-Proof Architecture**: Built for modern React ecosystem and contemporary web development

### Key Differences from Reagent
- `defui` instead of Form-1/2/3 components
- `$` macro HyperScript syntax instead of Hiccup vectors
- React hooks (`use-state`, `use-effect`) instead of ratoms
- Modern async patterns with fetch API
- Contemporary styling with CSS-in-JS libraries
- Modern form handling with React Hook Form
- Advanced animations with Framer Motion

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

*Generated by convert-claude-agents.js on 2025-09-01T13:17:06.247Z*
