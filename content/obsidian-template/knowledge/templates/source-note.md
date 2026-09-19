---
<%*
const type = await tp.system.suggester(["📰 Article", "📖 Book", "🎥 Video", "🎧 Podcast", "🔬 Paper"], ["article", "book", "video", "podcast", "paper"])
%>
created: <% tp.date.now("YYYY-MM-DD") %>
tags:
  - knowledge
  - <% type %>
source: 
author: 
url: 
status: "#to-read"
rating: 
---

# <% type %> <% tp.file.title %>

> [!abstract] Summary
> _2-3 sentence summary of the core ideas_

## 📌 Key Ideas

1. 
2. 
3. 

## 💡 Highlights

> "Highlight text goes here" — (p. X / timestamp)

## 🧠 My Notes

## 🔗 Connections

| Concept | Link |
|---------|------|
| Related idea | [[ ]] |
| Contradicts | [[ ]] |
| Supports | [[ ]] |

## 📝 Action Items

- [ ] Apply: 
- [ ] Research further: 

## 🏷️ Tags

#knowledge #<% type %> # 
