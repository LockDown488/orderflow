# OrderFlow — модель данных MVP
Версия: 1.0 (проектное предложение, 15.11.2026)

```mermaid
erDiagram
  USERS ||--|| CARTS : owns
  USERS ||--o{ ORDERS : places
  CATEGORIES ||--o{ PRODUCTS : contains
  PRODUCTS ||--|| INVENTORY : stocked_as
  CARTS ||--o{ CART_ITEMS : contains
  PRODUCTS ||--o{ CART_ITEMS : selected_in
  ORDERS ||--|{ ORDER_ITEMS : contains
  PRODUCTS ||--o{ ORDER_ITEMS : referenced_by
  ORDERS ||--o| PAYMENTS : paid_by
  USERS {
    uuid id PK
    varchar email UK
    varchar password_hash
    varchar role
    boolean enabled
    timestamptz created_at
  }
  CATEGORIES {
    uuid id PK
    varchar name UK
    text description
  }
  PRODUCTS {
    uuid id PK
    uuid category_id FK
    varchar name
    text description
    numeric price
    boolean active
    timestamptz created_at
  }
  INVENTORY {
    uuid id PK
    uuid product_id FK,UK
    integer available_quantity
    integer reserved_quantity
    bigint version
  }
  CARTS {
    uuid id PK
    uuid user_id FK,UK
    timestamptz created_at
    timestamptz updated_at
  }
  CART_ITEMS {
    uuid id PK
    uuid cart_id FK
    uuid product_id FK
    integer quantity
  }
  ORDERS {
    uuid id PK
    uuid user_id FK
    varchar status
    numeric total_amount
    timestamptz created_at
    timestamptz updated_at
  }
  ORDER_ITEMS {
    uuid id PK
    uuid order_id FK
    uuid product_id FK
    varchar product_name
    numeric unit_price
    integer quantity
  }
  PAYMENTS {
    uuid id PK
    uuid order_id FK,UK
    numeric amount
    varchar status
    timestamptz created_at
    timestamptz processed_at
  }
```

## Ограничения
- UUID для ключей; деньги `NUMERIC(19,2)`/`BigDecimal`; время `TIMESTAMPTZ`/`Instant`.
- `users.email` уникален без учёта регистра (нормализованный email или функциональный индекс).
- `inventory.product_id`, `carts.user_id`, `payments.order_id` уникальны.
- `cart_items(cart_id, product_id)` уникален.
- `price > 0`, `quantity > 0`, `available_quantity >= 0`, `reserved_quantity >= 0`, `total_amount >= 0`, `amount >= 0`.
- `order_items` содержит snapshot названия и цены; `product_id` — ссылка на исходный товар, физическое удаление товара после заказов запрещено.
- `inventory.available_quantity` — доступное для нового резерва количество, `reserved_quantity` — зарезервированное; при резерве available -= q, reserved += q.
- Версия MVP: одна попытка оплаты на заказ, поэтому `payments.order_id` уникален. Для повторных попыток позже понадобится `payment_attempts`.
- В таблицах допускаются дополнительные индексы по `orders(user_id,created_at)`, `products(category_id,active)`.

## Замечания к реализации
Связи на ER-диаграмме не означают необходимость двунаправленных JPA-ассоциаций. Межмодульные ссылки предпочтительно хранить как UUID и использовать DTO. Удаление корзины после оформления не требуется: очищаются позиции.
