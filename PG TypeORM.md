---
tags:
- Programming-Language/JS-TS
- ORM
- PG
MOC: Technology
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG JS-TS index|Back to index]]
### TypeORM tips
- Always make sure the attribute names used in the frontend DTO matches the backend entities for typeORM to know which fields to populate with data
# The MOFO bug
### The bug
```node
Error during migration generation:
Error: SASL: SCRAM-SERVER-FIRST-MESSAGE: client password must be a string
	at Object.continueSession (/home/blackdovah/WorkProjects-e-sky/Biditauto/biditauto-backend/node_modules/.pnpm/pg@8.16.3/node_modules/pg/lib/crypto/sasl.js:36:11)
	at Client._handleAuthSASLContinue (/home/blackdovah/WorkProjects-e-sky/Biditauto/biditauto-backend/node_modules/.pnpm/pg@8.16.3/node_modules/pg/lib/client.js:276:18)
	at Connection.emit (node:events:508:28)
	at /home/blackdovah/WorkProjects-e-sky/Biditauto/biditauto-backend/node_modules/.pnpm/pg@8.16.3/node_modules/pg/lib/connection.js:116:12
	at Parser.parse (/home/blackdovah/WorkProjects-e-sky/Biditauto/biditauto-backend/node_modules/.pnpm/pg-protocol@1.10.3/node_modules/pg-protocol/dist/parser.js:36:17)
	at Socket.<anonymous> (/home/blackdovah/WorkProjects-e-sky/Biditauto/biditauto-backend/node_modules/.pnpm/pg-protocol@1.10.3/node_modules/pg-protocol/dist/index.js:11:42)
	at Socket.emit (node:events:508:28)
	at addChunk (node:internal/streams/readable:559:12)
	at readableAddChunkPushByteMode (node:internal/streams/readable:510:3)
	at Readable.push (node:internal/streams/readable:390:5)
 ELIFECYCLE  Command failed with exit code 1.
 ELIFECYCLE  Command failed with exit code 1.
```
### The issue
- The development env is not being detected, so no variables are being passed
```node
NODE_ENV: undefined
Config params: {
  host: 'localhost',
  port: 5432,
  username: undefined,
  password: 'UNDEFINED',
  database: undefined
}
``` 
# The fix
- Change this line `dotenv({ path: `.env.${process.env.NODE_ENV }` });` in the datasource at least temporarily to 
- dotenv({ path: `.env.${process.env.NODE_ENV || 'development'}` });`
# Relations
``` TS
//1. OneToOne Relationship

//Example: User ↔ Profile (One user has one profile)

// user/entities/user.entity.ts
import { Entity, PrimaryGeneratedColumn, Column, OneToOne, JoinColumn } from 'typeorm';
import { Profile } from './profile.entity';

@Entity('users')
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  email: string;

  // ✅ OneToOne - Owning side (has the foreign key)
  @OneToOne(() => Profile, profile => profile.user, { cascade: true })
  @JoinColumn() // Only on the owning side
  profile: Profile;
}

// profile/entities/profile.entity.ts
import { Entity, PrimaryGeneratedColumn, Column, OneToOne } from 'typeorm';
import { User } from './user.entity';

@Entity('profiles')
export class Profile {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  firstName: string;

  @Column()
  lastName: string;

  // ✅ OneToOne - Inverse side (no foreign key)
  @OneToOne(() => User, user => user.profile)
  user: User;
}

// Create with relation
const user = userRepo.create({
  email: 'john@example.com',
  profile: {
    firstName: 'John',
    lastName: 'Doe'
  }
});
await userRepo.save(user);

// Query with relation
const userWithProfile = await userRepo.findOne({
  where: { id: 1 },
  relations: ['profile']
});
```

```TS
//2. OneToMany / ManyToOne Relationship

//Example: User ↔ Posts (One user has many posts)

// user/entities/user.entity.ts
import { Entity, PrimaryGeneratedColumn, Column, OneToMany } from 'typeorm';
import { Post } from './post.entity';

@Entity('users')
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  // ✅ OneToMany - One user has many posts
  @OneToMany(() => Post, post => post.user, { cascade: true })
  posts: Post[];
}

// post/entities/post.entity.ts
import { Entity, PrimaryGeneratedColumn, Column, ManyToOne, JoinColumn } from 'typeorm';
import { User } from './user.entity';

@Entity('posts')
export class Post {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  title: string;

  @Column()
  content: string;

  @Column()
  userId: number; // Foreign key column

  // ✅ ManyToOne - Many posts belong to one user
  @ManyToOne(() => User, user => user.posts, { onDelete: 'CASCADE' })
  @JoinColumn({ name: 'userId' }) // Optional: specify FK column name
  user: User;
}

// Create with relation
const user = await userRepo.findOne({ where: { id: 1 } });
const post = postRepo.create({
  title: 'My Post',
  content: 'Content here',
  user: user // or userId: 1
});
await postRepo.save(post);

// Query with relations
const userWithPosts = await userRepo.findOne({
  where: { id: 1 },
  relations: ['posts']
});

const postWithUser = await postRepo.findOne({
  where: { id: 1 },
  relations: ['user']
});
```

```TS
//3. ManyToMany Relationship

//Example: User ↔ Roles (Many users have many roles)

// user/entities/user.entity.ts
import { Entity, PrimaryGeneratedColumn, Column, ManyToMany, JoinTable } from 'typeorm';
import { Role } from './role.entity';

@Entity('users')
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  // ✅ ManyToMany - Owning side (has JoinTable)
  @ManyToMany(() => Role, role => role.users, { cascade: true })
  @JoinTable({
    name: 'user_roles', // Custom junction table name
    joinColumn: { name: 'userId', referencedColumnName: 'id' },
    inverseJoinColumn: { name: 'roleId', referencedColumnName: 'id' }
  })
  roles: Role[];
}

// role/entities/role.entity.ts
import { Entity, PrimaryGeneratedColumn, Column, ManyToMany } from 'typeorm';
import { User } from './user.entity';

@Entity('roles')
export class Role {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  // ✅ ManyToMany - Inverse side (no JoinTable)
  @ManyToMany(() => User, user => user.roles)
  users: User[];
}

// Create and assign roles
const adminRole = await roleRepo.findOne({ where: { name: 'Admin' } });
const userRole = await roleRepo.findOne({ where: { name: 'User' } });

const user = userRepo.create({
  name: 'John Doe',
  roles: [adminRole, userRole]
});
await userRepo.save(user);

// Add role to existing user
const user = await userRepo.findOne({ 
  where: { id: 1 }, 
  relations: ['roles'] 
});
const newRole = await roleRepo.findOne({ where: { id: 3 } });
user.roles.push(newRole);
await userRepo.save(user);

// Query with relations
const userWithRoles = await userRepo.findOne({
  where: { id: 1 },
  relations: ['roles']
});

const roleWithUsers = await roleRepo.findOne({
  where: { id: 1 },
  relations: ['users']
});
```