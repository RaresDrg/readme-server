<h1 align='center'>Backend Documentation</h1>
<br>

<h2>Technologies</h2>
<ul>
  <li>Node.js</li>
  <li>Express</li>
  <li>TypeScript</li>
  <li>MongoDB</li>
  <li>Mongoose</li>
</ul>

<h2>Tools</h2>
<ul>
  <li>
    MongoDB Atlas — cloud service for hosting and managing the production database.
  </li>
  <li>
    Resend — email delivery service with custom HTML templates.
  </li>
  <li>
    Google Cloud Platform — authentication setup for Google sign‑in integration.
  </li>
  <li>
    Open Exchange Rates — external API for fetching real‑time currency exchange rates.
  </li>
  <li>
    Swagger — interactive documentation interface for all API routes and data models.
  </li>
  <li>
    Postman — testing environment for validating and debugging all API endpoints.
  </li>
</ul>

<h2>Deploy</h2>
<ul>
  <li>
    Render — hosting for the backend as a web service.
  </li>
  <li>
    The configuration includes the closest available region for reduced latency, production environment variables for secure secret handling, and a <code>/health-check</code> route for internal service checks.
  </li>
  <li>
    UptimeRobot — external uptime monitoring that keeps the server awake and prevents cold starts.
  </li>
</ul>

<h2>Setup</h2>
<ul>
  <li>
    TypeScript config — enforces strict type‑checking and improves project stability.
  </li>
  <li>
    ESLint config — maintains reliable linting rules and consistent code quality.
  </li>
  <li>
    Prettier — ensures uniform formatting across the entire codebase.
  </li>
  <li>
    Nodemon — improves development with automatic server restarts and debugging support.
  </li>
</ul>

<h2>Architecture</h2>
<ul>
  <li>
    The architecture follows a modular REST API design, with a clear separation of concerns across the application.
  </li>
  <li>
    Core logic is structured into dedicated layers, each with a single, well‑defined responsibility.
  </li>
  <li>
    Barrel files centralize exports, ensuring consistent and straightforward access to modules within each layer.
  </li>
  <li>
    Modular Routing — API routes are grouped into distinct modules based on their domain, keeping related endpoints together and the routing layer clean and scalable.
  </li>
  <li>
    Centralized Error Handling — all errors are funneled into a global middleware that acts as the single source of truth for failure responses and prevents any exposure of internal details or sensitive configuration data.
  </li>
</ul>

<h2>Auth & Session Management</h2>
<ul>
  <li>
    The authentication system uses server‑side sessions instead of stateless JWTs to avoid client‑side token persistence, enabling real‑time invalidation and predictable control over the authentication flow.
  </li>
  <li>
    Sessions are stored in MongoDB because Render’s free‑tier environment is filesystem read‑only and prevents Redis from writing to its in‑memory cache.
  </li>
  <li>
    The model includes a TTL index that automatically expires stale sessions, and a compound unique index (owner + type) that limits the user to one active session per type.
  </li>
  <li>
    Each session type — auth and validation — has a distinct security role and dedicated middleware responsible for enforcing it.
  </li>
  <li>
    <b>Auth Session</b>
    <br>
    <span>
      – long‑lived session (24 h) that maintains continuous user authentication.
    </span>
    <br>
    <span>
      – initiated after successful auth flows (register, login, password updates, Google OAuth).
    </span>
    <br>
    <span>
      – removes the previous auth session before creating a new one.
    </span>
    <br>
    <span>
      – issues a secure access/refresh token pair stored in HTTP‑only cookies.
    </span>
    <br>
    <span>
      – exposes the session ID through response headers to facilitate browser tab isolation.
    </span>
  </li>
  <li>
    <b>Auth Session Middleware</b>
    <br>
    <span>
      – manages access to protected routes using the active auth session.
    </span>
    <br>
    <span>
      – extracts the session ID from the Authorization header and the token pair from signed cookies.
    </span>
    <br>
    <span>
      – validates the session by querying the database with the session ID and either the access or refresh token.
    </span>
    <br>
    <span>
      – when validation falls back to the refresh token, its single‑use nature triggers an automatic session renewal.
    </span>
    <br>
    <span>
      – on success, the hydrated session owner is attached to the request for downstream handlers.
    </span>
    <br>
    <span>
      – on failure, the middleware blocks access and returns 401 Unauthorized.
    </span>
  </li>
  <li>
    <b>Validation Session</b>
    <br>
    <span>
      – short‑lived session (15 min) that enforces user identity verification.
    </span>
    <br>
    <span>
      – initiated only when a sensitive account action begins (e.g., password resets).
    </span>
    <br>
    <span>
      – removes the previous validation session before creating a new one.
    </span>
    <br>
    <span>
      – issues a one‑time validation token required to complete the action.
    </span>
  </li>
  <li>
    <b>Validation Session Middleware</b>
    <br>
    <span>
      – controls the execution of sensitive account actions using the active validation session.
    </span>
    <br>
    <span>
      – extracts and verifies the validation token provided in the request body.
    </span>
    <br>
    <span>
      – retrieves the session from the database using the token and immediately consumes it for secure single‑use.
    </span>
    <br>
    <span>
      – on success, the hydrated session owner is attached to the request for downstream handlers.
    </span>
    <br>
    <span>
      – on failure, the middleware blocks execution and returns 404 NotFound.
    </span>
  </li>
</ul>

<h2>Security </h2>
<ul>
  <li>
    The authentication process is secured through hardened session cookies that are protected against JavaScript access, transmitted only over HTTPS, validated by server‑side signatures, and restricted to same‑origin requests.
  </li>
  <li>
    CORS — cross‑origin access is enabled only in development, while production operates under a strict same‑origin behavior and relies on client‑side rewrites to route all browser traffic to the backend.
  </li>
  <li>
    Rate Limiting — the API enforces time‑based request limits to block brute‑force attempts, automated scans, and abusive traffic, ensuring security and stability.
  </li>
  <li>
    Password Hashing — user credentials are protected through bcrypt, which applies salted hashing to keep database storage safe and to reduce brute‑force risks.
  </li>
  <li>
    Security Headers — the server uses Helmet to set industry‑standard HTTP security headers, adding baseline protection against common browser‑level attacks.
  </li>
</ul>

<h2>Validation</h2>
<ul>
  <li>
    The validation strategy follows a two-layer approach designed to validate client input and preserve database integrity.
  </li>
  <li>
    Layer 1: Request validation — all incoming client input (body, query, params) is validated through a centralized and dynamic Joi schema system, acting as the first layer of protection and rejecting invalid or incomplete data before it reaches the business logic.
  </li>
  <li>
    Layer 2: Database validation — Mongoose model‑level rules add an additional protection layer that guarantees data integrity through strict constraints, enums, conditional requirements, custom validators and type‑safe limits, preventing invalid or inconsistent records from ever being persisted.
  </li>
</ul>

<h2>Data Model</h2>
<ul>
  <li>
    Strict mode — unknown or unexpected fields are ignored to protect the model against accidental or malicious data injection.
  </li>
  <li>
    Unique indexes — structural database constraints that enforce uniqueness and prevent duplicated records.
  </li>
  <li>
    Cross‑model relationships — references between models support relational consistency and enable cross‑entity access to connected data.
  </li>
  <li>
    Controlled output shaping — custom serialization rules ensure that only intended fields are exposed in API responses, omitting sensitive or internal data.
  </li>
  <li>
    Data expiration — time‑sensitive records are automatically removed using TTL indexes, keeping the database clean and storage‑efficient.
  </li>
  <li>
    Default values — predefined defaults provide consistent initialization of fields without relying on client input.
  </li>
</ul>

<h2>Other Details</h2>
<ul>
  <li>
    Environment guard — all environment variables are strictly validated at startup, preventing the server from running with missing or invalid configuration.
  </li>
  <li>
    Logging — structured request logging (with custom formatting and selective route filtering) works alongside internal application logs to deliver a clean, easy‑to‑follow activity tracking flow.
  </li>
  <li>
    Cache control — the API implements a no‑cache policy to prevent proxy or browser layers from serving stale responses.
  </li>
  <li>
    Payload handling — incoming write operations accept only JSON payloads, with a strict size limit enforced to prevent excessive request bodies and potential memory abuse.
  </li>
  <li>
    404 Fallback — unmatched routes fall back to a global middleware that returns a clear error response pointing clients toward the API documentation.
  </li>
  <li>
    Cursor‑based pagination — transaction queries use a cursor‑driven approach that keeps pagination stable as the dataset evolves, avoiding the structural limitations of offset‑based pagination.
  </li>
  <li>
    External data caching — data sourced from external APIs is persisted in the database to reduce outbound requests, improve response times, and stay within third‑party usage limits.
  </li>
</ul>
