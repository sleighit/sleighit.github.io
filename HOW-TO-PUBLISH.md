# Sleigh It — your product page

Everything the site needs is in this `website` folder: `index.html` plus the `images` folder.

## 1. Add your details (one time)

Open `index.html` in any text editor (TextEdit works, set to plain text) and near the bottom find:

```js
const SHOP = {
  whatsapp: "",     // e.g. "919876543210" — country code + number, digits only
  instagram: "",    // e.g. "sleighit"
  currency: "₹",
};
```

**The WhatsApp number is required for ordering.** Customers add items to their cart, then tap **Send order on WhatsApp**. WhatsApp opens to your chat with the order already typed, like this:

```
Hi, I'm Anna!
I'm interested in these items:

1. NAT-01 – Classic Nativity Set × 2
2. ORN-03 – Wreath Mirror Ornaments × 1

Total items: 3
```

Prices go in the `PRODUCTS` list just below it: change `price: ""` to `price: 899`.
Leave it as `""` to show "Ask for price". Add `soldOut: true` to mark an item sold out.

Each product has a `code` that matches the **Item Code** in your stock sheet (C0001, C0002…).

## Live stock & prices from Google Sheets

Every time someone opens the page, it reads the sheet set in `SHOP.sheet` and updates:
- **Retail Price**: a number is shown as the price and the item can be added to the cart. If it's blank or "-", or the item isn't in the sheet, the item can't be added to the cart. It shows an **Ask price** button instead, which opens WhatsApp with "What is the price of <name>, <code>?".
- **In Stock**: 0 marks the item "Sold out". At or below `lowStock` (3) it shows "Only N left". Customers can't add more than you have.

The sheet needs a header row with columns named **Item Code**, **In Stock** and **Retail Price**, in any order. Other columns are ignored.
It must be shared as **Anyone with the link → Viewer**. Anyone with the link can read **every tab and column**, so the shared sheet must not contain wholesale prices or anything else private.

Safe setup: create a separate sheet for the website, and in cell A1 put:

```
=QUERY(IMPORTRANGE("1RJ9jaE2dfEjR4V7NpzrOsQ7OQF5CKetvfPspfbVRjbo", "Stock!A1:F"), "select Col1, Col3, Col5", 1)
```

This copies just Item Code, In Stock and Retail Price, and they update automatically. Click **Allow access** the first time. Share only this new sheet, then put its id and gid in `SHOP.sheet`.

If the sheet can't be reached, the site still works and shows "Ask for price" for everything.

The cart is saved on the customer's phone, so they can close the page and come back later.

## Where it's hosted now

The site is live on GitHub Pages: **https://reolraju.github.io/sleigh-it/**
(code: https://github.com/reolraju/sleigh-it, on the personal `reolraju` account).

- **Price and stock changes:** just edit the Google Sheet. No upload needed.
- **Changes to the page itself** (new products, photos, text): commit and push to `main`. GitHub republishes in about a minute.
  You can also edit `index.html` on github.com (pencil icon), or drag new photos into the `images` folder using **Add file → Upload files**.

## 2. Alternative: Netlify Drop

1. Go to https://app.netlify.com/drop
2. Drag the whole `website` folder onto the page.
3. You get a link like `https://something-random.netlify.app` straight away. Share it on WhatsApp or Instagram, or print it as a QR code.
4. To keep the site and get a nicer name (e.g. `sleighit.netlify.app`), sign up for a free account and rename it under **Site settings**.

To update later (new prices, sold-out items): edit `index.html`, then go to the site's **Deploys** tab in Netlify and drag the folder in again. The link stays the same.

Other free options: GitHub Pages or Cloudflare Pages. Both work, but they take a few more steps to set up.

## Adding a new product

1. Put a photo in `images/` (ideally under 1 MB, square or portrait).
2. Copy one line in `PRODUCTS`, then change its name, category, description and photo file name.
