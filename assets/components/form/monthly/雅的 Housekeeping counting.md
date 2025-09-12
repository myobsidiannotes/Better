
![[New Housekeeping.cform]]

```dataviewjs
        let total = 0;
        const content = await dv.io.load(dv.current().file.path);

        content.split("\n").forEach(line => {
            if (line.includes("$")) {
                const match = line.match(/(\d+)\s?$/i);
                if (match) total += parseInt(match[1]);
            }
        });
        total = parseInt(total).toLocaleString();

        dv.el("p", `💰 Total : $HKD ${total} `);
        ```
```dataviewjs
// Load current file content
const content = await dv.io.load(dv.current().file.path);

// Parse each line to find date/amount pairs
const items = content.split('\n')
  .map(line => {
    const match = line.match(/- (\d{4}\/\d{1,2}\/\d{1,2}) - \$(\d+)/i);
    if (!match) return null;
    
    return {
      date: match[1].replace(/\//g, '-'), // Convert to YYYY-MM-DD format
      amount: parseInt(match[2])
    };
  })
  .filter(item => item !== null) // Remove non-matching lines
  .sort((a, b) => new Date(b.date) - new Date(a.date)); // Sort by date

// Display as formatted table
dv.table(
  ["Date", "Amount"],
  items.map(item => [
    item.date, 
    "$" + item.amount.toLocaleString()
  ])
);
```
^Housekeeping-counting

### Data
- 2025/04/01 - $6000
- 2025/05/01 - $6000
- 2025/06/01 - $6000
- 2025/07/01 - $6000
- 2025/08/01 - $6000