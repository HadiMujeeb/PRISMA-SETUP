# PRISMA-SETUP


To set up and use multi-file Prisma schemas in your project, follow these detailed steps. This guide assumes you are already familiar with Prisma and have it installed in your project.

Step 1: Ensure You Have the Right Prisma Version
Make sure you are using Prisma version 5.15.0 or later, as multi-file schema support is available only in this version and above. You can check your version with:

bash
Copy code
npx prisma --version
If you need to update Prisma, you can run:

bash
Copy code
npm install prisma --save-dev
Step 2: Enable Multi-file Schema Support
You need to enable the prismaSchemaFolder preview feature in your main Prisma schema file (schema.prisma). Open or create the file, and add the following code:

prisma
Copy code
generator client {
  provider        = "prisma-client-js"
  previewFeatures = ["prismaSchemaFolder"]
}

datasource db {
  provider = "mongodb" // or your database provider
  url      = env("DATABASE_URL")
}
Step 3: Create the Schema Directory Structure
Create a directory inside your Prisma folder to hold your schema files. The recommended structure is:

arduino
Copy code
my-app/
├─ ...
├─ prisma/
│  ├─ schema/
│  │  ├─ schema.prisma      // Main schema file
│  │  ├─ user.prisma        // User-related models
│  │  ├─ post.prisma        // Post-related models
│  │  ├─ otp.prisma         // OTP-related models
Step 4: Add Your Sub-schema Files
Create sub-schema files in the prisma/schema/ directory. For example:

User Schema (user.prisma)
prisma
Copy code
model User {
    id        String    @id @default(auto()) @map("_id") @db.ObjectId
    name      String
    password  String
    email     String
    role      String?
    country   String?
    profession String?
    avatar    String?
}
Post Schema (post.prisma)
prisma
Copy code
model Post {
    id        String    @id @default(auto()) @map("_id") @db.ObjectId
    title     String
    content   String
    authorId  String    @db.ObjectId
    createdAt DateTime  @default(now())
}
OTP Schema (otp.prisma)
prisma
Copy code
model OTP {
    id        String   @id @default(auto()) @map("_id") @db.ObjectId
    otp       String
    email     String
    expiresAt DateTime
    createdAt DateTime @default(now())
}
Step 5: Run Prisma Commands
After setting up your schemas, you can run Prisma commands normally. For example, to push your schema changes to the database, use:

bash
Copy code
npx prisma db push --schema prisma/schema
Step 6: Generate the Prisma Client
After updating your schema, generate the Prisma Client by running:

bash
Copy code
npx prisma generate --schema prisma/schema
