---
<%*
const category = await tp.system.suggester(["🎯 Goal", "📊 Milestone", "🔄 Habit", "📈 OKR"], ["goal", "milestone", "habit", "okr"])
%>
created: <% tp.date.now("YYYY-MM-DD") %>
tags:
  - planning
  - <% category %>
deadline: 
status: "#active"
priority: 
---

# <% category %> <% tp.file.title %>

> [!goal] Why This Matters
> _What will change when this is achieved?_

## 📍 Current State

_Where am I now?_

## 🏁 Target State

_Where do I want to be?_

## 🗺️ Action Plan

| Step | Action | Due | Status |
|------|--------|-----|--------|
| 1 | | | 🔲 |
| 2 | | | 🔲 |
| 3 | | | 🔲 |

## 📈 Progress

- [ ] 25%
- [ ] 50%
- [ ] 75%
- [ ] 100% 🎉

## 📅 Review Dates

| Review | Date | Notes |
|--------|------|-------|
| Weekly | | |
| Monthly | | |
| Quarterly | | |

## 🔗 Related

- Project: [[ ]]
- Habit: [[ ]]
- Review: [[ ]]
