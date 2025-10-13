# 🧭 Mintlify — Complete Guide for Developers & Teams

This document gives you a **comprehensive yet practical overview** of how to use **Mintlify**, tailored for developer workflows (including **Cursor**, VS Code, and Git‑based environments).

---

## 📖 What is Mintlify

**Mintlify** is a modern “docs‑as‑code” platform for writing, publishing, and maintaining beautiful developer documentation.

### 🔑 Core Features

- **Docs‑as‑code**: your docs live in GitHub/GitLab, version‑controlled with your code.
- **Visual + Markdown/MDX editing**.
- **AI‑powered writing assistant** for automatic summaries, grammar, and translation.
- **Automatic API reference generation** from OpenAPI specs.
- **Beautiful default theme + full customization** through `docs.json`.
- **Collaboration + branching**: edit on feature branches, preview, merge to publish.
- **Custom domain support, analytics, and SEO tools.**
- **Integration‑ready** with MCP servers, SDKs, and generation APIs.

Website: [https://mintlify.com](https://mintlify.com)

Docs: [https://mintlify.com/docs](https://mintlify.com/docs)

---

## ⚙️ Setup & Quickstart

### 1. Create Account & Project

Go to [Mintlify Dashboard](https://mintlify.com/app) → **New Project** → Choose between:
- **Start from scratch**
- **Import from GitHub**

### 2. Connect Repository

Authorize the Mintlify GitHub App → select your repository → it syncs automatically.

> ✅ Mintlify will automatically create a deployment branch (e.g. `mintlify/main` or `docs`) and serve your site from it.

### 3. Local Development

Clone your repo and install dependencies:

```bash
git clone https://github.com/your-org/your-docs.git
cd your-docs
npm install  # or pnpm install / yarn install
```

Run the local dev server:

```bash
npm run dev
```

Default port → http://localhost:3000

### 4. Deploy & Publish

- Push to your deploy branch to publish automatically.
- You can preview non‑deploy branches before merging.

### 5. Configure Site

The main configuration lives in **`docs.json`**.

Example:

```json
{
  "name": "Autoppia Docs",
  "colors": { "primary": "#00C2FF" },
  "navigation": {
    "tabs": [
      {
        "tab": "Introduction",
        "icon": "home",
        "groups": [
          { "group": "Introduction", "pages": ["index", "vision"] }
        ]
      },
      {
        "tab": "Developers",
        "icon": "code",
        "groups": [
          { "group": "Developers", "pages": ["sdk", "api", "cli"] }
        ]
      }
    ]
  },
  "theme": {
    "mode": "light",
    "font": "Inter"
  },
  "api": {
    "openapi": "./openapi.yaml"
  }
}
```

**Alternative: Simple Sidebar Navigation**

For a simpler sidebar-only navigation (no horizontal tabs):

```json
{
  "navigation": [
    { "group": "Introduction", "pages": ["index", "vision"] },
    { "group": "Developers", "pages": ["sdk", "api", "cli"] }
  ]
}
```

---

## ✍️ Writing Content

Mintlify uses **Markdown (MDX)**, which allows **React components** and **dynamic content**.

### Common Elements

| Feature | Syntax / Example |
|----------|------------------|
| Headings | `#`, `##`, `###` |
| Code blocks | \`\`\`js ... \`\`\` |
| Tabs | `:::tabs` + `:::tab` blocks |
| Callouts | `:::tip`, `:::warning`, `:::info` |
| Embedding React components | `<Video src="..." />` or custom imports |
| Linking | `[label](link)` |

### Built‑in Components

Mintlify ships with a rich component set, including:

- Tabs & code groups
- Alerts / Callouts
- Card grids
- Interactive API blocks
- Tables
- Images, videos, and embeds

> Use `/` or `Cmd+K` in the web editor to insert these.

### AI Writing Assistant

Mintlify includes **AI completion and rewriting tools** in its editor:

- `Generate summary` — creates a TL;DR.
- `Improve readability` — optimizes phrasing.
- `Translate` — instant localization for docs.
- `Auto‑fix style` — ensures consistency.

---

## 🧠 Workflow: Editing, Branching, Publishing

### In the Web Editor

1. Choose the branch to work in.
2. Edit visually or in Markdown/MDX.
3. Preview changes instantly.
4. Commit → publish or open PR.

### In Cursor / VS Code

1. Work in the same `docs/` folder.
2. Edit `.mdx` files directly.
3. Run `npm run dev` to see changes.
4. Push → Mintlify auto‑syncs your branch.

### Pull Requests

- Each branch can have a live preview URL.
- Once merged to the deploy branch, Mintlify rebuilds the live site.

---

## 🧩 Configuration Deep Dive (`docs.json`)

| Field | Description |
|--------|-------------|
| `name` | Site name (shown in header & metadata) |
| `colors` | Theme colors |
| `navigation` | Defines sidebar hierarchy |
| `theme` | Fonts, mode, logo, and style |
| `api` | OpenAPI spec path or URL |
| `integrations` | Add MCP servers, tracking, analytics |
| `redirects` | Optional redirect mappings |

### Navigation Types

**1. Horizontal Tabs Navigation** (Recommended)
- Creates horizontal tabs at the top
- Each tab contains groups and pages
- Use `navigation.tabs` structure

**2. Simple Sidebar Navigation**
- Only sidebar navigation, no horizontal tabs
- Use simple `navigation` array structure

### Example: Adding an API Spec

```json
"api": {
  "openapi": "https://raw.githubusercontent.com/autoppia/openapi/main.yaml",
  "auth": true
}
```

This will auto‑generate interactive API docs.

---

## 🧰 Integrations & Advanced Features

### 🔌 REST API Integration

Mintlify provides a comprehensive REST API for programmatic control:

**Key Endpoints:**
- **Trigger Update**: `POST /api/update/trigger` - Force site rebuild
- **Get Update Status**: `GET /api/update/status` - Check deployment status
- **Create Agent Job**: `POST /api/agent/create-agent-job` - AI-powered content generation
- **Generate Assistant Message**: `POST /api/assistant/create-assistant-message` - Embed AI chat
- **Search Documentation**: `POST /api/assistant/search` - Programmatic search

**Authentication:**
- **Admin API Key**: `mint_*` prefix for site management
- **Assistant API Key**: `mint_dsc_*` prefix for AI features
- Generate keys in [Dashboard Settings](https://dashboard.mintlify.com/settings/organization/api-keys)

**Example API Usage:**
```bash
# Trigger site update
curl -X POST https://api.mintlify.com/update/trigger \
  -H "Authorization: Bearer mint_your_admin_key" \
  -H "Content-Type: application/json" \
  -d '{"domain": "your-docs.com"}'

# Generate AI assistant response
curl -X POST https://api.mintlify.com/assistant/create-assistant-message \
  -H "Authorization: Bearer mint_dsc_your_assistant_key" \
  -H "Content-Type: application/json" \
  -d '{"message": "How do I use the API?", "domain": "your-docs.com"}'
```

### 🤖 AI Agent Integration

**Slack Integration:**
- Add Mintlify Agent to your Slack workspace
- Team members can request documentation updates via chat
- Agent creates pull requests with proposed changes
- Supports natural language prompts

**API-Driven Agent:**
```bash
# Create agent job via API
curl -X POST https://api.mintlify.com/agent/create-agent-job \
  -H "Authorization: Bearer mint_your_admin_key" \
  -H "Content-Type: application/json" \
  -d '{
    "domain": "your-docs.com",
    "prompt": "Add documentation for the new authentication endpoint",
    "branch": "feature/auth-docs"
  }'
```

### 🔗 MCP Server Integration

Mintlify automatically generates **MCP (Model Context Protocol)** servers for AI tool integration.

**Features:**
- **Auto-generated** from your documentation and OpenAPI specs
- **Search tool** included by default
- **API endpoints** as tools (Pro/Enterprise plans)
- **Available at** `your-docs.com/mcp`

**Use Cases:**
- Generate API documentation from SDK comments
- Generate guides from PR descriptions
- Sync worker templates or studio SDK references
- Connect to Cursor, Claude, VS Code, and other AI tools

**MCP Configuration in OpenAPI:**
```json
{
  "paths": {
    "/api/v1/users": {
      "get": {
        "x-mint": {
          "mcp": {
            "enabled": true,
            "name": "get-users",
            "description": "Retrieve user list with pagination"
          }
        }
      }
    }
  }
}
```

### 🎯 Cursor Integration

**MCP Server Connection:**
1. Get your MCP server URL from dashboard
2. Add to Cursor's MCP configuration
3. Access your docs directly in Cursor's AI chat

**Configuration in Cursor:**
```json
{
  "mcpServers": {
    "mintlify-docs": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-mintlify"],
      "env": {
        "MINTLIFY_MCP_URL": "https://your-docs.com/mcp"
      }
    }
  }
}
```

**Benefits:**
- **Context-aware AI** with your documentation
- **Real-time API access** through MCP tools
- **Seamless development** workflow integration

### Analytics & SEO

- Track page views and engagement.
- Configure meta tags, title, and OpenGraph in `docs.json`.
- Optionally connect Google Analytics or Plausible.

### Custom Domain

Set up a CNAME record:

```
docs.autoppia.com → cname.mintlify.com
```

Add the domain in the project settings.

---

## 🌍 Localization

Mintlify supports translations per locale folder structure:

```
/docs
  /en
    index.mdx
  /es
    index.mdx
```

You can toggle languages from the site UI.

---

## 🔐 Access Control

Mintlify supports **private docs** for teams.

- Invite collaborators.
- Limit editing permissions.
- Protect with authentication (Mintlify Pro feature).

---

## 🪄 Useful Editor Shortcuts

| Action | Shortcut |
|--------|-----------|
| Search files | `Ctrl` / `Cmd` + `P` |
| Add link | `Ctrl` / `Cmd` + `K` |
| Create page | `/new` or “+” in sidebar |
| Insert component | `/` command |
| Preview mode | `Cmd` + `Shift` + `P` → “Preview” |

---

## 🧩 Built-in Components & Widgets

Mintlify provides a rich set of built-in components and widgets to enhance your documentation:

### 📋 Core Components

| Component | Description | Usage |
|-----------|-------------|-------|
| **Accordions** | Collapsible content sections | `<Accordion title="Title">Content</Accordion>` |
| **Accordion Groups** | Grouped accordions | `<AccordionGroup><Accordion>...</Accordion></AccordionGroup>` |
| **Banners** | Site-wide announcements | Configure in `docs.json` with `banner` property |
| **Callouts** | Highlighted information boxes | `:::tip`, `:::warning`, `:::info`, `:::danger` |
| **Tabs** | Tabbed content organization | `:::tabs` + `:::tab` blocks |
| **Code Blocks** | Syntax-highlighted code | \`\`\`language ... \`\`\` |
| **Tables** | Structured data display | Standard Markdown tables |
| **Cards** | Content grouping | Built-in card layouts |

### 🎨 Component Properties

**Accordion Properties:**
- `title` (required): Display title
- `description`: Subtitle text
- `defaultOpen`: Boolean to open by default
- `icon`: Font Awesome, Lucide, or custom SVG
- `iconType`: Font Awesome style (solid, regular, etc.)

**Banner Properties:**
- `content`: Message text (supports Markdown)
- `dismissible`: Boolean for user dismissal

### 🔧 Custom React Components

Create interactive widgets using React components:

```jsx
// In snippets/my-widget.jsx
export const MyWidget = () => {
  const [count, setCount] = useState(0);
  return (
    <div className="p-4 border rounded-xl">
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>
    </div>
  );
};
```

**Usage in MDX:**
```mdx
import { MyWidget } from '/snippets/my-widget.jsx';
<MyWidget />
```

### 📦 Reusable Snippets

Create reusable content components:

**Simple Snippet:**
```mdx
<!-- snippets/warning.mdx -->
⚠️ **Important:** This feature is in beta.
```

**Snippet with Variables:**
```mdx
<!-- snippets/version-info.mdx -->
Version {version} was released on {date}.
```

**Usage:**
```mdx
import WarningSnippet from '/snippets/warning.mdx';
import VersionInfo from '/snippets/version-info.mdx';

<WarningSnippet />
<VersionInfo version="2.0" date="2024-01-15" />
```

### 🎯 Advanced Widget Features

**Interactive Components:**
- State management with React hooks
- Event handlers and user interactions
- Dynamic content rendering
- Form inputs and validation
- Real-time data updates

**Custom Icons:**
- Font Awesome icons: `icon="home"`
- Lucide icons: `icon="search"`
- Custom SVG: `icon={<svg>...</svg>}`
- External URLs: `icon="https://example.com/icon.svg"`

## 🤖 AI-Native Features & Widgets

Mintlify includes powerful AI features that work as intelligent widgets:

### 🧠 AI Assistant Widget
- **Built-in chat interface** for user questions
- **Context-aware responses** from your documentation
- **API integration** for custom applications
- **Embeddable** in external sites

### 🔧 AI Agent Widget
- **Automated documentation updates** via pull requests
- **Slack integration** for team collaboration
- **API-driven** content generation
- **Cursor/VS Code integration** for development workflows

### 📋 Contextual Menu Widget
Add one-click AI integrations to any page:

```json
{
  "contextual": {
    "options": [
      "copy",        // Copy page as Markdown
      "view",        // View as Markdown
      "chatgpt",     // Open in ChatGPT
      "claude",      // Open in Claude
      "perplexity",  // Open in Perplexity
      "mcp",         // Copy MCP server URL
      "cursor",      // Connect to Cursor
      "vscode"       // Connect to VS Code
    ]
  }
}
```

**Custom Contextual Options:**
```json
{
  "contextual": {
    "options": [
      "copy",
      {
        "title": "Request Feature",
        "description": "Join GitHub discussion",
        "icon": "plus",
        "href": "https://github.com/orgs/your-org/discussions"
      }
    ]
  }
}
```

### 🔗 MCP (Model Context Protocol) Integration

**Automatic MCP Server:**
- **Auto-generated** from your documentation
- **Available at** `your-docs.com/mcp`
- **Search tool** included by default
- **API endpoints** as tools (Pro/Enterprise)

**MCP Configuration:**
```json
// In OpenAPI spec
{
  "x-mint": {
    "mcp": {
      "enabled": true,
      "name": "get-users",
      "description": "Get a list of users"
    }
  }
}
```

## 🧩 Tips & Best Practices

1. **Keep navigation flat** — avoid over‑nesting pages.
2. **Use callouts** to highlight key info.
3. **Include examples & code blocks** for developers.
4. **Document updates with PRs** — docs-as-code mindset.
5. **Use AI assist** to polish writing.
6. **Keep `docs.json` minimal** — organize logically.
7. **Enable analytics** to track engagement.
8. **Link SDKs & APIs** for developer context.
9. **Auto-generate API docs** from specs — less manual maintenance.
10. **Preview before publishing**.
11. **Use interactive widgets** to engage users.
12. **Enable contextual menu** for AI tool integration.
13. **Create reusable snippets** for consistent content.
14. **Leverage MCP server** for AI tool connectivity.
15. **Use accordions** for progressive disclosure.

---

## 🎨 Advanced Widget Configurations

### 🔧 Interactive Component Examples

**Color Picker Widget:**
```jsx
// snippets/color-picker.jsx
export const ColorPicker = () => {
  const [hue, setHue] = useState(180);
  const [saturation, setSaturation] = useState(50);
  const [lightness, setLightness] = useState(50);
  
  const colors = Array.from({length: 5}, (_, i) => {
    const l = Math.max(10, Math.min(90, lightness - 20 + i * 10));
    return `hsl(${hue}, ${saturation}%, ${l}%)`;
  });

  const copyToClipboard = (color) => {
    navigator.clipboard.writeText(color);
  };

  return (
    <div className="p-4 border rounded-xl">
      <div className="space-y-4">
        <div className="space-y-2">
          <label>Hue: {hue}°</label>
          <input
            type="range"
            min="0" max="360"
            value={hue}
            onChange={(e) => setHue(Number(e.target.value))}
            className="w-full"
          />
        </div>
        <div className="flex space-x-1">
          {colors.map((color, idx) => (
            <div
              key={idx}
              className="h-16 rounded flex-1 cursor-pointer"
              style={{ backgroundColor: color }}
              onClick={() => copyToClipboard(color)}
              title={`Click to copy: ${color}`}
            />
          ))}
        </div>
      </div>
    </div>
  );
};
```

**API Status Widget:**
```jsx
// snippets/api-status.jsx
export const ApiStatus = ({ endpoint }) => {
  const [status, setStatus] = useState('checking');
  const [responseTime, setResponseTime] = useState(null);

  useEffect(() => {
    const checkStatus = async () => {
      const start = Date.now();
      try {
        const response = await fetch(endpoint);
        const end = Date.now();
        setResponseTime(end - start);
        setStatus(response.ok ? 'healthy' : 'error');
      } catch {
        setStatus('error');
      }
    };

    checkStatus();
    const interval = setInterval(checkStatus, 30000);
    return () => clearInterval(interval);
  }, [endpoint]);

  return (
    <div className="flex items-center space-x-2 p-2 rounded">
      <div className={`w-3 h-3 rounded-full ${
        status === 'healthy' ? 'bg-green-500' : 
        status === 'error' ? 'bg-red-500' : 'bg-yellow-500'
      }`} />
      <span className="text-sm">
        {status === 'healthy' ? 'Online' : 
         status === 'error' ? 'Offline' : 'Checking...'}
        {responseTime && ` (${responseTime}ms)`}
      </span>
    </div>
  );
};
```

### 📊 Data Visualization Widgets

**Chart Component:**
```jsx
// snippets/simple-chart.jsx
export const SimpleChart = ({ data, type = 'bar' }) => {
  const maxValue = Math.max(...data.map(d => d.value));
  
  return (
    <div className="p-4 border rounded-xl">
      <div className="space-y-2">
        {data.map((item, index) => (
          <div key={index} className="flex items-center space-x-2">
            <span className="w-20 text-sm">{item.label}</span>
            <div className="flex-1 bg-gray-200 rounded h-4 relative">
              <div
                className="bg-blue-500 h-4 rounded"
                style={{ width: `${(item.value / maxValue) * 100}%` }}
              />
            </div>
            <span className="text-sm font-mono">{item.value}</span>
          </div>
        ))}
      </div>
    </div>
  );
};
```

### 🎯 Form Widgets

**Contact Form:**
```jsx
// snippets/contact-form.jsx
export const ContactForm = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  const [status, setStatus] = useState('');

  const handleSubmit = async (e) => {
    e.preventDefault();
    setStatus('sending');
    
    try {
      // Replace with your actual form submission logic
      await fetch('/api/contact', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
      });
      setStatus('success');
    } catch {
      setStatus('error');
    }
  };

  return (
    <form onSubmit={handleSubmit} className="space-y-4 p-4 border rounded-xl">
      <div>
        <label className="block text-sm font-medium mb-1">Name</label>
        <input
          type="text"
          value={formData.name}
          onChange={(e) => setFormData({...formData, name: e.target.value})}
          className="w-full p-2 border rounded"
          required
        />
      </div>
      <div>
        <label className="block text-sm font-medium mb-1">Email</label>
        <input
          type="email"
          value={formData.email}
          onChange={(e) => setFormData({...formData, email: e.target.value})}
          className="w-full p-2 border rounded"
          required
        />
      </div>
      <div>
        <label className="block text-sm font-medium mb-1">Message</label>
        <textarea
          value={formData.message}
          onChange={(e) => setFormData({...formData, message: e.target.value})}
          className="w-full p-2 border rounded h-24"
          required
        />
      </div>
      <button
        type="submit"
        disabled={status === 'sending'}
        className="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600 disabled:opacity-50"
      >
        {status === 'sending' ? 'Sending...' : 'Send Message'}
      </button>
      {status === 'success' && (
        <div className="text-green-600 text-sm">Message sent successfully!</div>
      )}
      {status === 'error' && (
        <div className="text-red-600 text-sm">Failed to send message. Please try again.</div>
      )}
    </form>
  );
};
```

### 🔧 Widget Best Practices

**Performance Optimization:**
- Use `useMemo` for expensive calculations
- Implement `useCallback` for event handlers
- Lazy load heavy components
- Optimize re-renders with proper dependencies

**Accessibility:**
- Include proper ARIA labels
- Ensure keyboard navigation
- Provide alt text for images
- Use semantic HTML elements

**Error Handling:**
```jsx
export const RobustWidget = () => {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch('/api/data');
        if (!response.ok) throw new Error('Failed to fetch');
        setData(await response.json());
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div className="text-red-500">Error: {error}</div>;
  if (!data) return <div>No data available</div>;

  return <div>{/* Your widget content */}</div>;
};
```

## 🧑‍💻 Recommended Workflow for Teams Using Cursor

1. **Setup Development Environment:**
   - Open your Mintlify docs repo in **Cursor**
   - Install Mintlify CLI: `npm install -g mintlify`
   - Run local dev server: `mintlify dev`

2. **Content Creation:**
   - Edit `.mdx` files with **AI pair programming**
   - Use Cursor's AI to generate documentation from code
   - Create interactive widgets in `/snippets` folder
   - Test components locally before deployment

3. **AI Integration:**
   - Connect MCP server to Cursor for context-aware AI
   - Use contextual menu for quick AI tool access
   - Leverage AI agent for automated documentation updates

4. **Deployment Workflow:**
   - Push to feature branch → Mintlify auto-creates preview
   - Review changes in preview environment
   - Merge to main branch → Auto-deploy to production
   - Monitor with built-in analytics

5. **Advanced Features:**
   - Use REST API for programmatic updates
   - Integrate with CI/CD pipelines
   - Set up automated testing for components
   - Monitor performance and user engagement

---

## 🧾 Resources & References

### 📚 Official Documentation
- [Mintlify Official Docs](https://mintlify.com/docs) - Complete documentation
- [Quickstart Guide](https://mintlify.com/docs/quickstart) - Get started in minutes
- [Config Reference (docs.json)](https://mintlify.com/docs/settings) - Configuration options
- [Components Reference](https://mintlify.com/docs/components) - Built-in components
- [API Reference](https://mintlify.com/docs/api) - REST API documentation

### 🤖 AI & Integration Features
- [AI-Native Features](https://mintlify.com/docs/ai-native) - AI-powered documentation
- [Contextual Menu](https://mintlify.com/docs/ai/contextual-menu) - One-click AI integrations
- [MCP Integration](https://mintlify.com/docs/ai/model-context-protocol) - Model Context Protocol
- [AI Assistant](https://mintlify.com/docs/ai/assistant) - Built-in chat interface
- [AI Agent](https://mintlify.com/docs/ai/agent) - Automated documentation updates

### 🛠️ Development & Customization
- [React Components](https://mintlify.com/docs/customize/react-components) - Custom widgets
- [Reusable Snippets](https://mintlify.com/docs/create/reusable-snippets) - Content components
- [Visual Editor](https://mintlify.com/docs/editor) - Browser-based editing
- [CLI Installation](https://mintlify.com/docs/installation) - Local development
- [Custom Scripts](https://mintlify.com/docs/customize/custom-scripts) - Advanced customization

### 🔗 Integration Guides
- [Cursor Integration](https://mintlify.com/docs/guides/cursor) - Cursor IDE setup
- [Claude Code Integration](https://mintlify.com/docs/guides/claude-code) - Claude integration
- [Windsurf Integration](https://mintlify.com/docs/guides/windsurf) - Windsurf setup
- [GitHub Integration](https://mintlify.com/docs/editor) - Git workflow
- [Slack Integration](https://mintlify.com/docs/ai/agent) - Team collaboration

### 📊 Analytics & SEO
- [Analytics Setup](https://mintlify.com/docs/optimize/analytics) - User tracking
- [SEO Optimization](https://mintlify.com/docs/optimize/seo) - Search optimization
- [Custom Domain](https://mintlify.com/docs/customize/domain) - Domain setup
- [Authentication](https://mintlify.com/docs/dashboard/authentication) - Access control

### 🆘 Support & Community
- [Mintlify Blog](https://mintlify.com/blog) - Latest updates and tutorials
- [GitHub Discussions](https://github.com/orgs/mintlify/discussions) - Community support
- [Feature Requests](https://github.com/orgs/mintlify/discussions/categories/feature-requests) - Request new features
- [Contact Support](https://mintlify.com/docs/contact-support) - Direct support
- [Status Page](https://mintlify.com/docs/status) - Service status

---

## 🎯 Key Findings: Widgets & Advanced Features for Cursor Users

### 🧩 Comprehensive Widget Ecosystem

**Built-in Components:**
- **Accordions & Accordion Groups** - Collapsible content with icons and custom styling
- **Banners** - Site-wide announcements with dismissible options
- **Callouts** - Rich information boxes (tip, warning, info, danger)
- **Tabs** - Organized content sections
- **Interactive Code Blocks** - Syntax highlighting with copy functionality

**Custom React Widgets:**
- **Interactive Components** - State management, event handlers, real-time updates
- **Data Visualization** - Charts, graphs, and data displays
- **Form Widgets** - Contact forms, surveys, and user input
- **API Status Widgets** - Real-time service monitoring
- **Color Pickers** - Interactive design tools

### 🤖 AI-Native Widget Features

**Contextual Menu Widget:**
- **One-click AI integrations** - ChatGPT, Claude, Perplexity
- **MCP server connection** - Direct integration with Cursor
- **Copy functionality** - Markdown export for AI tools
- **Custom options** - Branded integrations and workflows

**AI Assistant Widget:**
- **Built-in chat interface** - Context-aware responses
- **API integration** - Embeddable in external applications
- **Real-time search** - Semantic understanding of content

**AI Agent Widget:**
- **Automated documentation** - Pull request generation
- **Slack integration** - Team collaboration features
- **API-driven updates** - Programmatic content management

### 🔗 MCP (Model Context Protocol) Integration

**Automatic MCP Server:**
- **Auto-generated** from documentation and OpenAPI specs
- **Available at** `your-docs.com/mcp`
- **Search tool** included by default
- **API endpoints** as tools (Pro/Enterprise)

**Cursor Integration Benefits:**
- **Context-aware AI** with your documentation
- **Real-time API access** through MCP tools
- **Seamless development** workflow integration
- **Direct connection** via contextual menu

### 🛠️ Advanced Development Features

**REST API Capabilities:**
- **Trigger Updates** - Force site rebuilds
- **Agent Jobs** - AI-powered content generation
- **Assistant Messages** - Programmatic AI chat
- **Search API** - Documentation search integration

**Reusable Snippets:**
- **DRY principle** - Don't repeat yourself
- **Variable support** - Dynamic content injection
- **JSX components** - Interactive widgets
- **Import system** - Modular content architecture

### 🎨 Widget Best Practices

**Performance Optimization:**
- Use `useMemo` and `useCallback` for React components
- Implement lazy loading for heavy widgets
- Optimize re-renders with proper dependencies
- Error handling and loading states

**Accessibility:**
- ARIA labels and semantic HTML
- Keyboard navigation support
- Screen reader compatibility
- Color contrast compliance

### 🚀 Cursor-Specific Workflow

**Development Environment:**
1. **Local setup** with `mintlify dev`
2. **AI pair programming** for content generation
3. **Interactive widget creation** in `/snippets`
4. **MCP server connection** for context-aware AI

**Deployment Pipeline:**
1. **Feature branch** → Auto-preview generation
2. **Review process** → Preview environment testing
3. **Merge to main** → Auto-deploy to production
4. **Analytics monitoring** → Performance tracking

### 📊 Key Insights for Cursor Users

**1. Widget Ecosystem is Comprehensive**
- Mintlify provides both built-in and custom widget capabilities
- React components enable complex interactive features
- AI widgets offer seamless integration with development tools

**2. AI Integration is Native**
- Contextual menu provides one-click AI tool access
- MCP server enables direct Cursor integration
- AI agent automates documentation workflows

**3. Development Workflow is Optimized**
- Local development with hot reloading
- Git-based deployment with preview environments
- API-driven programmatic control

**4. Customization is Extensive**
- Reusable snippets for consistent content
- Custom React components for unique functionality
- Advanced configuration options for branding

**5. Performance is Built-in**
- Optimized rendering and loading
- Built-in analytics and monitoring
- SEO optimization and accessibility features

### 🎯 Recommendations for Cursor Teams

**Immediate Actions:**
1. **Set up MCP server connection** for context-aware AI
2. **Enable contextual menu** for quick AI tool access
3. **Create reusable snippets** for consistent content
4. **Implement interactive widgets** for user engagement

**Advanced Implementation:**
1. **Use REST API** for programmatic updates
2. **Integrate AI agent** for automated documentation
3. **Set up analytics** for performance monitoring
4. **Create custom components** for unique functionality

**Long-term Strategy:**
1. **Build widget library** for consistent user experience
2. **Implement CI/CD** for automated deployments
3. **Monitor performance** and user engagement
4. **Iterate based on analytics** and user feedback

---

© 2025 — Compiled for internal use with **Cursor + Autoppia workflows**.