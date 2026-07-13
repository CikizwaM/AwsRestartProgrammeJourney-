# 🏗️ Website Breakdown

<strong>Static site built for Cloud Fitness Gym, deployed to Amazon S3.</strong>

## 📄 Pages

🏠 <strong>Home</strong>
<img src="./Images/Website/1.%20Home%20page.png" width="1760">
<strong>Home, "why choose us" features and featured classes preview.</strong>

---

💳 <strong>Plans</strong>
<img src="./Images/Website/2.%20Plans.png" width="1792">
<img src="./Images/Website/3.%20Plans%20continued.png" width="1747">
<strong>Pricing tiers (Student / Professional / Premium), day pass, corporate & senior rates.</strong>

---

🏋️ <strong>Classes</strong>
<img src="./Images/Website/4.%20Classes.png" width="1716">
<strong>Class descriptions + filterable weekly schedule table (by day)</strong>

---

📅 <strong>Booking</strong>
<img src="./Images/Website/5.%20Booking.png" width="1690">
<strong>Class booking form which submits to confirmation.html</strong>

---

📞 <strong>Contact</strong>
<img src="./Images/Website/6.%20Contact%20Us.png" width="1697">
<strong>Membership sign-up form, gym contact details and map which submits to confirmation.html</strong>

---

✅ <strong>Confirmation</strong>
<img src="./Images/Website/7.%20Booking%20confirmation.png" width="1655">
<strong>Reads submitted form data from the URL and displays a booking/sign-up summary with a generated reference number</strong>

---

❌ <strong>Error</strong>
<img src="./Images/Website/8.%20Error.png" width="1693">
<strong>Custom 404, set as the S3 error document</strong>

---

## 🧩 Shared Components
- `styles.css` — single stylesheet covering layout, navbar, cards, forms, pricing grid, schedule table, and responsive breakpoints
- `main.js` — handles:
  - mobile nav toggle (hamburger menu)
  - scroll-based navbar background change
  - fade-in-on-scroll animation (feature/class cards, section titles)
  - class schedule day-filter tabs
  - confirmation page: reads query parameters from the booking/sign-up form submission and renders the details and reference number for the client's side.

## 📚 Design Notes
- No backend — booking and sign-up "submissions" are simulated via GET form parameters.
- Fully static, so it maps directly onto S3 static website hosting with no server required.
