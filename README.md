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
    Centralized Error Handling — all errors are funneled into a global middleware that acts as the single source of truth for failure responses, providing uniform and human‑readable outputs.
  </li>
</ul>

<h2>Auth & Session Management</h2>
<ul>
  <li>
    The authentication system uses sessions instead of stateless JWTs to enable real invalidation, automatic rotation, secure renewal, and predictable control over authentication state.
  </li>
  <li>
    Sessions are stored in MongoDB, since Render’s free‑tier environment is read‑only and prevents Redis from writing to its in‑memory cache.
  </li>
  <li>
    The model includes a TTL index that automatically expires stale sessions, and a compound unique index (owner + type) that limits each user to one active session per type.
  </li>
  <li>
    Each session type — auth and validation — has a specific security role and dedicated middleware responsible for enforcing it.
  </li>
  <br>
  <li>
    <b>Auth Session (24 h)</b>
    <br>
    <span>
      – long‑lived session that maintains continuous user authentication.
    </span>
    <br>
    <span>
      – initiated after successful auth flows (register, login, password updates, Google OAuth).
    </span>
    <br>
    <span>
      – enforces a single active auth session per user, by replacing the previous one (if present).
    </span>
    <br>
    <span>
      – issues a secure access/refresh token pair stored in HTTP‑only cookies.
    </span>
    <br>
    <span>
      – exposes the session ID through response headers to enable browser tab isolation.
    </span>
  </li>
  <br>
  <li>
    <b>Auth Session Middleware</b>
    <br>
    <span>
      – validates the active auth session on protected routes.
    </span>
    <br>
    <span>
      – reads the session ID from the Authorization header (&#96;Bearer &lt;sessionId&gt;&#96;).
    </span>
  </li>
</ul>

<h2>Security </h2>
<ul>
  <li>
    The authentication process is secured through hardened session cookies that are protected against JavaScript access, transmitted only over HTTPS, validated through server‑side signatures, and rely on same‑origin requests for safe and predictable / protected session handling.
  </li>
  <li>
    CORS
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
    Cursor‑based pagination — the API benefits from a cursor‑driven approach that keeps pagination stable and predictable as the dataset evolves, offering a more reliable way to navigate the data.
  </li>
  <li>
    Logging — structured request logging with custom formatting and selective route handling is combined with internal application logs to provide a clean, consistent and easy‑to‑debug production output.
  </li>
  <li>
    Fallback 404 — any unmatched routes are captured by a global middleware that handles invalid requests and guides the client toward the API documentation.
  </li>
</ul>
