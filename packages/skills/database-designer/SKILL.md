---
name: database-designer
description: This skill should be used when the user requests database schema design, migration generation, ER diagram creation, database optimization, or data modeling. It provides comprehensive database design patterns and migration strategies for SQL and NoSQL databases.
---

# Database Designer

## Overview

This skill enables comprehensive database schema design, migration generation, and data modeling. It creates optimized database structures following normalization principles, generates migrations for various ORMs, and provides best practices for both SQL and NoSQL databases.

## When to Use This Skill

Use this skill when the user requests:
- Database schema design
- ER diagram creation
- Database migration generation
- Data modeling
- Database normalization
- Index optimization
- Relationship design
- Migration strategy
- Database refactoring
- Schema versioning

## Core Workflow

### Step 1: Requirements Analysis

**Gather Information**:
- Entities and their attributes
- Relationships between entities
- Data volume expectations
- Query patterns
- Performance requirements
- Scalability needs

### Step 2: Database Design

**SQL Schema Example**:
```sql
-- Users table
CREATE TABLE users (
  id BIGSERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  username VARCHAR(50) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  deleted_at TIMESTAMP,
  CONSTRAINT email_format CHECK (email ~* '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$')
);

-- Posts table
CREATE TABLE posts (
  id BIGSERIAL PRIMARY KEY,
  user_id BIGINT NOT NULL,
  title VARCHAR(255) NOT NULL,
  content TEXT,
  status VARCHAR(20) DEFAULT 'draft',
  published_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  CONSTRAINT status_check CHECK (status IN ('draft', 'published', 'archived'))
);

-- Comments table
CREATE TABLE comments (
  id BIGSERIAL PRIMARY KEY,
  post_id BIGINT NOT NULL,
  user_id BIGINT NOT NULL,
  parent_id BIGINT,
  content TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  FOREIGN KEY (parent_id) REFERENCES comments(id) ON DELETE CASCADE
);

-- Tags table (many-to-many example)
CREATE TABLE tags (
  id BIGSERIAL PRIMARY KEY,
  name VARCHAR(50) UNIQUE NOT NULL,
  slug VARCHAR(50) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Post-Tag junction table
CREATE TABLE post_tags (
  post_id BIGINT NOT NULL,
  tag_id BIGINT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (post_id, tag_id),
  FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE,
  FOREIGN KEY (tag_id) REFERENCES tags(id) ON DELETE CASCADE
);

-- Indexes for performance
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_status ON posts(status);
CREATE INDEX idx_posts_published_at ON posts(published_at) WHERE status = 'published';
CREATE INDEX idx_comments_post_id ON comments(post_id);
CREATE INDEX idx_comments_user_id ON comments(user_id);
CREATE INDEX idx_comments_parent_id ON comments(parent_id) WHERE parent_id IS NOT NULL;
```

### Step 3: Migration Generation

**Sequelize Migration**:
```javascript
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.createTable('users', {
      id: {
        type: Sequelize.BIGINT,
        primaryKey: true,
        autoIncrement: true
      },
      email: {
        type: Sequelize.STRING(255),
        unique: true,
        allowNull: false
      },
      username: {
        type: Sequelize.STRING(50),
        unique: true,
        allowNull: false
      },
      password_hash: {
        type: Sequelize.STRING(255),
        allowNull: false
      },
      first_name: Sequelize.STRING(100),
      last_name: Sequelize.STRING(100),
      created_at: {
        type: Sequelize.DATE,
        defaultValue: Sequelize.literal('CURRENT_TIMESTAMP')
      },
      updated_at: {
        type: Sequelize.DATE,
        defaultValue: Sequelize.literal('CURRENT_TIMESTAMP')
      },
      deleted_at: Sequelize.DATE
    });

    await queryInterface.addIndex('users', ['email']);
    await queryInterface.addIndex('users', ['username']);
  },

  async down(queryInterface) {
    await queryInterface.dropTable('users');
  }
};
```

**TypeORM Migration**:
```typescript
import { MigrationInterface, QueryRunner, Table, TableIndex, TableForeignKey } from 'typeorm';

export class CreateUsers1234567890 implements MigrationInterface {
  public async up(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.createTable(
      new Table({
        name: 'users',
        columns: [
          {
            name: 'id',
            type: 'bigint',
            isPrimary: true,
            isGenerated: true,
            generationStrategy: 'increment'
          },
          {
            name: 'email',
            type: 'varchar',
            length: '255',
            isUnique: true,
            isNullable: false
          },
          {
            name: 'username',
            type: 'varchar',
            length: '50',
            isUnique: true,
            isNullable: false
          },
          {
            name: 'password_hash',
            type: 'varchar',
            length: '255',
            isNullable: false
          },
          {
            name: 'created_at',
            type: 'timestamp',
            default: 'CURRENT_TIMESTAMP'
          },
          {
            name: 'updated_at',
            type: 'timestamp',
            default: 'CURRENT_TIMESTAMP'
          }
        ]
      }),
      true
    );
  }

  public async down(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.dropTable('users');
  }
}
```

### Step 4: NoSQL Design

**MongoDB Schema**:
```javascript
// User schema with embedded documents
const userSchema = new Schema({
  email: { type: String, required: true, unique: true, lowercase: true },
  username: { type: String, required: true, unique: true },
  password: { type: String, required: true },
  profile: {
    firstName: String,
    lastName: String,
    avatar: String,
    bio: String
  },
  settings: {
    notifications: { type: Boolean, default: true },
    newsletter: { type: Boolean, default: false },
    theme: { type: String, enum: ['light', 'dark'], default: 'light' }
  },
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now }
});

// Indexes
userSchema.index({ email: 1 });
userSchema.index({ username: 1 });
userSchema.index({ createdAt: -1 });

// Post schema with references
const postSchema = new Schema({
  author: { type: Schema.Types.ObjectId, ref: 'User', required: true },
  title: { type: String, required: true, maxlength: 255 },
  content: { type: String, required: true },
  status: { type: String, enum: ['draft', 'published', 'archived'], default: 'draft' },
  tags: [{ type: String }],
  comments: [{
    user: { type: Schema.Types.ObjectId, ref: 'User' },
    content: String,
    createdAt: { type: Date, default: Date.now }
  }],
  stats: {
    views: { type: Number, default: 0 },
    likes: { type: Number, default: 0 }
  },
  publishedAt: Date,
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now }
});

// Indexes
postSchema.index({ author: 1, createdAt: -1 });
postSchema.index({ status: 1, publishedAt: -1 });
postSchema.index({ tags: 1 });
```

### Step 5: Relationship Types

**One-to-One**:
```sql
CREATE TABLE users (id BIGSERIAL PRIMARY KEY, ...);
CREATE TABLE profiles (
  id BIGSERIAL PRIMARY KEY,
  user_id BIGINT UNIQUE NOT NULL,
  bio TEXT,
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

**One-to-Many**:
```sql
CREATE TABLE authors (id BIGSERIAL PRIMARY KEY, ...);
CREATE TABLE books (
  id BIGSERIAL PRIMARY KEY,
  author_id BIGINT NOT NULL,
  FOREIGN KEY (author_id) REFERENCES authors(id)
);
```

**Many-to-Many**:
```sql
CREATE TABLE students (id BIGSERIAL PRIMARY KEY, ...);
CREATE TABLE courses (id BIGSERIAL PRIMARY KEY, ...);
CREATE TABLE enrollments (
  student_id BIGINT,
  course_id BIGINT,
  enrolled_at TIMESTAMP,
  PRIMARY KEY (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES students(id),
  FOREIGN KEY (course_id) REFERENCES courses(id)
);
```

## Database Design Patterns

**Soft Delete**:
```sql
ALTER TABLE users ADD COLUMN deleted_at TIMESTAMP;
CREATE INDEX idx_users_deleted_at ON users(deleted_at) WHERE deleted_at IS NULL;
```

**Audit Trail**:
```sql
CREATE TABLE audit_log (
  id BIGSERIAL PRIMARY KEY,
  table_name VARCHAR(50),
  record_id BIGINT,
  action VARCHAR(20),
  old_values JSONB,
  new_values JSONB,
  user_id BIGINT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Versioning**:
```sql
CREATE TABLE document_versions (
  id BIGSERIAL PRIMARY KEY,
  document_id BIGINT NOT NULL,
  version INT NOT NULL,
  content TEXT,
  created_by BIGINT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE (document_id, version)
);
```

## Best Practices

**Normalization**:
- 1NF: Atomic values, no repeating groups
- 2NF: No partial dependencies
- 3NF: No transitive dependencies
- Denormalize when performance requires it

**Indexing**:
- Index foreign keys
- Index frequently queried columns
- Index columns used in WHERE, ORDER BY, JOIN
- Avoid over-indexing (impacts INSERT/UPDATE)
- Use partial indexes when appropriate

**Constraints**:
- Use NOT NULL where appropriate
- Add CHECK constraints for data validation
- Use UNIQUE constraints
- Define proper foreign keys with CASCADE rules

**Naming Conventions**:
- Tables: plural, lowercase with underscores (users, post_tags)
- Columns: lowercase with underscores (created_at, user_id)
- Indexes: idx_tablename_columnname
- Foreign keys: fk_tablename_columnname
- Primary keys: Usually 'id' or composite

## Migration Best Practices

**Always Include**:
- Up and down migrations
- Timestamps in migration names
- Backup before running migrations
- Test migrations on staging first
- Version control all migrations

**Migration Checklist**:
1. Create backup
2. Review migration SQL
3. Test on development
4. Test on staging
5. Plan rollback strategy
6. Run during low-traffic period
7. Monitor performance after migration
