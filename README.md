# shouter

shouter is a functional web application framework for Deno and Node.js.

## Installation

## Examples

Basic implementation:
```typescript
import ws from 'ws';
import shouter from 'shouter';

// Setup websocket server
const wsServer = new ws.Server({ noServer: true });

// Create the router function
const router = shouter(wsServer);


// Define routes, the Route type is as follows:
//     type Route<P extends Object, R> = { parameters: P, response: R };
// This is used to force the server and client to have the same types for each route
interface ExampleRoutes extends Routes<any, any> {
    "hello_world": Route<{}, 'hello world'>,
    "foo": Route<{ z: boolean }, 'bar'|'baz'>
}

// Define individual route handlers
const onHelloWorld = () => of('hello world');
const onFoo = ({ z }) => of(z ? 'baz' : 'bar');

// Create the router and use ExampleRoutes
router<ExampleRoutes>(
    'hello_world' -> onHelloWorld,
    'foo'         -> onFoo
);
```

Streamlined:
```typescript
import ws from 'ws';
import shouter from 'shouter';
import {
    type ExampleRoutes,
    onHelloWorld,
    onFoo
} from '@backend-routes';

pipe(
    new ws.Server({ noServer: true }),
    shouter,
    router => router<ExampleRoutes>(
        'hello_world' -> onHelloWorld,
        'foo'         -> onFoo
    )
);

```
