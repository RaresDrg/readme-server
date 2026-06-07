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
    Modular Routing — API routes are organized into domain‑specific modules, grouping related endpoints to keep the routing layer clean, predictable, and easy to extend.
  </li>
  <li>
    Centralized Error Handling — all errors are funneled into a global middleware that acts as the single source of truth for failure responses, providing uniform and human‑readable outputs.
  </li>
</ul>

<h2>Auth & Session Management</h2>
<ul>
  <li>
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
    Layer 2: Database validation —
  </li>
</ul>

<h2>Other Details</h2>
<ul>
  <li>
    Environment guard — env variables are strictly validated at startup to prevent the server from running with missing or invalid configuration.
  </li>
  <li>
    Controlled output shaping — Mongoose schemas apply custom serialization rules to ensure that only intended fields are exposed in API responses, while sensitive or internal data is omitted.
  </li>
  <li>
    Data expiration — Time‑sensitive records are automatically removed via MongoDB TTL indexes, ensuring clean, up‑to‑date storage without any manual maintenance.
  </li>
</ul>
