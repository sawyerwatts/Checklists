# Web APIs

Shoutout to [StickFigure](https://github.com/stickfigure) for their amazing
article on REST API design
([here](https://github.com/stickfigure/blog/wiki/How-to-%28and-how-not-to%29-design-REST-APIs)).
This has a ton of great ideas, many of which are referenced here.

- When writing RESTful APIs, assuming a statically typed language is used,
ensure the language/framework is fully auto-generating the OpenAPI spec for the
web app, including documentation.
- Is there a health check endpoint?
- Pagination!
- How is this endpoint secured?
- Is `Content-Type` being checked? (`415: Unsupported media type`)
- Is `Accept` being checked and respected? (`406: Not acceptable`)

## Request Pipeline (Middleware)

Some of these would generally be best implemented in the API gateway, but they
aren't there, then implementing them in the request pipeline would be a good

- At ends of request pipeline, def want to log things like
    - Host URL
    - Request verb and URL
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
- Panic protection (catch exceptions and return 500, ideally scrubbed of info)
- Obfuscate payload of 500s (so stack traces and possibly sensitive data in
errors is not leaked)
- Idempotent POSTs (`X-Idempotency-Token`)
    - `409: Conflict` is generally good when a completed POST is resubmitted
- Request timeouts
- Rate limiting: `429: Too many requests`
- Backpressure limiting: `503: Service unavailable` (altho `429` could work too)
- Request size limiting
    - This would be much preferred to be handled by the API gateway, and NGINX
    and IIS servers both cap request sizes at 30 MB

