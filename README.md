# Movewell Wellness Center — Website + Staff System (Phase 1)

Public website with live pricelist and booking requests, plus a staff app for client records,
visits (services, vitals, IV details, payment) and daily sales.

```
movewell/
├── index.html          Public landing page (services menu, booking form, hours, map)
├── staff.html          Staff app (login required)
├── config.js           Supabase URL + anon key (edit this)
├── assets/             Logo icon, favicon, photos (from the brochure)
└── supabase/schema.sql Database: tables, security rules, functions, 2026 pricelist
```

## Setup (about 15 minutes)

1. **Create a Supabase project** (free tier is enough to start). Region: Singapore (`ap-southeast-1`).
2. **Run the schema.** Supabase → SQL Editor → New query → paste all of `supabase/schema.sql` → Run.
   It creates the tables and loads the 2026 pricelist. Safe to run again.
3. **Create the owner's login right away.** Authentication → Users → Add user → Create new user,
   tick *Auto Confirm User*. **The first account created becomes Admin.**
4. **Turn off public sign-ups.** Authentication → Sign In / Providers → disable *Allow new users to sign up*.
   Staff accounts are added by the admin from the dashboard (step 3 method), never self-registered.
5. **Edit `config.js`** with the Project URL and the anon / publishable key
   (Project Settings → API). The anon key is meant to be public; Row Level Security protects the data.
6. **Deploy to GitHub Pages.** Push the folder to a repo → Settings → Pages → Deploy from branch `main` / root.
   Website: `https://<user>.github.io/<repo>/` · Staff: `.../staff.html` (also linked in the footer).
7. Sign in at `staff.html`, then:
   - **Staff** → add therapists and doctors (therapists don’t need a login; doctors: add PRC no.)
   - **Website info** → confirm phone numbers, Facebook link, “Message us” link
   - **Services & prices** → set Hydro Therapy’s price

## Roles

| Role | Can do |
|---|---|
| Admin | Everything: sales, prices, staff, website info, delete |
| Front desk | Record visits, clients, bookings; see sales; void visits |
| Therapist / Doctor | Record visits and client health notes |
| Pending | Nothing until the admin assigns a role |

## What’s inside

- **Visits** — client, services done (with who did each one), vitals before/after (BP, HR, SpO₂, pain,
  temp, RR, weight), IV details (doctor, dose, lot no., site, reaction), notes, discount (SC/PWD 20%, promo % or ₱),
  payment method, amount received, change/balance. Totals are recomputed on the server. Visits are voided, never deleted.
- **Health warnings** — flagged conditions and allergies show when recording a visit; extra alert for
  G6PD + vitamin C drips and kidney disease + IV. Out-of-range vitals are highlighted.
- **Sales** — collected, billed, unpaid, discounts; by payment method, category, staff, service; 14-day chart;
  printable sales report and CSV export for any date range.
- **Clients** — profile, health screening, consent record (Data Privacy Act), vitals history, visit history, printable record.
- **Bookings** — website requests land as Pending; confirm, start visit (auto-matches client by mobile no.), no-show, cancel.
  Messenger / phone bookings can be added manually.
- **Website** — menu, prices, hours and contact load live from the database; if the database can’t be reached,
  the built-in 2026 pricelist is shown.

## Notes for the owners

- The printed receipt is an **acknowledgment receipt**, not a BIR official receipt / invoice.
- Brochure contact number **+63 994631329** is missing a digit, so only +63 927 557 3040 is shown. Add the correct one in Website info.
- Website photos were cropped from the brochure PDF and are low resolution. Replace `assets/therapist.jpg`
  and `assets/chair.jpg` with original photos (same filenames) for a sharper site.
- “Message us” uses the link from the brochure QR code. Replace it with their m.me link if preferred.

## Next phases

- **Phase 2:** promo packages / bundles with session balances, booking calendar view
- **Phase 3:** commission (per service, fixed or %) and payroll — `visit_items.staff_id` already records who did each service
