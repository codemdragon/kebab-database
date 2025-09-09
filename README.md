#  Kebab O'Clock – Menu Management Guide

This guide explains how to manage the menu items for the **Kebab O'Clock website** by editing the associated JSON files.

---

## 1. Menu Data Structure
The menu items are organized into several JSON files, each representing a category. These files are loaded dynamically by the website to display the menu.

**List of JSON Files:**
- `beefy-tastic.json`
- `chicken-menu.json`
- `wrap-tastic.json`
- `don-shawarma.json`
- `grilled-tastic.json`
- `wings-tenders.json`
- `veggie-tastic.json`
- `drinks-milkshakes.json`
- `addons-sauces.json`
- `loaded-fries.json`

⚠️ **Important:** All JSON files must be located in the same directory as `index.html`.

---

## 2. Understanding a Menu Item JSON Object

Example:
```json
{
  "name": "Chicken Tenders or Wings",
  "description": "Chicken wings or tenders with any 1 sauce",
  "image": "Chicken Tenders or Wings.jpg",
  "tags": ["Wings/tenders", "chicken","D"],
  "mealPriceModifier": 2.50,
  "options": [
    {"name": "5 piece", "price": 5.00},
    {"name": "10 piece", "price": 9.00},
    {"name": "15 piece", "price": 14.00},
    {"name": "20 piece", "price": 18.00}
  ],
  "showSlider": false,
  "sauces": true,
  "vegetables": false
}
```

### Field Explanations
- **name**: Name of the menu item. *(string)*  
- **description**: Short description displayed under the name. *(string)*  
- **image**: Filename of the image (must exist in the same folder). *(string)*  
- **tags**:
  - Used for categorization and filtering.
  - First tag = badge on menu card.
  - Special tags:
    - `"D"` → Drinks (centered images, scaled properly for cans).
    - `"M"` → Milkshakes (image scaled fully outward to show full picture on all screen sizes).
- **mealPriceModifier**: Extra cost when upgrading to a meal. *(decimal)*  
- **options**: *(optional)* variations of the item.
  - **Special behavior:**
    - If *all options* have prices → price shows on the **button**.
    - If *not all options* have prices → price shows in the **dropdown**.
    - Example: Samosas (Chicken/Lamb) both priced → shows on button.
- **showSlider**: Always set to `false` (legacy field).
- **sauces**: `true`/`false` – whether customer can choose sauces.
- **vegetables**: `true`/`false` – whether customer can choose vegetables.

---

## 3. Image Rules

- **Size**: All images must be **1200 x 1200px**.
- **Format**: `.jpg` or `.png`.
- **Naming**: Must match exactly the `image` field in JSON (case-sensitive).

**Special Cases:**
- **Drinks** (`"D"` tag): Canned drinks centered properly.
- **Milkshakes** (`"M"` tag): Full image always shown, scaled outward.
- **All Other Items**: Positioned according to Website Manager’s preferences.

---

## 4. Editing Workflow

1. Identify which JSON file contains the category.
2. Copy an existing item as a template.
3. Edit `name`, `description`, `image`, `tags`, `price/options`.
4. Save the JSON file.
5. Add the image file (1200x1200px) to the same folder.
6. Validate the JSON (use [jsonlint.com](https://jsonlint.com)) to avoid syntax errors.
7. Refresh the website to see changes.

---

## 5. Examples

**Drink Example (Coke Can):**
```json
{
  "name": "Coke Can",
  "description": "Chilled Coca Cola (330ml)",
  "image": "Coke Can.jpg",
  "price": 1.50,
  "tags": ["Drinks", "D"],
  "mealPriceModifier": 0.00,
  "showSlider": false,
  "sauces": false,
  "vegetables": false
}
```

**Milkshake Example:**
```json
{
  "name": "Chocolate Milkshake",
  "description": "Thick chocolate shake with whipped cream",
  "image": "Chocolate Milkshake.jpg",
  "price": 3.50,
  "tags": ["Milkshake", "M"],
  "mealPriceModifier": 0.00,
  "showSlider": false,
  "sauces": false,
  "vegetables": false
}
```

**Options Example (Samosa):**
```json
{
  "name": "Samosas (3 Pieces)",
  "description": "Crispy samosas",
  "image": "Samosas.jpg",
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

## 6. Backups & Support

- Always **keep a backup** of JSON files before making changes.
- If something breaks, restore the previous backup.
- The Developer can provide guidance if needed, but **ongoing menu management is the Client’s responsibility**.

---
