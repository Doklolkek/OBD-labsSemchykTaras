<div align="center">
  <strong>Міністерство освіти і науки України</strong><br>
  <strong>Національний технічний університет України</strong><br>
  <strong>«Київський політехнічний інститут імені Ігоря Сікорського»</strong><br><br>
  
  <h2>Лабораторна робота №6</h2>
  <h3>«Міграції схем за допомогою Prisma ORM»</h3><br>
  
  Київ 2026
</div>

<br>

**Роботу виконав:**
Студент групи ІО-43
Семчук Т.Р

**Роботу перевірив:**
Русінов В.В.

---

## 1. Додавання нової таблиці `Review`

**Що змінилося:** Було створено абсолютно нову таблицю `Review` для зберігання відгуків. 

**Схема до:**
*(Модель `review` не існувала)*

**Схема після:**
```prisma
model Review {
  id        Int     @id @default(autoincrement())
  rating    Int
  comment   String?
  product   product @relation(fields: [productId], references: [product_id])
  productId Int
}
```

## 2. Додавання поля isAvailable до таблиці product

**Що змінилося:** Для перевірки товару чи він наявний було добавлено поле isAvailable у таблицю product,
також до цієї таблиці було додано новий звязок з review.

**Схема до:**
```prisma
model product {
  product_id     Int             @id @default(autoincrement())
  sku            String          @unique @db.VarChar(30)
  product_name   String          @db.VarChar(100)
  description    String?
  category_id    Int?
  base_price     Decimal         @db.Decimal(10, 2)
  nicotine_level Decimal?        @db.Decimal(4, 2)
  volume         Int?
  power          Int?
  inventory      inventory?
  category       category?       @relation(fields: [category_id], references: [category_id], onUpdate: NoAction)
  purchase_item  purchase_item[]
  sale_item      sale_item[]
}
```

**Схема після:**
```prisma
model product {
  product_id     Int             @id @default(autoincrement())
  sku            String          @unique @db.VarChar(30)
  product_name   String          @db.VarChar(100)
  description    String?
  category_id    Int?
  base_price     Decimal         @db.Decimal(10, 2)
  nicotine_level Decimal?        @db.Decimal(4, 2)
  volume         Int?
  power          Int?
// new field 
  isAvailable    Boolean         @default(true)
// new field
  inventory      inventory?
  category       category?       @relation(fields: [category_id], references: [category_id], onUpdate: NoAction)
  purchase_item  purchase_item[]
  sale_item      sale_item[]
// new link
  review         Review[]
}
```

## 3. Видалення поля description з таблиці product

**Що змінилося:** Задля оптимізації структури бази даних з таблиці product було видалено зайве поле description.

**Схема до:**
```prisma
model product {
  product_id     Int             @id @default(autoincrement())
  sku            String          @unique @db.VarChar(30)
  product_name   String          @db.VarChar(100)
  description    String?
  category_id    Int?
  base_price     Decimal         @db.Decimal(10, 2)
  nicotine_level Decimal?        @db.Decimal(4, 2)
  volume         Int?
  power          Int?
// new field 
  isAvailable    Boolean         @default(true)
// new field
  inventory      inventory?
  category       category?       @relation(fields: [category_id], references: [category_id], onUpdate: NoAction)
  purchase_item  purchase_item[]
  sale_item      sale_item[]
// new link
  review         Review[]
}
```

**Схема після:**
```prisma
model product {
  product_id     Int             @id @default(autoincrement())
  sku            String          @unique @db.VarChar(30)
  product_name   String          @db.VarChar(100)
  category_id    Int?
  base_price     Decimal         @db.Decimal(10, 2)
  nicotine_level Decimal?        @db.Decimal(4, 2)
  volume         Int?
  power          Int?
// new field 
  isAvailable    Boolean         @default(true)
// new field
  inventory      inventory?
  category       category?       @relation(fields: [category_id], references: [category_id], onUpdate: NoAction)
  purchase_item  purchase_item[]
  sale_item      sale_item[]
// new link
  review         Review[]
}
```