# Context Findings

## Overview
Based on the discovery answers and research conducted, here are the key technical findings for building a custom Deep Research system with Claude Code.

## 1. Web Scraping Architecture

### Recommended Technology Stack
- **Primary Library**: Playwright (preferred over Selenium)
  - Superior performance and modern architecture
  - Better handling of JavaScript-heavy sites
  - Built-in stealth capabilities
- **Fallback**: BeautifulSoup + Requests for simple static sites
- **Rate Limiting**: 60 requests/minute baseline, 1-5 second delays

### Key Implementation Considerations
- Implement request throttling and random delays
- Use residential proxies for anti-bot avoidance
- Respect robots.txt and identify scraper properly
- Handle 429 status codes gracefully
- Focus on publicly available data (legally safer)

## 2. MCP Integration for Local Data

### Configuration Structure
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/path/to/research/directory"
      ]
    }
  }
}
```

### Security Best Practices
- Use read-only mounts where possible
- Implement fine-grained directory permissions
- Consider Docker-based sandboxing for enhanced security
- Store API keys in environment variables

### Available Pre-built Servers
- Filesystem (local file access)
- GitHub (repository management)
- PostgreSQL/SQLite (database access)
- Puppeteer/Playwright (browser automation)
- Memory Server (persistent context)

## 3. Claude Artifacts for Report Generation

### Capabilities for Research Reports
- Interactive HTML/CSS/JS documents
- SVG visualizations and diagrams
- Collapsible sections for organization
- Professional formatting with Tailwind CSS
- Version control and sharing capabilities

### Limitations to Consider
- No code execution within artifacts
- No real-time data or external API access
- Read-only format (cannot be directly edited)
- Limited to visual and text-based content

## 4. Orchestration Architecture

### Multi-Phase Workflow Design
1. **Task Decomposition Phase**
   - Break research query into sub-questions
   - Generate research plan (JSON format)
   - Human approval checkpoint

2. **Execution Loop Phase**
   - Web scraping for each sub-question
   - Text-based analysis (no code execution per requirements)
   - Summarization of findings
   - Source validation checkpoint

3. **Synthesis Phase**
   - Aggregate all findings
   - Generate interactive HTML report via Artifacts
   - Final human review checkpoint

### Prompt Engineering Strategy
- Use role-specific system prompts for each phase
- Implement prompt chaining for context continuity
- Request structured outputs (JSON) for parsing
- Include explicit instructions for citation tracking

## 5. Specific Files to Create/Modify

Based on the requirements, the following structure is recommended:

```
deep-research/
├── orchestrator/
│   ├── main.py                    # Main orchestration script
│   ├── prompts/
│   │   ├── decomposition.txt      # Task breakdown prompt
│   │   ├── analysis.txt           # Analysis prompt template
│   │   └── synthesis.txt          # Final report prompt
│   └── config.yaml                # Configuration settings
├── scrapers/
│   ├── playwright_scraper.py      # Main web scraping module
│   └── utils.py                   # Helper functions
├── mcp-config/
│   └── claude_desktop_config.json # MCP server configuration
└── templates/
    └── report_template.html       # Base HTML template for reports
```

## 6. Integration Points Identified

### External Dependencies
- Playwright for web scraping
- Python 3.8+ for orchestration
- Node.js for MCP servers
- Claude API access

### Key APIs and Services
- Claude API for LLM operations
- Proxy services for web scraping (optional)
- Local filesystem via MCP

## 7. Technical Constraints

### Based on Requirements
- No code execution for data analysis (keep it simple)
- Focus on public web content (primary)
- Local file access as secondary feature
- Human-in-the-loop checkpoints required
- Interactive report output required

### Performance Considerations
- API rate limits and costs
- Web scraping delays for ethical compliance
- Context window limitations (200K tokens)
- Processing time: 5-30 minutes per research task

## 8. Patterns to Follow

### From INSTRUCTIONS.md Blueprint
- Search → Analyze → Synthesize workflow
- JSON-based communication between phases
- Structured logging of all operations
- Citation tracking throughout process
- Modular component design

### Best Practices Identified
- Containerize security-sensitive components
- Implement comprehensive error handling
- Use tiered model strategy for cost optimization
- Cache scraped content to avoid re-fetching
- Version control for prompts and configurations