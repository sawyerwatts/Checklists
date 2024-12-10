# Web APIs

Shoutout to [StickFigure](https://github.com/stickfigure) for their amazing
article on REST API design
([here](https://github.com/stickfigure/blog/wiki/How-to-%28and-how-not-to%29-design-REST-APIs)).
This has a ton of great ideas, many of which are referenced here.

## Operation-Level

- Ensure the operation is versioned appropriately
- Ensure the operation handles content negotiation*
    - `Content-Type`: `415: Unsupported media type`
    - `Accept`: `406: Not acceptable`
- Ensure POST operations utilize the idempotency middleware
- Ensure the operation makes use of pagination
- Ensure IDs are always strings, even if they're ints in the DB. This will help
the API evolve without breaking changes
    - If you're feeling spicy, prepend IDs with the containing entity, like
    `"era_00000001"`
- Ensure `404: Not Found` isn't used when returning an empty result on a real
endpoint - consider returning an empty message with `200: OK` or `410: Gone`.
- Ensure the operation has sufficient auth*
- Ensure the operation is in the OpenAPI spec, and possibly the hypothetical
example `.http`
- Ensure the operation's health is reflected in the health check endpoint
- Ensure the operation make relevant use of the `ETag` and `Cache-Control`
headers, and potentially the `If-Match`, `If-None-Match`, and
`If-Modified-Since` headers
    - [Understanding Cache-Control and ETag for efficient web caching](https://dev.to/andreasbergstrom/understanding-cache-control-and-etag-for-efficient-web-caching-2nf5)

\* There is a decent chance this is deferred to the app-level checklist.

## App-Level

- Ensure a healthcheck endpoint exists (and has proper access management, if
accessible at all)
- Ensure an OpenAPI spec is accessible (if not a Swagger webpage), at least in
lower environments
    - An example `http` file isn't a terrible idea too

### Middleware

Some of these would generally be best implemented in the API gateway, but they
aren't there, then implementing them in the request pipeline would be a good
alternative.

- Ensure panic protection in place
    - Ensure the response body of a 500 is obfuscated*
    - Ensure the response body (pre-obfuscation) is logged
- Ensure these are logged (with the logger used by the rest of the app)
    - Request received timestamp
    - Request verb
    - Request host name
    - Request path
    - Request client IP
    - Request client identity (if using auth)
    - Response sent timestamp
    - Response latency
    - Response status code
- Ensure a trace UUID middleware is in place, and is added to the logging scope
very early
- Ensure Authentication and authorization is in place (API key, JWT, etc)
    - API keys: don't forget to perform cryptographically secure string comparisons!
    - JWT checks
        - Come from the correct authority
        - Is sent to the correct audience
        - Ensure that the token is active with a configurable clock skew
        - Use an algorithm from the configurable list (explicitly not allowing
        blank or none or whatever that one is)
        - Use the issuer signing key to validate the JWT's signature
        - Is sent with HTTPS metadata
- Ensure a durable idempotency middleware is available to POST operations
- Ensure request timeouts are implemented (comprehensive, read, write, idle)
- Ensure request sizes are limited*
- Ensure rate limiting is implemented* (`429: Too many requests`)
- Ensure backpressure limiting is implemented* (`503: Service Unavailable`)

\* There is a decent chance this is deferred to the API gateway.

