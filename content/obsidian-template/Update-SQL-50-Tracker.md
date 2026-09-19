<%*
const trackerPath = "Database/SQL-Mastery-Vault/00-SQL-50-Tracker.md";
const file = app.vault.getAbstractFileByPath(trackerPath);
if (!file) {
  throw new Error("Tracker not found at " + trackerPath);
}
const content = await app.vault.read(file);
const lines = content.split("\n");

const headerIdxs = [];
lines.forEach((l, i) => { if (l.startsWith("## ")) headerIdxs.push(i); });

let totalAll = 0, doneAll = 0;
headerIdxs.forEach((idx, i) => {
  const end = headerIdxs[i + 1] ?? lines.length;
  const block = lines.slice(idx, end);
  const total = block.filter(l => /^- \[[ x]\]/.test(l)).length;
  const done = block.filter(l => /^- \[x\]/.test(l)).length;
  totalAll += total;
  doneAll += done;
  lines[idx] = lines[idx].replace(/\(\d+\/\d+\)/, `(${done}/${total})`);
});

const progressIdx = lines.findIndex(l => l.startsWith("**Progress:**"));
if (progressIdx >= 0) {
  lines[progressIdx] = `**Progress:** \`${doneAll} / ${totalAll} Completed\``;
}

await app.vault.process(file, () => lines.join("\n"));
%>
