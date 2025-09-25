# Kebab O'Clock – Menu Management & Order API Guide

This guide explains how to manage the menu items for the **Kebab O'Clock website** by editing the associated JSON files, and how the front‑end interacts with the backend API.

---

## 1. Data Structure

The website's content is managed through several JSON files. These files must all be located in the same directory as `index.html`.

### 1.1 Menu Data

Each file represents a menu category and is loaded dynamically by the website.

**List of Menu JSON Files:**

*   `beefy-tastic.json`
*   `chicken-menu.json`
*   `wrap-tastic.json`
*   `don-shawarma.json`
*   `grilled-tastic.json`
*   `wings-tenders.json`
*   `veggie-tastic.json`
*   `drinks-milkshakes.json`
*   `addons-sauces.json`
*   `loaded-fries.json`

### 1.2 Pickup Times Data (`pickup-times.json`)

A new file, `pickup-times.json`, is used to dynamically load the available pickup slots for collection orders.

**Example Format:**

```json
{
  "times": ["18:00", "18:15", "18:30", "19:00"]
}
```

⚠️ **Important:** Images for menu items must be stored in the `images/` folder.

---

## 2. Understanding a Menu Item JSON Object

Example of a menu item from `wings-tenders.json`:

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

*   **name**: The display name of the menu item. *(string)*
*   **description**: A short description shown under the name. *(string)*
*   **image**: The path to the item's image. Must exist in the `images/` folder. *(string)*
*   **tags**: Used for categorization and special formatting.
    *   The first tag in the array is displayed as a badge on the menu card.
    *   A tag of `"D"` formats the image for a drink can (centered).
    *   A tag of `"M"` formats the image for a milkshake (scaled to show the full image).
*   **mealPriceModifier**: The additional cost to make the item a meal (adds Chips & a Drink). *(decimal)*
*   **options** *(optional)*: An array of variations for the item (e.g., size, flavor).
    *   If all options have a price, they appear as buttons.
    *   If any option lacks a price, they appear as a dropdown list.
*   **showSlider**: Legacy field. Should always be `false`.
*   **sauces**: If `true`, the customer can select sauces for this item.
*   **vegetables**: If `true`, the customer can select vegetables for this item.

---

## 3. Image Rules

*   **Size**: Recommended 1200 × 1200 pixels.
*   **Format**: `.jpg` or `.png`.
*   **Location**: Must be placed inside the `images/` folder.
*   **Naming**: The filename in the `image` field is case-sensitive and must match the actual file exactly.

---

## 4. Editing Workflow

1.  **Choose** the correct JSON file for the menu category you're editing.
2.  **Copy** an existing menu item object to use as a template.
3.  **Update** the fields (`name`, `description`, `image`, `price`, `options`, etc.).
4.  **Save** the JSON file.
5.  **Upload** the new image (1200×1200 px) to the `images/` folder.
6.  **(Optional but Recommended)** Validate your JSON syntax using a tool like [jsonlint.com](https://jsonlint.com).
7.  **Refresh** the website to see your changes.

---

## 5. Front-End Features & Checkout Flow

### 5.1 Cart Enhancements

*   A **Clear Cart** button has been added to empty the entire order.
*   The item modal now includes a **quantity selector**, allowing customers to choose between 1 and 10 of a single item configuration.

### 5.2 Checkout Improvements

The **“Place Order” button is disabled by default** and only becomes active when all of the following conditions are met:
1.  An **Order Type** has been selected (currently only “Collection”).
2.  All **Customer Details** are filled out (Full Name, Phone Number, Email, and Card Details).
3.  A **Pickup Time** has been chosen from the available slots.

The button's text dynamically updates to show the total price of the order (e.g., "Place Order – £15.50").

---

## 6. Order Payload Structure

When a user places an order, the front-end sends a structured JSON object to the backend. The menu JSON structure itself is unchanged, but the order payloads have been expanded.

### Key Payload Fields

*   **`selectedOption`**: If an item has options (e.g., "10 piece"), the chosen option is included here.
*   **`selectedSauce`**: The selected sauce is included.
*   **`selectedVegetables`**: An array of selected vegetables.
*   **Meal Upgrade**: If a user upgrades to a meal, a **separate line item** named "Chips & Drink" is added to the order, using the `mealPriceModifier` value as its price.

### Payload Examples

#### Example 1: Chicken Tenders (10-piece) with BBQ sauce, no meal

```json
{
  "deliveryType": "collection",
  "pickupTime": "18:30",
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

#### Example 2: Chicken Tenders (10-piece) with BBQ sauce, upgraded to a meal

```json
{
  "deliveryType": "collection",
  "pickupTime": "19:00",
  "customer": {
    "fullName": "John Doe",
    "phone": "123-456-7890",
    "email": "john@example.com",
    "specialInstructions": ""
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

---

## 7. Backend API Specification

### 7.1 API Base URL

The API endpoint is configured via the `API_BASE` variable in `index.html`. This controls whether the site runs in demo mode or sends orders to a live server.

```javascript
// Set to your server's URL to enable backend integration
// Set to '' (empty string) to run in demo mode
const API_BASE = 'https://api.kebaboclock.com';
```

*   **Live Mode (`API_BASE` is set):** Real orders are sent to `${API_BASE}/orders`. The confirmation modal displays the actual response message from the server.
*   **Demo Mode (`API_BASE = ''`):** No API call is made. The site shows a fake confirmation message after a 2-second delay.

### 7.2 Endpoints

#### `POST /orders`

This is the primary endpoint for receiving new orders.

*   **Request Body**: A JSON object matching the structure described in **Section 6**.
*   **Successful Responses (200 OK)**:
    *   **Approved:** The order was accepted by the backend.
      ```json
      {
        "status": "approved",
        "message": "Your order has been accepted. It will be ready for pickup at 19:30."
      }
      ```
    *   **Denied:** The order was received but could not be fulfilled (e.g., restaurant is too busy).
      ```json
      {
        "status": "denied",
        "message": "We are currently too busy to accept new orders. Please try again later."
      }
      ```
*   **Error Response (e.g., 400 or 500)**:
    *   **Error:** A server or validation error occurred.
      ```json
      {
        "status": "error",
        "message": "Could not process your order due to a technical issue. Please contact the store directly."
      }
      ```

### 7.3 CORS

The backend server must be configured with Cross-Origin Resource Sharing (CORS) headers to permit `POST` requests from the website's domain (e.g., your Vercel deployment URL).

---

## 8. Deploying Changes to Vercel

The website is configured for automatic deployment from its GitHub repository.

*   **Editing on GitHub**: Committing changes directly on the GitHub website will automatically trigger a new deployment on Vercel.
*   **Editing Locally**: If you work on the files locally, `git pull` to get the latest version, make your changes, then `git commit` and `git push` them to the repository. Vercel will then redeploy.

You can monitor deployment status and check for any build errors in your Vercel dashboard.

---

## 9. Backups & Support

*   **Always back up the JSON files** before making significant changes.
*   If you introduce a syntax error that breaks the menu, revert to a backed-up version of the file.
*   The client is responsible for regular menu maintenance. The developer is available for support with structural changes or bug fixes.
