# Web APIs

Shoutout to [StickFigure](https://github.com/stickfigure) for their amazing
article on REST API design
([here](https://github.com/stickfigure/blog/wiki/How-to-%28and-how-not-to%29-design-REST-APIs)).
This has a ton of great ideas, many of which are referenced here.

TODO: Refactor this list too

- Ensure an OpenAPI spec is accessible (if not a Swagger webpage), at least in
lower environments
    - An example `http` file isn't a terrible idea too
- Is there a health check endpoint?
- Pagination! Beware of how much load (such as requested objects) you are
allowing clients to inflict on your app.
- How is this endpoint secured?
- How's the content negotiation?
    - Is `Content-Type` being checked? (`415: Unsupported media type`)
    - Is `Accept` being checked and respected? (`406: Not acceptable`)
- Is the API versioned?
- A `404: Not Found` is technically correct, but meaningfully can be ambiguous.
Using `410: Gone` can be clearer.
- Does it make sense for any endpoints to support async or be async-only?
- Does the API make relevant use of the `ETag` and `Cache-Control` headers? How
about `If-Match`, `If-None-Match`, and `If-Modified-Since` headers?
    - [Understanding Cache-Control and ETag for efficient web caching](https://dev.to/andreasbergstrom/understanding-cache-control-and-etag-for-efficient-web-caching-2nf5)
- Use strings for IDs, never ints

## Request Pipeline (Middleware)

Some of these would generally be best implemented in the API gateway, but they
aren't there, then implementing them in the request pipeline would be a good

- Ensure web framework uses same logger as the rest of the app
- Ensure these are logged
    - Request received timestamp
    - Request verb
    - Request host name
    - Request path
    - Request client IP
    - Request client identity (if using auth)
    - Response sent timestamp
    - Response latency
    - Response status code
    - If 500, content body
- Trace UUID is nice, esp when added to logging scope very early
- Authentication and authorization (API key, JWT, etc)
    - Prob want to add user ID to logging scope
    - API keys
        - Don't forget to perform cryptographically secure string comparisons!
    - JWT checks
        - Come from the correct authority
        - Is sent to the correct audience
        - Ensure that the token is active with a configurable clock skew
        - Use an algorithm from the configurable list (explicitly not allowing
        blank or none or whatever that one is)
        - Use the issuer signing key to validate the JWT's signature
        - Is sent with HTTPS metadata
- Is panic protection in place and do they return 500?
    - You don't want to leak stack frames or other incidental data when sending
      a 500, so having panic protection middleware not write the panic is more
    secure
    - However, if you have the http server automatically obfuscate 500s, then
    you won't have to worry about an app forgetting to obfuscate its panics, but
    then you also won't be able to send errors in 500s. This will require either
    sending errors on another status code or not sending errors at all.
- Are POSTs made idempotent (`X-Idempotency-Token`)?
    - `409: Conflict` is generally good when a completed POST is resubmitted
- Are request timeouts implemented (comprehensive, read, write, idle)?
- Is rate limiting impleneted and does it return `429: Too many requests`?
- Is backpressure limiting implemented and does it return `503: Service
unavailable` (altho `429` could work too)?
- Is request size limiting implemented?
    - This would be much preferred to be handled by the API gateway, and NGINX
    and IIS servers both cap request sizes at 30 MB

