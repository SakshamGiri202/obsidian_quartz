---
created: <% tp.date.now("YYYY-MM-DD HH:mm") %>
modified: <% tp.date.now("YYYY-MM-DD HH:mm") %>
<%*
const type = await tp.system.suggester(["📝 General Note", "💡 Idea", "📖 Meeting", "🔍 Research", "📌 Reference"], ["note", "idea", "meeting", "research", "reference"])
const status = await tp.system.suggester(["🔴 Draft", "🟡 In Progress", "🟢 Final", "⚫ Archived"], ["draft", "in-progress", "final", "archived"])
%>
tags:
  - <% type %>
status: <% status %>
source: 
related: 
---

# <% type %> <% tp.file.title %>

> [!summary] TL;DR
> _One-line summary of this note_

## 📝 Content



## 📌 Key Points

- 

## 🔗 Connections

| Relation | Link |
|----------|------|
| Related to | [[ ]] |
| Source | [[ ]] |
| Follow-up | [[ ]] |

## 📎 Attachments

- 

## 🏷️ Tags

# 
