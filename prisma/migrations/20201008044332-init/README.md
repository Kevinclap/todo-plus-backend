# Migration `20201008044332-init`

This migration has been generated at 10/7/2020, 11:43:32 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "public"."List" (
"id" SERIAL,
"createdAt" timestamp(3)   NOT NULL DEFAULT CURRENT_TIMESTAMP,
"name" text   NOT NULL ,
"userId" integer   ,
PRIMARY KEY ("id")
)

CREATE TABLE "public"."Todo" (
"id" SERIAL,
"createdAt" timestamp(3)   NOT NULL DEFAULT CURRENT_TIMESTAMP,
"title" text   NOT NULL ,
"text" text   ,
"completed" boolean   NOT NULL DEFAULT false,
"schedule" timestamp(3)   NOT NULL ,
"authorId" integer   NOT NULL ,
"listId" integer   ,
PRIMARY KEY ("id")
)

CREATE TABLE "public"."Profile" (
"id" SERIAL,
"bio" text   ,
"userId" integer   NOT NULL ,
PRIMARY KEY ("id")
)

CREATE TABLE "public"."User" (
"id" SERIAL,
"email" text   NOT NULL ,
"name" text   ,
PRIMARY KEY ("id")
)

CREATE UNIQUE INDEX "Profile.userId_unique" ON "public"."Profile"("userId")

CREATE UNIQUE INDEX "User.email_unique" ON "public"."User"("email")

ALTER TABLE "public"."List" ADD FOREIGN KEY ("userId")REFERENCES "public"."User"("id") ON DELETE SET NULL ON UPDATE CASCADE

ALTER TABLE "public"."Todo" ADD FOREIGN KEY ("authorId")REFERENCES "public"."User"("id") ON DELETE CASCADE ON UPDATE CASCADE

ALTER TABLE "public"."Todo" ADD FOREIGN KEY ("listId")REFERENCES "public"."List"("id") ON DELETE SET NULL ON UPDATE CASCADE

ALTER TABLE "public"."Profile" ADD FOREIGN KEY ("userId")REFERENCES "public"."User"("id") ON DELETE CASCADE ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20201008044332-init
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,41 @@
+// This is your Prisma schema file,
+// learn more about it in the docs: https://pris.ly/d/prisma-schema
+
+datasource db {
+  provider = "postgresql"
+  url = "***"
+}
+
+generator client {
+  provider = "prisma-client-js"
+}
+
+model List {
+  id        Int      @default(autoincrement()) @id
+  createdAt DateTime @default(now())
+  name     String
+  todos Todo[]
+}
+model Todo {
+  id        Int      @default(autoincrement()) @id
+  createdAt DateTime @default(now())
+  title     String
+  text   String?
+  completed Boolean  @default(false)
+  schedule DateTime
+  author    User     @relation(fields: [authorId], references: [id])
+  authorId  Int
+}
+model Profile {
+  id     Int     @default(autoincrement()) @id
+  bio    String?
+  user   User    @relation(fields: [userId], references: [id])
+  userId Int     @unique
+}
+model User {
+  id      Int      @default(autoincrement()) @id
+  email   String   @unique
+  name    String?
+  lists   List[]
+  profile Profile?
+}
```


