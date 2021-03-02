## About

This is a template for [OpenAPI Generator](https://openapi-generator.tech/), which allows generating server stubs for TypeScript and Express.

## Usage

Each tag results in a separate API class.  Let's say you have a `GET` method that looks like this:

```
/games/{gameId}:
  get:
    tags:
      - game
    operationId: getGame
    parameters:
      - name: gameId
        in: path
        required: true
        schema:
          type: string
    responses:
      '200':
        description: successful operation
        content:
          application/json:
            schema:
              $ref: '#/definitions/Game'
```

Run the OpenAPI generator using the `typescript-fetch` generator, pointing to this template, something like:

```
openapi-generator-cli generate -i ./api.yaml -g typescript-fetch -t /path/to/openapi-typescript-server -o ./output
```

The generator will produce a class that looks like this:

```
abstract class GameApiBase {
    router = Express.Router()

    protected abstract getGame(params: GetGameParams context: runtime.Context): Promise<Game>;

    constructor() {
        <Some logic>
    }
}
```

This abstract class will handle Express requests matching the API definitions.  Any calls to the `getGame` API will end up calling the abstract `getGame` method in this class.

To provide the API implementation, all you need to do is implement the abstract methods:

```
class GameApi extends GameApiBase {
    protected async getGame(params: GetGameParams context: runtime.Context): Promise<Game> {
        <API logic>
    }
}
```

and then hook the router up to your Express server:

```
const app = Express();

const gameApi = new GameApi();
app.use('/', gameApi.router);
```


## Authors

Matt Adam

## License

[MIT](LICENSE.md)
