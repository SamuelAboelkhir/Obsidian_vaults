---
tags: 
- JS-TS
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG JS-TS index|Back to index]]

# Enums
- A TypeScript enum combines two concepts that C keeps more separate: a type-level set of named values and a runtime object containing those values.
- So this
```ts
enum DaysOfWeek {
    MONDAY = "monday",
    TACO_TUESDAY = "taco tuesday",
    WEDNESDAY = "wednesday",
    // ...
}
```
- Is equivalent to this
```C
enum DaysOfWeek {
    MONDAY,
    TACO_TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    FUNDAY,
};

const char *days_of_week[] = {
    [MONDAY] = "monday",
    [TACO_TUESDAY] = "taco tuesday",
    [WEDNESDAY] = "wednesday",
    [THURSDAY] = "thursday",
    [FRIDAY] = "friday",
    [SATURDAY] = "saturday",
    [FUNDAY] = "funday",
};
```
- However, achieving the goal of a runtime object with type safety can still be achieved in another way
```ts
const DaysOfWeek = {
    MONDAY: "monday",
    TACO_TUESDAY: "taco tuesday",
    WEDNESDAY: "wednesday",
} as const;

type Day = typeof DaysOfWeek[keyof typeof DaysOfWeek];
```