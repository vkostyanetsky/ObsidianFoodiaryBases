# ObsidianFoodiary (Bases)

```
---
calories:
protein:
fat:
carbs:
water:
unit:
unit_size:
aliases:
---
```

```
<%*
const f = tp.file.find_tfile(tp.file.path(true));
await app.vault.modify(f, "");

let output = "";

const ts = Number(tp.date.now("X"));
const day = tp.date.now("YYYY-MM-DD");

output += `---\n`;
output += `day: ${day}\n`;
output += `product: \n`;
output += `quantity: \n`;
output += `timestamp: ${ts}\n`;
output += `---\n\n`;

tR = output;

await tp.file.rename(String(ts)) 
-%>
```