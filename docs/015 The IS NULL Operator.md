# 15 The IS NULL Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/iUMvtVXBvJ4?si=DDbNYDVRkgvGjkc7"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## Understanding `NULL`

* In SQL, **`NULL`** represents the **absence of a value**.
* It’s **not the same as `0` or an empty string (`''`)**.
* Example: In the `customers` table, the customer with `id = 5` has no phone number, so the `phone` field shows `NULL`.

---

## Checking for `NULL` Values

Since `NULL` cannot be compared using normal operators (`=`, `!=`, etc.), SQL provides special operators:

### `IS NULL`

* Used to find rows where a column’s value is missing.
* Example: Get all customers without a phone number.

```sql
SELECT 
    *
FROM
    customers
WHERE
    phone IS NULL;
```

✅ **Result:** Returns only those customers whose `phone` column is `NULL`.

---

### `IS NOT NULL`

* Used to find rows where a column has a value (not missing).
* Example: Get all customers who do have a phone number.

```sql
SELECT 
    *
FROM
    customers
WHERE
    phone IS NOT NULL;
```

✅ **Result:** Returns customers with valid phone numbers.

---

## Practical Use Case

* **Business scenario:** A company may want to contact customers who don’t have a phone number recorded.
* This allows them to request updated details via **email** or other methods.

---

## Exercise: Find Orders That Are Not Shipped Yet

### Context

* We have an `orders` table.
* An order is considered **not shipped** if:

  * `shipped_date` is `NULL`, **or**
  * `shipper_id` is `NULL`.

This indicates that the order hasn’t been assigned to a shipper or hasn’t been shipped.

---

### Query: Get Orders Without a Shipped Date

```sql
SELECT 
    *
FROM
    orders
WHERE
    shipped_date IS NULL;
```

✅ **Logic:**

* If `shipped_date` is `NULL`, it means no shipment has occurred.
* This will list **all pending orders**.

---

### Alternative Query: Check Missing Shipper

```sql
SELECT 
    *
FROM
    orders
WHERE
    shipper_id IS NULL;
```

✅ Both queries are correct, since a missing shipper also means the order is not shipped.

---

## Example Result (Based on Transcript)

The query should return these **5 unshipped orders**:

* Order IDs: `1, 3, 4, 6, 8`

---

## Key Takeaways

* Always use `IS NULL` and `IS NOT NULL` for checking missing values.
* `NULL` handling is critical in real-world applications such as:

  * Contacting customers missing key information.
  * Tracking pending shipments/orders.
  * Data validation and reporting.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
    phone IS NOT NULL;
    -- phone IS NULL;
```

## Exercise

### Get all the orders that are not shipped yet
```sql
USE sql_store;

SELECT 
    *
FROM
    orders
WHERE
    shipped_date IS NULL;
```
- **Logic:** If `shipped_date` is `NULL`, it means the order hasn’t been shipped.