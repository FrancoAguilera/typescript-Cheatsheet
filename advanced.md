# Advanced types

### Any
A silver bullet at first glance, but not..., more like the beginning of bigger problems

### Unknown
Almost the same type as any with the subtle difference that does a type check for operations
e.g.:

```typescript
  if (typeof a === 'number' && typeof b === 'number') {
    return a + b;
  }

  if (typeof a === 'string' && typeof b === 'string') {
    return a + b;
  }

```
The above example might like like it does the same in both cases, but not. on the first if it does an addition, on the second case will make a concatenation


### Void
Typically, you use the void type as the return type of functions that do not return a value. For example:
```typescript
function log(message): void {
    console.log(messsage);
}
```
**void** is used where there is no data. For example, if a function does not return any value then you can specify void as return type.

### Never
Indicates the values that will never occur, and it's used for type checking, and **Exhaustive switch**
The never type is used when you are sure that something is never going to occur. For example, you write a function which will not return to its end point or always throws an exception. 

```typescript
function throwError(errorMsg: string): never { 
  throw new Error(errorMsg); 
} 

function keepProcessing(): never { 
  while (true) { 
    console.log('I always does something and never ends.')
  }
}
```

## Exhaustive switch example
To do this, we'll use the **never** type which represents values which "shouldn't" occur.
```typescript
function assertUnreachable(x: never): never {
  throw new Error("Didn't expect to get here");
}

function getColorName(c: Color): string {
  switch(c) {
    case Color.Red:
      return 'red';
    case Color.Green:
      return 'green';
  }
  return assertUnreachable(c);
}


```


## Advanced example 
```typescript
type QueryOptions = {
  table: "user" | "widget" | "session"; // not so good 
  userId?: string;
  widgetId?: string;
  sessionId?: string;
  offset: number;
  page: number;
};


type QueryType = { // will assert value type more correct
  offset: number;
  page: number;
} & ({
  table: "user";
  userId: string;
} | {
  table: "widget";
  widgetId: string;
} | {
  table: "session";
  sessionId: string;
})
```