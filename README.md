<h1 align='center'>Frontend Documentation</h1>
<br>

<h2>Technologies</h2>
<ul>
  <li>React</li>
  <li>TypeScript</li>
  <li>Redux Toolkit</li>
  <li>Styled Components</li>
</ul>

<h2>Deploy</h2>
<ul>
  <li>
    Render — hosting for the frontend as a static site.
  </li>
  <li>
    All <code>/api/*</code> requests are forwarded to the backend via reverse proxy, ensuring same‑origin communication.
  </li>
  <li>
    All non‑API routes are rewritten to <code>/index.html</code>, enabling the SPA to manage client‑side routing.
  </li>
</ul>

<h2>Setup</h2>
<ul>
  <li>
    Vite — provides fast project setup and optimized production builds.  
  </li>
  <li>
    TypeScript config — enforces strict type‑checking and improves project stability.
  </li>
  <li>
    ESLint config — maintains reliable linting rules and consistent code quality.
  </li>
  <li>
    Prettier — ensures uniform formatting across the entire codebase.
  </li>
</ul>

<h2>Architecture</h2>
<ul>
  <li>
    The architecture follows a clean, modular structure with a clear separation of concerns across the application.
  </li>
  <li>
    Core logic is structured in responsibility‑based folders, with related functionality grouped consistently.
  </li>
  <li>
    Barrel files centralize exports, providing consistent and straightforward access to modules within each folder.
  </li>
  <li></li>
  <li>
    Global UI elements — modals, loaders, notifications — are mounted through dedicated portal roots to avoid interfering with the main layout, and they operate independently from the route‑level rendering tree.
  </li>
  <li>
    Responsive Context — a global context exposes device type and pixel density, allowing components to adapt layout, assets, and behavior dynamically.
  </li>
</ul>

<h2>UX & Accessibility</h2>
<ul>
  <li>
    LQIP Approach — blurred low‑quality previews improve perceived loading speed and provide a smoother visual transition.
  </li>
  <li>
    Loading indicators integrated throughout the interface clarify ongoing processes and keep the experience predictable.
  </li>
  <li>
    Notifications deliver immediate feedback for key user actions, helping maintain clarity and reducing uncertainty.
  </li>
  <li>
    Transitions shape a smoother interaction flow and keep the interface fluid and natural.
  </li>
  <li>
    Animations enhance visual perception and make interactions feel more dynamic and engaging.
  </li>
  <li>
    Semantic roles and live regions are applied across the interface to ensure proper announcements and accessible navigation for assistive technologies.
  </li>
  <li>
    Keyboard navigation is fully supported, with clear focus-visible states that keep interactions predictable and accessible.
  </li>
  <li>
    Modal interactions follow accessible patterns, including controlled focus behavior, consistent dismissal rules, and full isolation from the underlying layout.
  </li>
  <li>
    Form behavior provides a clear and guided submission flow, helping users enter information confidently while the interface manages validation, limits, and action availability.
  </li>
</ul>

<h2>UI & Styling</h2>
<ul>
  <li>
    Styling is managed with styled‑components, providing predictable behavior, clean component structure, and scoped styles. 
  </li>
  <li>
    Cross‑browser compatibility is achieved through modern‑normalize, custom reset rules, and manual vendor prefixes for consistent rendering across browsers.
  </li>
  <li>
    Responsive Design — built on a mobile‑first foundation, the interface adapts seamlessly across devices and screen sizes.
  </li>
  <li>
    Responsive Assets — images are served in 1x/2x resolutions and breakpoint‑specific variants for optimal visual quality on all displays.
  </li>
  <li>
    Images are manually optimized with Squoosh, converted to modern WebP formats, and compressed to achieve a balanced quality–size ratio.
  </li>
  <li>
    Videos are processed with HandBrake and re‑encoded using H.264 to ensure reduced file size and reliable cross‑browser playback.
  </li>
  <li>
    Media Delivery — all media assets are delivered through Cloudinary, benefiting from automatic format optimization, smart compression, and CDN‑level performance.
  </li>
  <li>
    Sprite Technique — all vector icons are consolidated into a single SVG sprite to reduce network requests and keep icon management consistent and efficient.
  </li>
</ul>

<h2>Routing</h2>
<ul>
  <li>
    The project runs as a Single Page Application, with React Router managing all client‑side navigation.
  </li>
  <li>
    Restricted Routes — pages accessible only when the user is not authenticated, with a redirect guard preventing access once a session is active.
  </li>
  <li>
    Protected Routes — core application pages become available only after authentication, and unauthorized access is automatically redirected to the login flow.
  </li>
  <li>
    Fallback Route — any unmatched path is routed to a dedicated Not Found page, ensuring consistent handling of invalid URLs.
  </li>
</ul>

<h2>State Management</h2>
<ul>
  <li>
    Redux — manages the application’s global state in a centralized way.
  </li>
  <li>
    Redux Toolkit — simplifies the Redux setup with slice‑based logic and reduced boilerplate.
  </li>
  <li>
    Redux Persist — persists the application state across sessions and page reloads.
  </li>
</ul>

<h2>API Layer</h2>
<ul>   
  <li>
    The API layer operates through an Axios instance that handles all backend requests, configured with a baseURL and credential support for authenticated communication.
  </li>
  <li>
    Request Interceptor — looks for a session token in session storage and, if present, includes it in the Authorization header as a Bearer token.
  </li>
  <li>
    Response Interceptor — stores the session token if the server returns one and triggers an automatic logout when a 401 error occurs.
  </li>
  <li>
    Session Logic — the server issues secure HTTP‑Only cookies to handle authentication and assigns each browser tab a unique session identifier, ensuring a single active session per user and proper isolation across tabs.
  </li>
</ul>

<h2>Other Details</h2>
<ul>
  <li>
    Local caching applies user ownership and TTL‑based invalidation to maintain fresh, isolated, and reliable client‑side data.
  </li>
  <li>
    Font and media delivery are optimized through preconnect and DNS-prefetch directives, reducing latency for Google Fonts and Cloudinary assets.
  </li>
</ul>
