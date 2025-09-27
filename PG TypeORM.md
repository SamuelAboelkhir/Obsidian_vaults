---
tags:
- JS/TS
- ORM
- PG
MOC: Technology
---
[[_0000 Home|Home]] | [[_0001 Technology MOC|Back to Technology MOC]] | [[PG JS-TS index|Back to index]]
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