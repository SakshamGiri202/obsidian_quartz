---
<%*
const type = await tp.system.suggester(["📅 Daily", "📆 Weekly", "🗓️ Monthly", "📊 Quarterly"], ["daily", "weekly", "monthly", "quarterly"])
let fileTitle = tp.file.title;
let today = moment(fileTitle, "YYYY-MM-DD", true).isValid() ? moment(fileTitle) : moment();

if (type === "weekly") {
  let weekStart = today.startOf("week");
  let prevWeek = moment(weekStart).subtract(7, "days").format("gggg-[W]ww");
  let nextWeek = moment(weekStart).add(7, "days").format("gggg-[W]ww");
  tR += `prev: [[${prevWeek}]]\nnext: [[${nextWeek}]]\n`;
} else if (type === "daily") {
  let yesterday = today.clone().subtract(1, "day").format("YYYY-MM-DD");
  let tomorrow = today.clone().add(1, "day").format("YYYY-MM-DD");
  tR += `prev: [[${yesterday}]]\nnext: [[${tomorrow}]]\n`;
} else {
  tR += `prev: \nnext: \n`;
}
%>
tags:
  - review
  - <% type %>
date: <% tp.date.now("YYYY-MM-DD") %>
---

# 📋 <% type.charAt(0).toUpperCase() + type.slice(1) %> Review — <% tp.date.now("YYYY-MM-DD") %>

> [!review] Focus Question
> _What did I learn this <% type %> that I didn't know before?_

## 🎉 Wins

- 

## 🚧 Challenges

- 

## 📊 Metrics

| Area | Last | This | Trend |
|------|------|------|-------|
| Productivity | | | ➡️ |
| Learning | | | ➡️ |
| Health | | | ➡️ |
| Relationships | | | ➡️ |

## 📝 Reflections

### What Went Well

### What Could Improve

### What Will I Do Differently

## 🎯 Next <% type %> Focus

1. 
2. 
3. 

## 🔗 Links

<%* if (type === "weekly") { %>
### Daily Notes This Week
- [[<% tp.date.now("YYYY-MM-DD", 0, tp.date.now("YYYY-MM-DD"), "-dddd") %>]]
- [[<% tp.date.now("YYYY-MM-DD", 1, tp.date.now("YYYY-MM-DD"), "-dddd") %>]]
- [[<% tp.date.now("YYYY-MM-DD", 2, tp.date.now("YYYY-MM-DD"), "-dddd") %>]]
- [[<% tp.date.now("YYYY-MM-DD", 3, tp.date.now("YYYY-MM-DD"), "-dddd") %>]]
- [[<% tp.date.now("YYYY-MM-DD", 4, tp.date.now("YYYY-MM-DD"), "-dddd") %>]]
<%* } %>
