<%*
const f = tp.file.find_tfile(tp.file.path(true));
await app.vault.modify(f, "");

let output = "";

const ts = Number(tp.date.now("X"));
const start = tp.date.now("YYYY-MM-DD HH:mm");

output += `---\n`;
output += `length: 12\n`;
output += `start: ${start}\n`;
output += `end: \n`;
output += `timestamp: ${ts}\n`;
output += `---\n\n`;

tR = output;

await tp.file.rename(String(ts))
-%>
