# Car Budget

A small personal app for tracking car spending against a monthly budget. It runs in
Chrome on Android and can be installed to the home screen.

- **Two monthly budgets:** Petrol, and Parking & Others (car park, car wash, other).
- **Quick entry:** tap **+**, pick a category, type the amount, tap **Save**.
- **Monthly view:** budget, spent and balance left for each budget, with a colour bar
  (green under 80%, amber from 80% to 100%, red when over).
- **Your data stays on your phone.** There is no account and no server. Use
  ⚙️ → **Save backup** regularly.

## Files

| File | What it is |
|------|------------|
| `index.html` | The whole app: screens, styles and logic |
| `manifest.webmanifest` | Name, icon and colours used when the app is installed |
| `sw.js` | Makes the app work offline |
| `icon-192.png`, `icon-512.png` | App icons (made by `../tools/make-icons.js`) |
| `INSTALL_GUIDE.md` | Step-by-step guide for installing on the phone |

## Run it on a PC

Open `index.html` in Chrome. To test offline mode and installing, serve it over HTTP
instead:

```
npx serve .
```

## Data format

Data is stored in `localStorage` under the key `carBudget.v1`. Amounts are stored as
whole cents to avoid rounding errors:

```json
{
  "amountUnit": "cents",
  "currency": "S$",
  "budgets": { "petrol": 30000, "others": 15000 },
  "monthOverrides": { "2026-12": { "petrol": 40000 } },
  "entries": [
    { "id": 1727150000000, "cat": "petrol", "amount": 8550, "date": "2026-09-24", "note": "Shell" }
  ],
  "lastCat": "petrol"
}
```

Categories: `petrol` counts toward the Petrol budget. `parking`, `carwash` and `other`
count toward Parking & Others.

A backup file wraps this as `{ "app": "car-budget", "version": 1, "exportedAt": …, "data": {…} }`.
Restore also accepts the same shape with amounts in dollars (no `amountUnit` field).
