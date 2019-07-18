# Migration `20190718141303-create-farmer-and-products`

This migration has been generated by zakayothuku at 7/18/2019, 2:13:03 PM.
You can check out the [state of the datamodel](./datamodel.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "file:dev"."Farmer"("id" TEXT NOT NULL  ,"farmerId" TEXT NOT NULL DEFAULT '' ,"firstName" TEXT NOT NULL DEFAULT '' ,"lastName" TEXT NOT NULL DEFAULT '' ,"createdAt" DATE NOT NULL  ,"updatedAt" DATE NOT NULL DEFAULT '1970-01-01 00:00:00' ,PRIMARY KEY ("id"));

CREATE TABLE "file:dev"."Product"("id" TEXT NOT NULL  ,"qrCode" TEXT NOT NULL DEFAULT '' ,"productType" TEXT NOT NULL DEFAULT 'APPLE' ,"productName" TEXT NOT NULL DEFAULT '' ,"plantedOn" DATE NOT NULL DEFAULT '1970-01-01 00:00:00' ,"harvestedOn" DATE NOT NULL DEFAULT '1970-01-01 00:00:00' ,"createdAt" DATE NOT NULL  ,"updatedAt" DATE NOT NULL DEFAULT '1970-01-01 00:00:00' ,"farmer" TEXT   REFERENCES Farmer(id),PRIMARY KEY ("id"));

CREATE UNIQUE INDEX "file:dev"."Farmer.id._UNIQUE" ON "Farmer"("id")

CREATE UNIQUE INDEX "file:dev"."Product.id._UNIQUE" ON "Product"("id")
```

## Changes

```diff
diff --git datamodel.mdl datamodel.mdl
migration ..20190718141303-create-farmer-and-products
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,41 @@
+datasource db {
+    provider = "sqlite"
+    url      = "file:dev.db"
+    default  = true
+}
+
+generator photon {
+    provider = "photonjs"
+}
+
+model Farmer {
+  id String @default(cuid()) @id @unique
+  farmerId String
+  firstName String
+  lastName String
+
+  products Product[]
+
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+}
+
+model Product {
+    id String @default(cuid()) @id @unique
+    qrCode String
+    productType ProductType
+    productName String
+
+    farmer Farmer?
+    plantedOn DateTime
+    harvestedOn DateTime
+
+    createdAt DateTime @default(now())
+    updatedAt DateTime @updatedAt
+}
+
+enum ProductType {
+    APPLE
+    GRAPES
+    BERRIES
+}
```

## Photon Usage

You can use a specific Photon built for this migration (20190718141303-create-farmer-and-products)
in your `before` or `after` migration script like this:

```ts
import Photon from '@generated/photon/20190718141303-create-farmer-and-products'

const photon = new Photon()

async function main() {
  const result = await photon.users()
  console.dir(result, { depth: null })
}

main()

```