---
name: prisma
description: Prisma ORM patterns, schema design, migrations, type-safe queries, and performance best practices.
triggers: ["prisma", "orm", "database", "migration", "schema", "model", "query"]
---

# Prisma Patterns

Best practices for working with Prisma ORM: schema design, migrations, type-safe queries, and performance optimization.

## When to Use

Invoke this skill when:
- Creating or modifying Prisma schema
- Writing database migrations
- Querying data with Prisma Client
- Optimizing database performance
- Setting up relations between models

## Project Structure

```
your-project/
├── prisma/
│   ├── schema.prisma       # Main schema file
│   └── migrations/         # Generated migrations
├── prisma.config.ts        # Prisma configuration
├── generated/
│   └── prisma/             # Generated Prisma Client (gitignored)
└── .env                    # DATABASE_URL (gitignored)
```

## Schema Design Patterns

### Model Naming Convention

```prisma
// Use PascalCase for models
model Store {
  // Use camelCase for fields
  id          String   @id
  ownerWallet String   @map("owner_wallet") // Map to snake_case in DB
  createdAt   DateTime @map("created_at")
  
  @@map("stores") // Map to snake_case table name
}
```

### Field Types

```prisma
// Correct type mappings
model Example {
  // IDs
  id        String   @id @default(dbgenerated("gen_random_uuid()")) @db.Uuid
  
  // Strings
  name      String   @db.VarChar(200)       // With length constraint
  bio       String?                          // Optional, unlimited text
  
  // Numbers
  count     Int                              // Standard integer
  amount    BigInt                           // Large numbers
  price     Decimal  @db.Decimal(18, 6)     // Money (never use Float!)
  
  // Dates
  createdAt DateTime @default(now()) @db.Timestamptz
  updatedAt DateTime @updatedAt @db.Timestamptz
  
  // Boolean
  isActive  Boolean  @default(true)
  
  // Arrays
  tags      String[] @default([])
  
  // JSON
  metadata  Json?    @db.JsonB
}
```

### Enum Pattern

```prisma
// Define enums for constrained values
enum Status {
  draft
  published
  archived
}

model Post {
  status Status @default(draft)
}
```

### Relations

```prisma
// One-to-Many
model Store {
  id      String   @id @default(uuid())
  bundles Bundle[]
}

model Bundle {
  id      String @id @default(uuid())
  storeId String @map("store_id")
  store   Store  @relation(fields: [storeId], references: [id], onDelete: Cascade)
  
  @@index([storeId])
}

// Many-to-Many (explicit)
model Post {
  id       String     @id
  tags     PostTag[]
}

model Tag {
  id    String     @id
  posts PostTag[]
}

model PostTag {
  postId String @map("post_id")
  tagId  String @map("tag_id")
  post   Post   @relation(fields: [postId], references: [id])
  tag    Tag    @relation(fields: [tagId], references: [id])
  
  @@id([postId, tagId])
  @@map("post_tags")
}
```

### Indexes

```prisma
model Bundle {
  id      String @id
  storeId String @map("store_id")
  name    String
  version String
  status  String
  
  // Single-column index
  @@index([storeId])
  @@index([status])
  
  // Composite index (column order matters!)
  @@index([status, createdAt])
  
  // Unique constraint
  @@unique([storeId, name, version])
}
```

## Migration Patterns

### Creating Migrations

```bash
# Create a new migration (development)
npm run prisma:migrate -- --name add_users_table

# Apply migrations (production)
npm run prisma:migrate:deploy

# Reset database (development only!)
npx prisma migrate reset
```

### Migration Best Practices

```sql
-- migrations/20240101000000_add_users_table/migration.sql

-- Always check if exists for safety
CREATE TABLE IF NOT EXISTS users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT NOT NULL UNIQUE,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Add index in the same migration
CREATE INDEX IF NOT EXISTS idx_users_email ON users(email);
```

### Data Migrations

```typescript
// For complex data migrations, use a script
import { prisma } from '../lib/prisma'

async function migrateData() {
  const users = await prisma.user.findMany({
    where: { migratedAt: null }
  })
  
  for (const user of users) {
    await prisma.user.update({
      where: { id: user.id },
      data: { 
        normalizedEmail: user.email.toLowerCase(),
        migratedAt: new Date()
      }
    })
  }
}

migrateData().finally(() => prisma.$disconnect())
```

## Query Patterns

### Basic CRUD

```typescript
import { prisma } from '../lib/prisma'

// Create
const store = await prisma.store.create({
  data: {
    name: 'My Store',
    ownerWallet: '0x123...',
  },
})

// Read one
const store = await prisma.store.findUnique({
  where: { id: storeId },
})

// Read many
const stores = await prisma.store.findMany({
  where: { status: 'active' },
  orderBy: { createdAt: 'desc' },
  take: 10,
})

// Update
const updated = await prisma.store.update({
  where: { id: storeId },
  data: { name: 'New Name' },
})

// Delete
await prisma.store.delete({
  where: { id: storeId },
})
```

### Relations

```typescript
// Include relations
const store = await prisma.store.findUnique({
  where: { id: storeId },
  include: {
    bundles: {
      where: { status: 'published' },
      orderBy: { createdAt: 'desc' },
    },
  },
})

// Select specific fields
const store = await prisma.store.findUnique({
  where: { id: storeId },
  select: {
    id: true,
    name: true,
    bundles: {
      select: { id: true, name: true },
    },
  },
})

// Nested create
const store = await prisma.store.create({
  data: {
    name: 'Store',
    ownerWallet: '0x...',
    bundles: {
      create: [
        { name: 'Bundle 1', version: '1.0.0', ... },
        { name: 'Bundle 2', version: '1.0.0', ... },
      ],
    },
  },
  include: { bundles: true },
})
```

### Filtering

```typescript
// Complex filters
const bundles = await prisma.bundle.findMany({
  where: {
    AND: [
      { status: 'published' },
      { storeId: storeId },
      {
        OR: [
          { name: { contains: 'skill', mode: 'insensitive' } },
          { description: { contains: 'skill', mode: 'insensitive' } },
        ],
      },
    ],
  },
})

// Date filtering
const recent = await prisma.bundle.findMany({
  where: {
    createdAt: {
      gte: new Date('2024-01-01'),
      lt: new Date('2024-02-01'),
    },
  },
})

// Array filtering
const withFramework = await prisma.bundle.findMany({
  where: {
    frameworks: { has: 'react' },
  },
})
```

### Pagination

```typescript
// Offset pagination (simple, not recommended for large datasets)
const page = 1
const pageSize = 20

const bundles = await prisma.bundle.findMany({
  skip: (page - 1) * pageSize,
  take: pageSize,
  orderBy: { createdAt: 'desc' },
})

// Cursor pagination (recommended)
const bundles = await prisma.bundle.findMany({
  take: 20,
  cursor: lastId ? { id: lastId } : undefined,
  skip: lastId ? 1 : 0,
  orderBy: { createdAt: 'desc' },
})
```

### Transactions

```typescript
// Interactive transaction
const result = await prisma.$transaction(async (tx) => {
  const store = await tx.store.create({
    data: { name: 'Store', ownerWallet: '0x...' },
  })
  
  const bundle = await tx.bundle.create({
    data: { 
      storeId: store.id, 
      name: 'Bundle',
      version: '1.0.0',
      // ...
    },
  })
  
  return { store, bundle }
})

// Sequential transaction
const [store, bundle] = await prisma.$transaction([
  prisma.store.create({ data: { ... } }),
  prisma.bundle.create({ data: { ... } }),
])
```

### Raw Queries

```typescript
// When Prisma API is insufficient
const results = await prisma.$queryRaw<{ count: bigint }[]>`
  SELECT COUNT(*) as count
  FROM bundles
  WHERE status = 'published'
  AND created_at > ${startDate}
`

// Execute raw (for DDL or special cases)
await prisma.$executeRaw`
  UPDATE stores 
  SET status = 'inactive' 
  WHERE updated_at < NOW() - INTERVAL '1 year'
`
```

## Prisma Client Setup

### Singleton Pattern

```typescript
// lib/prisma.ts
import { PrismaClient } from '../generated/prisma'

const globalForPrisma = globalThis as unknown as {
  prisma: PrismaClient | undefined
}

export const prisma =
  globalForPrisma.prisma ??
  new PrismaClient({
    log: process.env.NODE_ENV === 'development' 
      ? ['query', 'error', 'warn'] 
      : ['error'],
  })

if (process.env.NODE_ENV !== 'production') {
  globalForPrisma.prisma = prisma
}

export type { PrismaClient } from '../generated/prisma'
```

### Edge Runtime Support

```typescript
// For edge environments (Vercel Edge, Cloudflare Workers)
import { PrismaClient } from '../generated/prisma/edge'
import { withAccelerate } from '@prisma/extension-accelerate'

export const prisma = new PrismaClient().$extends(withAccelerate())
```

## Performance Patterns

### N+1 Prevention

```typescript
// ❌ BAD: N+1 queries
const stores = await prisma.store.findMany()
for (const store of stores) {
  const bundles = await prisma.bundle.findMany({
    where: { storeId: store.id },
  })
}

// ✅ GOOD: Single query with include
const stores = await prisma.store.findMany({
  include: { bundles: true },
})

// ✅ GOOD: Separate query with IN
const stores = await prisma.store.findMany()
const bundles = await prisma.bundle.findMany({
  where: { storeId: { in: stores.map(s => s.id) } },
})
```

### Select Only What You Need

```typescript
// ❌ BAD: Fetching all fields
const store = await prisma.store.findUnique({
  where: { id: storeId },
})

// ✅ GOOD: Select specific fields
const store = await prisma.store.findUnique({
  where: { id: storeId },
  select: { id: true, name: true },
})
```

### Batch Operations

```typescript
// ❌ BAD: Individual creates
for (const item of items) {
  await prisma.bundle.create({ data: item })
}

// ✅ GOOD: Batch create
await prisma.bundle.createMany({
  data: items,
  skipDuplicates: true,
})
```

## Testing Patterns

### Test Setup

```typescript
// test/setup.ts
import { prisma } from '../lib/prisma'

beforeEach(async () => {
  // Clean tables in correct order (respect FKs)
  await prisma.receipt.deleteMany()
  await prisma.offer.deleteMany()
  await prisma.bundle.deleteMany()
  await prisma.store.deleteMany()
})

afterAll(async () => {
  await prisma.$disconnect()
})
```

### Factory Pattern

```typescript
// test/factories/store.ts
import { prisma } from '../../lib/prisma'

export function createStore(overrides = {}) {
  return prisma.store.create({
    data: {
      name: 'Test Store',
      ownerWallet: '0x' + '1'.repeat(40),
      status: 'active',
      ...overrides,
    },
  })
}
```

## Best Practices

### DO:
- ✅ Use meaningful model and field names
- ✅ Map to snake_case in database (`@map`, `@@map`)
- ✅ Add indexes for frequently queried fields
- ✅ Use `@db.Uuid` for UUIDs, `@db.Timestamptz` for dates
- ✅ Use `Decimal` for money, never `Float`
- ✅ Use transactions for multi-step operations
- ✅ Use cursor pagination for large datasets
- ✅ Handle `BigInt` serialization for JSON responses

### DON'T:
- ❌ Use raw queries when Prisma API suffices
- ❌ Forget indexes on foreign keys
- ❌ Use offset pagination on large tables
- ❌ Create N+1 query patterns
- ❌ Use `Float` for money calculations
- ❌ Commit `.env` or `generated/` folder

## Prisma CLI Commands

```bash
# Generate client after schema changes
npm run prisma:generate

# Create migration (development)
npm run prisma:migrate -- --name descriptive_name

# Deploy migrations (production)
npm run prisma:migrate:deploy

# Push schema without migration (prototyping)
npm run prisma:push

# Pull schema from existing database
npm run prisma:pull

# Open Prisma Studio (visual editor)
npm run prisma:studio

# Format schema
npx prisma format

# Validate schema
npx prisma validate
```

## Checklist

- [ ] Schema uses proper naming conventions (PascalCase models, camelCase fields)
- [ ] All models map to snake_case in database
- [ ] Foreign keys have indexes
- [ ] Unique constraints defined where needed
- [ ] Enums used for constrained values
- [ ] Relations have proper onDelete behavior
- [ ] Money fields use Decimal, not Float
- [ ] Dates use @db.Timestamptz
- [ ] UUIDs use @db.Uuid
- [ ] Migration tested locally before deploy

## Related

- **Database Reviewer Agent**: `agents/database-reviewer.md`
- **Files**: `prisma/schema.prisma`, `generated/prisma/`

---

**Remember**: Prisma provides type safety at compile time, but you still need proper indexes and query optimization for runtime performance. Always test migrations locally before deploying to production.
