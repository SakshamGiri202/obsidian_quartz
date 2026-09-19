---
<%*
let fileDate = tp.file.title;
let today = moment(fileDate, "YYYY-MM-DD", true).isValid() ? moment(fileDate) : moment();
let yesterday = today.clone().subtract(1, "day").format("YYYY-MM-DD");
let tomorrow = today.clone().add(1, "day").format("YYYY-MM-DD");
%>
date: <% tp.date.now("YYYY-MM-DD") %>
tags:
  - daily
prev: [[<% yesterday %>]]
next: [[<% tomorrow %>]]
mood: 
energy: 
---

# 📅 <% today.format("dddd, MMMM D, YYYY") %>

> [!quote] Daily Intention
> "_Today I will focus on..._"

## 🌅 Morning

### Top 3 Priorities
1. [ ] 
2. [ ] 
3. [ ] 

### Gratitude
- 

## 📝 Log

| Time | Activity | Notes |
|------|----------|-------|
| 09:00 | | |
| 11:00 | | |
| 13:00 | | |
| 15:00 | | |
| 17:00 | | |

## 💡 Ideas & Thoughts

## 📥 Inbox

_Things to process later_

- [ ] 

## 🌙 Evening Reflection

### What Went Well

### What I Learned

### Tomorrow's Focus


