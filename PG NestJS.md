---
tags:
- JS/TS
- Framework
- PG
MOC: Knowledge Base
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[PG JS-TS index|Back to index]]
# Decorator
- A decorator can be defined as follows: 
	```
	import { Reflector } from '@nestjs/core';
	import { Permissions } from 'src/enums';

	export const RequirePermissions = Reflector.createDecorator<Permissions[]>();
	```
- The reflector is a helper class that has the ability to create a decorator used to add metadata to classes and methods.
- It uses the provided type to know what metadata to add
# Guard creation tips
- When creating a guard, remember that:
	- `CanActivate` is a class that has the `canActivate()` function, which must be implemented by guards as its return value determines if the request will be allowed to proceed.
	- The return value can be a synchronous boolean, or an asynchronous promise, or observable.
	- `canActivate()`requires a context, such as `ExecutionContext` which is the context of the incoming request in the pipeline
	- Inside canActivate we need to use the `reflector` which takes a decorator to reflect, and an array of contexts
	- `context.getHandler()` Is a context for CRUD methods like GET, POST, PUT, PATCH and DELETE
	- `context.getClass()` is for the controller class as a whole
	- Passing both `[context.getHandler(), context.getClass()]` in this order in an array means to first check if the specific method has a specific permission to check, and if not, fall back to the controller permission
	- `context.switchToGttp().getRequst()` converts the generic nestJS execution context that works with HTTP, WebSocket, GraphQL specifically to HTTP