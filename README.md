# Kebab O'Clock – Menu Management Guide

This guide explains how to manage the menu items for the **Kebab O'Clock website** by editing the associated JSON files.

---

## 1. Menu Data Structure

The menu items are organized into several JSON files, each representing a category. These files are loaded dynamically by the website to display the menu.

**List of JSON Files:**

* `beefy-tastic.json`
* `chicken-menu.json`
* `wrap-tastic.json`
* `don-shawarma.json`
* `grilled-tastic.json`
* `wings-tenders.json`
* `veggie-tastic.json`
* `drinks-milkshakes.json`
* `addons-sauces.json`
* `loaded-fries.json`

⚠️ **Important:** All JSON files must be located in the same directory as `index.html`.
Images for menu items are now stored in the `images/` folder, so the `image` field should point to `images/YourImageName.jpg`.

---

## 2. Understanding a Menu Item JSON Object

Example:

```json
{
  "name": "Chicken Tenders or Wings",
  "description": "Chicken wings or tenders with any 1 sauce",
  "image": "images/Chicken Tenders or Wings.jpg",
  "tags": ["Wings/tenders", "chicken", "D"],
  "mealPriceModifier": 2.50,
  "options": [
    { "name": "5 piece", "price": 5.00 },
    { "name": "10 piece", "price": 9.00 },
    { "name": "15 piece", "price": 14.00 },
    { "name": "20 piece", "price": 18.00 }
  ],
  "showSlider": false,
  "sauces": true,
  "vegetables": false
}
```

### Field Explanations

* **name**: Name of the menu item. *(string)*
* **description**: Short description displayed under the name. *(string)*
* **image**: Filename of the image (must exist in the `images/` folder). *(string)*
  e.g. `"images/Chocolate Milkshake.jpg"`
* **tags**:

  * Used for categorization and filtering.
  * First tag = badge on menu card.
  * Special tags:

    * `"D"` → Drinks (centered images, scaled properly for cans).
    * `"M"` → Milkshakes (image scaled fully outward to show full picture on all screen sizes).
* **mealPriceModifier**: Extra cost when upgrading to a meal. *(decimal)*
* **options** *(optional)*: Variations of the item.

  * **Special behavior:**

    * If *all options* have prices → price shows on the **button**.
    * If *not all options* have prices → price shows in the **dropdown**.
    * Example: Samosas (Chicken/Lamb) both priced → shows on button.
* **showSlider**: Always set to `false` (legacy field).
* **sauces**: `true`/`false` – whether customer can choose sauces.
* **vegetables**: `true`/`false` – whether customer can choose vegetables.

---

## 3. Image Rules

* **Size**: All images must be **1200 × 1200 px**.
* **Format**: `.jpg` or `.png`.
* **Location**: All images go inside the `images/` folder.
* **Naming**: Must match exactly the `image` field in JSON (case-sensitive), including the `images/` prefix.

**Special Cases:**

* **Drinks** (`"D"` tag): Canned drinks centered properly.
* **Milkshakes** (`"M"` tag): Full image always shown, scaled outward.
* **All Other Items**: Positioned according to Website Manager’s preferences.

---

## 4. Editing Workflow

1. Identify which JSON file contains the category.
2. Copy an existing item as a template.
3. Edit `name`, `description`, `image`, `tags`, `price/options`.
4. Save the JSON file.
5. Add the image file (1200×1200 px) to the `images/` folder.
6. Validate the JSON (use [jsonlint.com](https://jsonlint.com)) to avoid syntax errors.
7. Refresh the website to see changes.

---

## 5. Examples

### Drink Example (Coke Can)

```json
{
  "name": "Coke Can",
  "description": "Chilled Coca Cola (330ml)",
  "image": "images/Coke Can.jpg",
  "price": 1.50,
  "tags": ["Drinks", "D"],
  "mealPriceModifier": 0.00,
  "showSlider": false,
  "sauces": false,
  "vegetables": false
}
```

### Milkshake Example

```json
{
  "name": "Chocolate Milkshake",
  "description": "Thick chocolate shake with whipped cream",
  "image": "images/Chocolate Milkshake.jpg",
  "price": 3.50,
  "tags": ["Milkshake", "M"],
  "mealPriceModifier": 0.00,
  "showSlider": false,
  "sauces": false,
  "vegetables": false
}
```

### Options Example (Samosa)

```json
{
  "name": "Samosas (3 Pieces)",
  "description": "Crispy samosas",
  "image": "images/Samosas.jpg",
  "tags": ["Addons & Sauces"],
  "mealPriceModifier": 0.00,
  "showSlider": false,
  "sauces": false,
  "vegetables": false,
  "options": [
    { "name": "Chicken", "price": 2.50 },
    { "name": "Lamb", "price": 2.50 }
  ]
}
```

---

## 6. Order Payload Examples for Backend

These show how the front‑end sends orders to the backend with different combinations:

#### 1. Chicken Tenders 10-piece (BBQ sauce, no chips/drink):

```json
   {
  "deliveryType": "collection",
  "customer": {
    "fullName": "Jane Smith",
    "phone": "555-1234",
    "email": "jane@example.com",
    "specialInstructions": "No onions please"
  },
  "items": [
    {
      "name": "Chicken Tenders or Wings (10 piece) with BBQ",
      "price": 9.00,
      "quantity": 1,
      "selectedOption": "10 piece",
      "selectedSauce": "BBQ"
    }
  ],
  "total": "9.00"
}
```

#### 2. Chicken Tenders 10-piece with Chips & Drink (Pepsi):

```json
   {
  "deliveryType": "delivery",
  "customer": {
    "fullName": "John Doe",
    "phone": "123-456-7890",
    "email": "john@example.com",
    "specialInstructions": "Please ring the doorbell."
  },
  "items": [
    {
      "name": "Chicken Tenders or Wings (10 piece) with BBQ",
      "price": 9.00,
      "quantity": 1,
      "selectedOption": "10 piece",
      "selectedSauce": "BBQ"
    },
    {
      "name": "Chips & Drink (Pepsi)",
      "price": 2.50,
      "quantity": 1
    }
  ],
  "total": "11.50"
}
```

#### 3. Samosas (3 Pieces) – Chicken:

```json
  {
  "deliveryType": "collection",
  "customer": {
    "fullName": "Alice Brown",
    "phone": "555-9876",
    "email": "alice@example.com",
    "specialInstructions": ""
  },
  "items": [
    {
      "name": "Samosas (3 Pieces) (Chicken)",
      "price": 2.50,
      "quantity": 1,
      "selectedOption": "Chicken"
    }
  ],
  "total": "2.50"
}
```

#### 4. Samosas (3 Pieces) – Lamb with Chips & Drink (Coke):

```json
  {
  "deliveryType": "delivery",
  "customer": {
    "fullName": "Bob Green",
    "phone": "555-4321",
    "email": "bob@example.com",
    "specialInstructions": "Extra napkins"
  },
  "items": [
    {
      "name": "Samosas (3 Pieces) (Lamb)",
      "price": 2.50,
      "quantity": 1,
      "selectedOption": "Lamb"
    },
    {
      "name": "Chips & Drink (Coke)",
      "price": 2.50,
      "quantity": 1
    }
  ],
  "total": "5.00"
}
```
##Notes on the total feild:
* The total is calculated by summing (price × quantity) for all items in the cart.
* It is formatted as a string with two decimal places (e.g., "11.50").
* This field is included alongside the detailed items array in the order payload sent to the backend.
* Including the total helps with order validation, reporting, and simplifies backend processing.

**Key points for backend:**

* `selectedOption` can be a variant name (Chicken/Lamb) or portion size (10 piece). It comes directly from the JSON `options[].name`.
* `price` is always per-unit price from the chosen option (or base price if no options).
* If chips/drink is added, you’ll always see a second line `{ "name": "Chips & Drink (DrinkName)", "price": mealPriceModifier, "quantity": sameAsMainItem }`.
* If no chips/drink, there’s no second line.
* Sauces/vegetables are only on the main line.

---

## 7. Backups & Support

* Always **keep a backup** of JSON files before making changes.
* If something breaks, restore the previous backup.
* The Developer can provide guidance if needed, but **ongoing menu management is the Client’s responsibility**.

---

## 8. Deploying Changes to Vercel

The site is automatically deployed from this GitHub repository using Vercel.

* **If you edit files directly on GitHub**
  Save/commit your changes. Vercel will automatically pick up the new commit and redeploy within a minute or two.

* **If you work locally on your PC**

  1. Pull or clone the repo.
  2. Make your JSON/image changes.
  3. Commit and push to the `main` branch (or the branch Vercel is connected to).

  Vercel will build and deploy automatically after your push.

* **Check deployment status**
  In your Vercel dashboard you’ll see the deployment logs and the new version going live.

---

## 9. API Integration (Future-Proofing)

The front-end code is already set up to talk to a backend API when it becomes available:

```js
// In index.html
const API_BASE = ''; // e.g. 'https://api.yoursite.com'

// Helper to get file/API URL
function getJsonUrl(file) {
  return API_BASE ? `${API_BASE}/${file}` : file;
}
```

* **Current behaviour**: With `API_BASE = ''` the site loads the JSON files locally and does not send orders anywhere.
* **When backend is ready**: Set `API_BASE` to your API’s URL (for example `https://api.kebaboclock.com`).

  * Menu JSON will be fetched from the API instead of local files.
  * Orders will automatically be sent to `${API_BASE}/orders` via `POST`.

No other code changes are needed — just update the `API_BASE` value.






