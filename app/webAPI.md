# Web APIs

Shoutout to [StickFigure](https://github.com/stickfigure) for their amazing
article on REST API design
([here](https://github.com/stickfigure/blog/wiki/How-to-%28and-how-not-to%29-design-REST-APIs)).
This has a ton of great ideas, many of which are referenced here.

## App-Level

- Ensure a healthcheck endpoint exists (and has proper access management, if
accessible at all)
- Ensure an OpenAPI spec is accessible (if not a Swagger/Scalar/etc webpage), at
least in lower environments
    - An example `http` file isn't a terrible idea too

### Middleware

Some of these would generally be best implemented in the API gateway, but they
aren't there, then implementing them in the request pipeline would be a good
alternative.

- Ensure panic protection in place
    - Ensure the response body of a 500 is obfuscated*
        - If not, probably want a problem details. See below for more
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
- If using problem details, may want middleware to log those on the egress, but
  also that could potentially be a security concern based off your app
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
- Ensure request timeouts are implemented (comprehensive, read, idle)
- Ensure request sizes are limited*
- Ensure rate limiting is implemented* (`429: Too many requests`)
- Ensure backpressure limiting is implemented* (`503: Service Unavailable`)

\* There is a decent chance this is deferred to the API gateway.

## Operation-Level

- Ensure the operation handles content negotiation*
    - `Content-Type`: `415: Unsupported media type`
    - `Accept`: `406: Not acceptable`
- Ensure POST operations utilize the idempotency middleware
- Ensure the operation makes use of pagination
- Ensure IDs are always strings, even if they're ints in the DB. This will help
the API evolve without breaking changes
    - If you're feeling spicy, prepend IDs with the containing entity, like
    `"era_00000001"`
- Ensure `ISO8601` strings are used for timestamps
    - Dates: `2024-12-10`
    - Times: `18:59:38Z`, `T185938Z`
    - DateTimes in UTC: `2024-12-10T18:59:38Z`, `20241210T185938Z`
    - DateTimes not in UTC: `2024-12-10T06:59:38−12:00 UTC−12:00`,
    `2024-12-10T18:59:38+00:00 UTC+00:00`, `2024-12-11T06:59:38+12:00 UTC+12:00`
- Ensure objects are returned from operations, never arrays: arrays are much
harder to make backwards compatible changes to
- Ensure *nested* maps are used as little as possible; instead, favor lists of
objects. This allows for more flexible schemas that don't require nearly as many
breaking changes
- Ensure backend-oriented services implement *proposed* standard RFC 9457. See
below for more
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
- Ensure the operation is versioned appropriately

\* There is a decent chance this is deferred to the app-level checklist.

### RFC 9457: Problem Details

[RFC 9457: Problem Details](https://www.rfc-editor.org/rfc/rfc9457.html)

- Altho this spec doesn't seem to play nice with batch operations, and it is
  just a proposal, sooo
- Request `Accept: application/json, application/problem+json`
- Response `Content-Type: application/problem+json`

#### Schema

Basically everything is optional, and since this is a proposed standard, I've
observed that it's definitely more of a guideline, especially with regards to
how `type` is supposed to be a URI.

- `type`
    - This is a URI (preferrably absolute) for a resource to an
    explanation about the error, such as `fake.com/example-problem` or
    `/example-problem`. Basically the type of the problem detail
    - The URIs don't need to be resolvable, but it's recommended in case you
    wanted to add a spec there later
    - The client is to consider this the key of the object
    - If a server doesn't supply, then value `about:blank` is to be assumed by
    the client
- `status`: double track the response code, if you want. This is supposedly
helpful when problem details are cascaded through systems
- `title`: "string containing a short, human-readable summary of the problem
type... the "title" string is advisory and is included only for users who are
unaware of and cannot discover the semantics of the type URI (e.g., during
offline log analysis).
- `detail`: this contains a human-oriented explanation of the problem.
Do *not* programmatically parse this for info - use extensions instead.
- `instance`: a URI (preferrably absolute) to the stored problem detail, or
something. I've also seen people throw the request's verb + URI into this field.
- Additional fields can be added to the response schema to contain
problem-specific details; these are called extensions. These can be whatever
schema you want, with any degree of nesting.

#### Examples

```http
HTTP/1.1 403 Forbidden
Content-Type: application/problem+json
Content-Language: en

{
 "type": "https://example.com/probs/out-of-credit",
 "status": 403,
 "title": "You do not have enough credit.",
 "detail": "Your current balance is 30, but that costs 50.",
 "instance": "/account/12345/msgs/abc",
 "balance": 30,
 "accounts": ["/account/12345",
              "/account/67890"]
}
```

```http
HTTP/1.1 422 Unprocessable Content
Content-Type: application/problem+json
Content-Language: en

{
 "type": "https://example.net/validation-error",
 "status": 422,
 "title": "Your request is not valid.",
 "errors": [
             {
               "detail": "must be a positive integer",
               "pointer": "#/age"
             },
             {
               "detail": "must be 'green', 'red' or 'blue'",
               "pointer": "#/profile/color"
             }
          ]
}
```

