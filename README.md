# Car Load Planner

A browser tool that plans where each car goes on a car transporter. You enter the cars for a trip and it gives a position for each one on the truck and the trailer. Every plan is checked against length, height, stacked height, deck length, payload and unloading order.

It is the first module of **SBMF**, a SaaS ERP for small and medium road transport companies. Car-transport load planning is the first vertical module.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole planner: layout, solver, drawing and settings |
| `cars-db.js` | Database of 500 car model generations (length, height, kerb weight, wheelbase, ground clearance, body type, flags) |
| `README.md` | This file |

There is no build step, server or install. Opening `index.html` in a browser is enough. Keep `cars-db.js` in the same folder.

## Put it online with GitHub Pages

1. Create a new repository on GitHub, for example `car-load-planner`.
2. Click **Add file → Upload files** and upload `index.html`, `cars-db.js` and `README.md`. Then click **Commit changes**.
3. Go to **Settings → Pages**. Under "Build and deployment" choose **Deploy from a branch**, then branch `main` and folder `/ (root)`, and click **Save**.
4. After about a minute the planner is live at `https://<your-username>.github.io/car-load-planner/`.

GitHub Pages is free for public repositories. For a private repository it needs a paid GitHub plan.

## What it does

- **Trucks:** Lohr Eurolohr 200 with a low cab (9 positions) or a high cab (8 positions, no spot over the cab).
- **Positions:**
  - truck: T1 over the cab (Lohr B2), T2 upper deck, T3 lower deck
  - trailer: R1–R3 upper deck (Lohr K2), R4–R6 lower deck
- **Cars:** up to 12 per trip. You can search 575 models (500 from `cars-db.js`) or type sizes by hand. Each car has:
  - a delivery stop
  - a roof box option (+0.40 m)
  - an optional fixed position
- **Checks:**
  - maximum length and height for each position
  - stacked height for each upper/lower pair
  - total length of the three cars on each trailer deck
  - payload for the truck and for the trailer
  - unloading order by stop
  - tight margins, plus the reason any car could not be loaded
- **Loading rules:**
  - big and tall cars go on top
  - short cars fit the middle spots
  - cars that are tall for their size are kept off the over-cab spot
- **Route rules:**
  - as recorded in the real loads (default)
  - EU minimum
  - Germany
  - France
  - UK
  - custom height and overhang
- **Truck specs:** every limit can be edited, and the Lohr code for each position can be added. Changes are saved in the browser (localStorage).

## Where the numbers come from

- **Position limits** come from an analysis of 10 real Eurolohr 200 low-cab loads. Each position allows the largest car actually loaded there, so the limits are on the safe side, not the physical maximum.
- **Payload defaults** (truck about 6.9 t, trailer about 9.8 t) come from used listings of 2017–18 Lohr EHR 200 C2 units. Check them against the truck's registration papers.
- **Route rules:**
  - EU Directive 96/53/EC: 4.00 m height, 2.55 m width, 18.75 m road train.
  - ECG (Association of European Vehicle Logistics) papers on loaded length: Germany 20.75 m with 0.5 m front and 1.5 m rear overhang; France 20.35 m with 1.6 m rear and no front overhang; UK no extension beyond 18.75 m.
- **Car sizes** in `cars-db.js` are approximate base-version figures. Check them.

## Known limits

- Lohr does not publish deck lengths, position heights or stacker tilt ranges for the Eurolohr 200, so the planner relies on recorded loads instead of exact geometry.
- Route rules are applied on top of the overhang already in the recorded loads. Set "Front/Rear overhang in your loads" under Truck specs. While they are 0, Germany and France may allow more length than is real.
- Only the Eurolohr 200 is modelled. Other models are not in yet: the 3-car and 4-car body versions, the D deck, Kässbohrer and Rolfo.

## Roadmap

1. **Measure a real Eurolohr 200 (EHR 200 C2).** For each position: deck length, lowest and highest height, tilt range and overlap points. Record the Lohr codes for every position.
2. **Collect more real loads (50+)**, with what the driver changed, and use them as test cases.
3. **Add truck profiles:** Eurolohr 300 and 600, Kässbohrer, Rolfo.
4. **Multi-truck planning** from an order list, plus a printable loading sheet for the driver.
5. **The wider app:**
   - per-car damage inspection
   - digital handover (ePOD/eCMR, ahead of EU eFTI in July 2027)
   - invoicing for small car-transport subcontractors

## Related

Lohr's own loading app, ILFY, covers Lohr trailers only. This planner aims to be multi-brand and to connect with the handover side.
