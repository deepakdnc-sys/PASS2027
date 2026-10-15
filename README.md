# Swarnkar Garba Mahotsav 2026 — Digital Pass Website

Luxury maroon, black and gold Garba-themed responsive website with:
- Visitor booking form (name, mobile, ₹200 amount reference, UTR, payment screenshot)
- QR ticket generated for each booking
- Ticket status remains **INACTIVE** until admin approval
- Retrieve ticket by UTR
- Admin panel to view booking details and payment screenshots, approve/reject passes
- Gate scanner panel with camera scanning + manual pass-ID lookup
- Gate staff records 1–4 people per approved QR; total usage cannot exceed 4
- Download/print ticket for saving as PDF on a phone
- Help and support: Deepak Soni, 8385963066, Moti Chowk, Jodhpur

## Important: demo mode vs live production

**This downloaded project runs in DEMO MODE.** It saves bookings to the current browser's `localStorage`. This is useful for testing the interface, but it is **not a shared database**: another phone/computer will not see those records, and clearing browser data deletes them. Payment screenshots are stored locally in that browser. Do not accept real public bookings until a shared backend is configured.

For the website to work securely across visitor phones, admin computers and gate scanners, connect a production database such as Supabase and implement server-side authorization and atomic entry counting. A client-only HTML/JS site cannot securely protect admin passwords, verify payments, share records across devices, or prevent concurrent scans. The included browser password is only a demo gate, not security. Before deployment:
1. In `app.js`, replace `CHANGE_THIS_ADMIN_PASSWORD` with a strong temporary demo password if testing locally; use real server-side authentication for production.
2. The gate password is `ADMIN` per request. Replace it before public deployment and enforce scanner access server-side.
3. For production, never put a Supabase service-role key in frontend code. Use Supabase Auth + Row Level Security and Edge Functions/server-side API for booking creation, approval, QR validation and entry recording.
4. The payment screenshot upload must go to a private storage bucket. Use signed URLs or an authenticated admin-only endpoint; never expose payment screenshots publicly.
5. Configure the production site for HTTPS. Camera scanning generally requires HTTPS (or localhost).
6. Verify each UTR in the payment app/bank before approving a pass. A screenshot alone is not proof of payment.

## Run locally
Open `index.html` in a browser, or serve the folder over localhost:
```bash
python -m http.server 8000
```
Then open `http://localhost:8000`.

The QR and camera libraries are loaded from public CDNs, so an internet connection is needed for those features. On some browsers, camera access may require localhost or HTTPS.

## Deploy to GitHub Pages
1. Create a GitHub repository, e.g. `sgm2026-digital-pass`.
2. Upload all project files and folders.
3. Go to **Settings → Pages**.
4. Under build/deploy, select **Deploy from a branch**, branch `main`, folder `/ (root)`, then Save.
5. Open the published URL over HTTPS.

GitHub Pages is static hosting only. It does not provide a shared database or secure backend. The current demo mode is only for preview/testing and must not be used for real multi-device event entry.

## Test checklist
- Submit a test booking using a dummy UTR and image.
- Retrieve it using that UTR.
- Open Admin / Scanner → Admin Panel, enter your configured demo password, then approve the test booking.
- Open Gate Scanner and enter `ADMIN`, scan the QR or type its pass ID.
- Try recording 4 entries, then confirm a fifth is denied.
- Verify pending and rejected tickets cannot be admitted.

## Event settings
- Event: Swarnkar Garba Mahotsav 2026
- Booking amount: ₹200 per QR pass
- Maximum: 4 people per QR code
- Support: Deepak Soni — 8385963066
- Location: Moti Chowk, Jodhpur

## Production launch requirements
This starter is intentionally transparent about its limits rather than pretending browser-only storage is a working shared booking database. For live launch, add a backend before advertising the URL. Recommended data model:
- `passes`: `id`, `pass_id`, `name`, `mobile`, `utr`, `screenshot_path`, `status`, `max_entries`, `used_entries`, `created_at`, `approved_at`
- `entry_logs`: `id`, `pass_id`, `count`, `gate_staff_id`, `created_at`
- Enforce unique UTR and pass ID; status approval and entry recording only via authorized server-side functions.
