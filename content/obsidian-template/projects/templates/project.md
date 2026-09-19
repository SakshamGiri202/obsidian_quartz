---
<%*
const status = await tp.system.suggester(["🟢 Active", "🟡 On Hold", "🔵 Planning", "⚫ Archived"], ["active", "on-hold", "planning", "archived"])
const priority = await tp.system.suggester(["🔥 High", "⚡ Medium", "💤 Low"], ["high", "medium", "low"])
tR += `created: ${tp.date.now("YYYY-MM-DD")}\n`
tR += `modified: ${tp.date.now("YYYY-MM-DD")}\n`
%>
tags:
  - project
status: <% status %>
priority: <% priority %>
area: 
deadline: 
---

# 📁 <% tp.file.title %>

> [!brief] Project Brief
> _One-line description of what this project is about_

## 🎯 Objectives

- [ ] 

## 📋 Phases

| Phase | Status | Deliverable | Due |
|-------|--------|-------------|-----|
| Discovery | 🔲 | | |
| Execution | 🔲 | | |
| Review | 🔲 | | |
| Ship | 🔲 | | |

## ✅ Tasks

### This Week
- [ ] 

### Backlog
- [ ] 

## 📝 Notes

## 🔗 Connections

| Related | Link |
|---------|------|
| Goal | [[ ]] |
| Resources | [[ ]] |
| Meeting Notes | [[ ]] |

## 📊 Metrics

- Progress: 0%
- Time Logged: 
- Budget: 
