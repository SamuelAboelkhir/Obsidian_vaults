---
tags:
- Programming-Language/JS-TS
- Framework
- PG
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG JS-TS index|Back to index]]
# Decorator
- A decorator can be defined as follows: 
	```TS
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
# NestJS commands
- To generate a new module
	- `nest g resource [module name]`
# Keep in mind
##### Before you integrate
- Always review your entities and DTOs clearly, and think, do these fields come as data from the frontend? When? From where? or does this data get generated in the backend itself, and should be added to what's in the frontend DTO when it arrives?
##### Combining operations into a transaction
-  When combining two endpoints — e.g., a large DTO arrives at one endpoint containing data for multiple related resources (like _Listing_ and _Vehicle_) — the backend endpoint may need to call a service from another module.  
    Normally, each request runs as a **separate database operation**, but sometimes you want both to occur within a **single transaction** (a set of operations that succeed or fail together).  
    In that case, you use a **QueryRunner** (or **query manager**) to start and end that transaction manually.
- To do this, inject the **DataSource** in your service’s constructor.  
    The DataSource provides access to the global TypeORM context (i.e., all registered entities).  
    From it, you can create a **QueryRunner**, which in turn exposes a **manager** object — this manager operates within the transaction scope.
- However, if the second module (e.g., `VehicleModule`) already defines its own service (`VehicleService`) and uses its repository normally, that service is isolated within its own context.  
    To make it participate in your custom transaction, you must modify it to **accept a `manager` parameter** so that its operations use the same transaction instead of creating a new one.
 ```TS
 @Injectable()
export class VehicleService {
  constructor(
    @InjectRepository(Vehicle)
    private readonly vehicleRepo: Repository<Vehicle>,
  ) {}
	  async create(
    createVehicleDto: CreateVehicleDto,
    manager?: EntityManager,
  ): Promise<Vehicle> {
    // Allow the function to be included in a transaction made in another endpoint
    // with another typeORM injection
    const repo = manager ? manager.getRepository(Vehicle) : this.vehicleRepo;

	  ```

and in the first module, you will use it like this:

```ts
@Injectable()
export class ListingService {
  constructor(
    private readonly dataSource: DataSource,
    private readonly vehicleService: VehicleService,
  ) {}

  async create(createListingDto: CreateListingDto) {
    const queryRunner: QueryRunner = this.dataSource.createQueryRunner();

    // Establish database connection
    await queryRunner.connect();
    await queryRunner.startTransaction();

    try {
      // Check if listing exists
      const existingListing = await queryRunner.manager.findOne(Listing, {
        where: { vehicle: { vin: createListingDto.vehicle.vin } },
      });

      if (existingListing) {
        throw new ConflictException(
          `A listing for this vehicle already exists.`,
        );
      }

      // Create vehicle
      const vehicle = await this.vehicleService.create(
        createListingDto.vehicle,
        queryRunner.manager,
      );

      // Create listing
      const listing = queryRunner.manager.create(Listing, {
        reservePrice: createListingDto.reservePrice,
        buyNowPrice: createListingDto.buyNowPrice,
        duration: createListingDto.duration,
        startTime: createListingDto.startTime,
        endTime: createListingDto.endTime,
        status: createListingDto.status,
        vehicle,
        // user: { id: createListingDto.userId },
      });
```

This also means:
- You need to **import the second module’s service** and **register its entity** with `TypeOrmModule.forFeature()` in your current module.

```ts
@Module({
  imports: [TypeOrmModule.forFeature([Vehicle]), VehicleModule],
  controllers: [ListingController],
  providers: [ListingService],
})
export class ListingModule {}
```

- You must **export** whatever the other module should expose (e.g., its service or entity) so that it’s available when imported.
- Remember: in NestJS, a module only has access to **what another module explicitly exports**, not its entire internal scope.

```ts
@Module({
  imports: [TypeOrmModule.forFeature([Vehicle])],
  controllers: [VehicleController],
  providers: [VehicleService],
  exports: [VehicleService, TypeOrmModule],
})
export class VehicleModule {}
```