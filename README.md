# Forget Me Not

A static fundraising site. 100% of what customers pay goes to the Alzheimer Society of Canada.

| Page | Live URL | Who it's for |
| --- | --- | --- |
| `index.html` | https://ivankirischian.github.io/forget-me-not/ | Anyone who types the bare URL. Shows the brand, charity line and contact details, with no product links. |
| `dentists/index.html` | https://ivankirischian.github.io/forget-me-not/dentists/ | Dentist flyer QR code |
| `realtors/index.html` | https://ivankirischian.github.io/forget-me-not/realtors/ | Realtor flyer QR code and the sample token |

The site is plain HTML with one shared stylesheet (`assets/css/styles.css`). There's no build step. Edit a file, commit and push to `main`, and GitHub Pages republishes in about a minute.

The dentist and realtor pages must never link to each other.

Every placeholder is marked in the HTML with a `PLACEHOLDER` comment. Search the files for `PLACEHOLDER` to find them all.

---

## Change prices or the deadline

Prices and the deadline are written out as plain text. Each page holds its own copy, so edit each file that mentions them:

- **Squeezers** (`dentists/index.html`): the hero note (`$1 each · Order by Nov 27`), the Pricing section (the two `.price` boxes and the "$1 each, in packs of 25 or 50" line), the deadline line, and the FAQ. Also update the `<meta name="description">` and `og:description` tags at the top.
- **NFC tokens** (`realtors/index.html`): the hero note, the Pricing section, the deadline line, the FAQ, and the meta and `og:` tags.
- **Preview images** (`assets/img/og-*.png`) also show the prices. If prices change, regenerate them (see "QR codes and preview images" below) or edit them in any image editor.

Before you print, check that the prices, deadline and charity wording match the flyers exactly.

## Swap the product photos

1. Save each photo in `assets/img/`, for example `squeezer.jpg` and `token.jpg`. A landscape 4:3 photo about 1200×900 px works best (the page crops it to 4:3). Keep each file under about 300 KB so it loads fast on phones.
2. In `dentists/index.html`, find the `PLACEHOLDER: product photo` comment. Change `src="../assets/img/squeezer-placeholder.svg"` to `src="../assets/img/squeezer.jpg"`, and update `width` and `height` to the photo's real pixel size.
3. Do the same in `realtors/index.html` with `token-placeholder.svg` → `token.jpg`.
4. Rewrite the `alt="..."` text so it describes what the photo actually shows.

## Add your photo

Each page has a "Hi, I'm Ivan Kirischian" box, which shows a flower placeholder until you add a photo.

1. Save a square photo of yourself (about 400×400 px, face centred) as `assets/img/ivan.jpg`.
2. Search for `PLACEHOLDER: photo of Ivan` in `index.html`, `dentists/index.html` and `realtors/index.html`. In each one, change `ivan-placeholder.svg` to `ivan.jpg`. The page crops the photo into a circle.

## How people order and pay

**Dentists:** you drop by each clinic and leave a flyer plus a sample squeezer printed with that clinic's name (make it with `add_text.py`, see "Sample squeezers" below), then leave. The dentist page is written for someone holding that sample: it says the sample is theirs to keep, that orders are printed to match it, and that a mockup is only needed if they want changes. Payment is due on delivery.

**Realtors:** most sales still start with a visit or a flyer. The realtor page keeps the mockup step and the "already ordered in person" note.

Each product page offers these ways to order:
- the order form (or the email button until Tally is set up),
- a **Text Ivan** button, which opens a text to 415-456-1579 with a short message already filled in,
- (realtors only) ordering in person. The page tells people who already ordered with you that they don't need to fill in the form.

Payment is **cash on delivery or Interac e-Transfer**. You deliver every order by hand. If any of that changes, update the "How ordering works" steps (dentists), the line under Pricing (realtors), and the "How do I pay?" and "How is it delivered?" FAQ entries on both pages.

## Sample squeezers

The 3D files live outside this repo, in `C:\Users\Ivan\Documents\forget-me-not-3d\`. To make a clinic's sample:

```
python add_text.py "Bathurst Finch Dental Office" "416-229-2444"
```

This writes `squeezers\<clinic name>.3mf` (phone number on top, clinic name below, raised 0.5 mm), a single-colour `.stl` and a preview PNG. Check the spelling in the preview, and confirm the phone number with the clinic if it came from an online directory. For two colours, change filament at Z = 2.6 mm.

## Add the Tally order forms

Until the forms are in, each page shows an "Order by text or email" box with pre-filled email and text buttons, so the QR codes already work.

1. Create two free forms at https://tally.so.
   - **Dentists:** Clinic name; Pack size (25 or 50); Contact name; Email; Phone; Same as the sample? (Yes / I'd like changes); Changes (short answer, shown only if they want changes); Delivery notes. You don't need a "How did you hear" question here, because every clinic got a sample.
   - **Realtors:** Name; Brokerage; Email; Phone; Link to program (URL); Logo (file upload); Token colour; Number of packs (10 tokens each); How did you hear about this?
   - On the realtor form, make **How did you hear about this?** a required multiple-choice question with these options: *Ivan visited and we talked*, *Found a flyer Ivan left*, *Someone told me*, *Other*. Optionally add a short-answer field, "Visit # on your flyer (if any)".
2. In each form, go to Settings > Notifications and turn on self email notifications so each submission is sent to ivan.kirischian@gmail.com.
3. Go to Share > Embed > **Standard**. Turn on *Dynamic height*, *Hide form title* and *Transparent background*, then copy the code.
4. In each page, find `PLACEHOLDER: TALLY FORM`. Paste the code there, then delete the whole `<div class="form-fallback"> … </div>` block below it.
5. Submit one test order on each page and check that the email arrives.

## Add the fundraising page link

When you've registered at alzheimer.ca/Fundraiser, find `PLACEHOLDER: fundraising page link` in all three HTML files and replace the line below it with:

```html
<p><a href="YOUR-FUNDRAISING-PAGE-URL">See our fundraising page</a></p>
```

## Update the "raised so far" figure

Each page has this line in its charity section, hidden until you're ready to show it:

```html
<!-- RAISED-SO-FAR: ... -->
<p class="raised" hidden>Raised so far: <strong>$0</strong></p>
```

Change `$0` to the current total and delete the word `hidden`. Search for `RAISED-SO-FAR` to find it in `index.html`, `dentists/index.html` and `realtors/index.html`, and use the same figure on all three.

## Track which visits turn into orders

For **dentists**, every visit is the same (a flyer plus a named sample), so the clinic name on each order tells you which visit it came from. Log the date you dropped off each sample, and mark the row "ordered" when an order comes in. That shows how many samples it takes to get an order, and how long clinics take to reply. For **realtors**:

1. Keep a simple visit log in a notebook or spreadsheet, with one row per visit: **visit #**, date, office name, and whether you *talked to someone* or *left a flyer*.
2. When you leave a flyer, write the date and the visit # in the blank at the bottom of it.
3. Every order says how the customer heard about it (Tally question, email template, or you ask in person) and names the clinic or brokerage. Match each order to the log and mark that row "ordered".

After a week you'll see which approach works better (talking vs. leaving a flyer, and dentists vs. realtors), so you can spend your remaining visits there before the Nov 27 deadline.

## Printable flyers

The flyers are **not** in this repo, so they never become public pages. They live in `C:\Users\Ivan\Documents\forget-me-not-flyers\`:
- `dentists-flyer.pdf` and `realtors-flyer.pdf`: letter size, ready to print.
- `dentists.html` and `realtors.html`: the source for each flyer.
- `make_flyers.py`: rebuilds both, using the QR codes and product images from this repo.

To change a flyer, edit its text in `make_flyers.py` and run `python make_flyers.py` from that folder. It needs Google Chrome. If you swap the product photos on the site, update the flyer image too. In `make_flyers.py`, change `squeezer-placeholder.svg` and `token-placeholder.svg` to your photo filenames (.jpg or .png work).

Each flyer has the price, the deadline, the charity line, a large QR code, "text Ivan" with your number, and a "Visited by Ivan on ____ · Visit # ____" line for your log. Print a test page and scan the QR code with a phone before printing a batch.

## QR codes and preview images

- `assets/qr/dentists.svg` and `assets/qr/realtors.svg` are for the flyers (vector, so they print sharp at any size). PNG copies sit next to them. Both have been test-decoded and open:
  - https://ivankirischian.github.io/forget-me-not/dentists/
  - https://ivankirischian.github.io/forget-me-not/realtors/
- They use high error correction, so they still scan if a corner gets scuffed. Print them at least 2 cm (0.8 in) wide, and keep a white margin around them.
- If the site address ever changes (for example, a custom domain), regenerate the QR codes **before** printing, and update the `canonical` and `og:` URLs at the top of each page. Search for `SITE URL`.
- The link preview images are `assets/img/og-home.png`, `og-dentists.png` and `og-realtors.png` (1200×630).

## Enable GitHub Pages

1. On GitHub, open the repo and go to **Settings > Pages**.
2. Under **Build and deployment**, set **Source** to *Deploy from a branch*.
3. Set **Branch** to `main` and the folder to `/ (root)`, then click **Save**.
4. Wait about a minute, then refresh. The page shows "Your site is live at https://ivankirischian.github.io/forget-me-not/".
5. You can watch each deploy in the repo's **Actions** tab ("pages build and deployment").

## Accessibility notes

- Text and button colours meet WCAG AA. The lowest pair is muted grey on light blue, at 6.6:1.
- Body text is 17px. The layout is a single column on phones, up to 720px wide on desktop.
- Every content image has alt text, and decorative flowers use `alt=""`.
- A skip link jumps to the order form, and the FAQ uses native `<details>`, so it works with a keyboard.
- Don't add the Alzheimer Society logo anywhere on the site.

## Launch checklist

- [ ] Both product pages work on iPhone and Android, with no sideways scrolling at 375px wide
- [ ] A test submission on each form arrives by email
- [ ] Each QR code opens the right page
- [ ] Neither product page links to the other
- [ ] Prices, deadline and charity wording match the flyers exactly
