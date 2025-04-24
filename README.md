
# 🛒 E-commerce Product Data Model

This project defines a comprehensive schema for managing products, variations, and attributes in an e-commerce platform. It supports dynamic filtering, variation handling (e.g., color and size), and flexible attribute assignment, making it ideal for diverse catalogs such as clothing, electronics, and accessories.

---

## 📐 Database Relationships

### 🔁 One-to-Many
- **Brand → Products**
- **Product Category → Products**
- **Product → Product Items**
- **Product → Product Variations**
- **Product Item → Product Images**
- **Size Category → Size Options**

### 🔁 Many-to-One
- **Product Item → Product**
- **Product Variation → Product**
- **Product Variation → Size Option**
- **Product Variation → Color**

### 🔀 Many-to-Many
- **Product ↔ Attributes** (via `product_attribute`)

---

## 🔄 Data Flow Overview

### 1️⃣ Customer Browsing Journey
- Users land on a **Product Detail Page (PDP)**.
- Variations (Color, Size) are shown using **Product Variations**.
- Customer selects a variation → Adds to cart.

### 2️⃣ Variation Selection Logic
- On size + color selection, query:
  - `product_variation`
  - Joined with: `color`, `size_option`

### 3️⃣ PDP Data Requirements
- **Product**: Name, Description, Base Price, Brand
- **Variations**: Available Sizes and Colors, Stock
- **Images**: Default and Variation-Specific
- **Dynamic Price** (based on selected variant)
- **Optional**: Ratings, Reviews

### 4️⃣ Image Display Logic
- Default images shown first.
- When a color is selected, show `product_image` for that `product_item`.
- If none found → fallback to default product images.

---

## 📈 Entity Data Flow

### ➕ Product Creation
- Linked to a `brand` and `product_category`
- Custom attributes via `product_attribute`
  - Attributes use: `attribute_type` & `attribute_category`

### 🔀 Variation Management
- One `product_variation` for each (size + color) combo
- Connects to `color` and `size_option`

### 💰 Purchasable Units
- `product_item`: A specific SKU of a product (with price + stock)
- Links to the main product and its variation

### 🖼 Image Handling
- Stored in `product_image`, linked to `product_item`

### 🧩 Attributes
- Custom product features like material or weight
- Stored in `product_attribute`

---

## 🧱 Table Definitions

### 1. `product`
| Column         | Description                          |
|----------------|--------------------------------------|
| `product_id`   | Primary Key                          |
| `productName`  | Product name                         |
| `brand_id`     | FK → `brand`                         |
| `category_id`  | FK → `product_category`              |
| `base_price`   | Base price                           |
| `description`  | Overview                             |

### 2. `brand`
| Column       | Description            |
|--------------|------------------------|
| `brand_id`   | Primary Key            |
| `brand_name` | Name (e.g., Nike)      |
| `description`| Optional brand details |
| `website`    | Brand URL              |

### 3. `product_category`
| Column        | Description                    |
|---------------|--------------------------------|
| `category_id` | Primary Key                    |
| `category_name`| E.g., Electronics, Clothing   |
| `description` | Category description           |

### 4. `product_item`
| Column           | Description                           |
|------------------|---------------------------------------|
| `product_item_id`| Primary Key                           |
| `product_id`     | FK → `product`                        |
| `sku`            | Unique identifier                     |
| `price`          | Final price                           |
| `stock_quantity` | Available stock                       |

### 5. `product_variation`
| Column           | Description                          |
|------------------|--------------------------------------|
| `variation_id`   | Primary Key                          |
| `product_item_id`| FK → `product_item`                  |
| `color_id`       | FK → `color`                         |
| `size_option_id` | FK → `size_option`                   |

### 6. `product_image`
| Column         | Description                      |
|----------------|----------------------------------|
| `image_id`     | Primary Key                      |
| `product_item_id` | FK → `product_item`           |
| `image_url`    | Image path or URL                |
| `alt_text`     | SEO / Accessibility description  |

### 7. `color`
| Column       | Description             |
|--------------|-------------------------|
| `color_id`   | Primary Key             |
| `color_name` | E.g., Crimson           |
| `hex_value`  | For UI display          |

### 8. `size_option`
| Column            | Description                 |
|-------------------|-----------------------------|
| `size_option_id`  | Primary Key                 |
| `size_name`       | E.g., S, M, L, 10.5         |
| `size_category_id`| FK → `size_category`        |

### 9. `size_category`
| Column            | Description                      |
|-------------------|----------------------------------|
| `size_category_id`| Primary Key                      |
| `category_name`   | E.g., Shoe Sizes, Shirt Sizes    |

### 10. `product_attribute`
| Column             | Description                              |
|--------------------|------------------------------------------|
| `attribute_id`     | Primary Key                              |
| `product_item_id`  | FK → `product_item`                      |
| `attribute_category_id`| FK → `attribute_category`           |
| `attribute_type_id`| FK → `attribute_type`                    |
| `attribute_name`   | E.g., "Material"                         |
| `attribute_value`  | E.g., "Cotton"                           |

### 11. `attribute_category`
| Column              | Description             |
|---------------------|-------------------------|
| `attribute_category_id` | Primary Key         |
| `name`              | E.g., Technical, Physical|

### 12. `attribute_type`
| Column            | Description           |
|-------------------|-----------------------|
| `attribute_type_id`| Primary Key          |
| `type_name`       | text, number, boolean |

---

## 📊 ERD Summary

```
product_category ——< product >—— brand
                                 |
                              product_item >—— product_image
                                 |
                           product_variation ——> color
                                 |
                               size_option ——> size_category

product >—— product_attribute ——> attribute_type ——> attribute_category
```

---

## 🚀 Future Enhancements
- Integrate **reviews** and **ratings**.
- Add **wishlist** and **product comparison** tables.
- Implement **inventory alerts** and **backorder handling**.
