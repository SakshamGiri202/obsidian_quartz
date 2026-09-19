---
<%*
const env = await tp.system.suggester(["🖥️ Production", "🧪 Staging", "🧑‍💻 Development", "🧪 Testing"], ["production", "staging", "development", "testing"])
%>
created: <% tp.date.now("YYYY-MM-DD") %>
modified: <% tp.date.now("YYYY-MM-DD") %>
tags:
  - backend
  - config
  - <% env %>
environment: <% env %>
service: 
version: 
---

# ⚙️ Config: <% tp.file.title %>

> [!config] Purpose
> _What does this configuration control?_

## 📋 Details

| Property | Value |
|----------|-------|
| Environment | `<% env %>` |
| Service | |
| Last Updated | |
| Maintainer | |

## 🔧 Configuration

```yaml
# Add config here
```

## 🔑 Environment Variables

| Variable | Value | Description |
|----------|-------|-------------|
| | | |

## 📝 Change Log

| Date | Change | Author |
|------|--------|--------|
| <% tp.date.now("YYYY-MM-DD") %> | Initial config | |

## 🔗 Related

- Runbook: [[ ]]
- Script: [[ ]]
- Monitoring: [[ ]]
