Frontend Documentation

Technologies
React
TypeScript
Redux Toolkit
Styled Components
Deploy
Render — hosting for the frontend as a static site.
All /api/* requests are forwarded to the backend via reverse proxy, ensuring same‑origin communication.
All non‑API routes are rewritten to /index.html, enabling the SPA to manage client‑side routing.
Setup
Vite — provides fast project setup and optimized production builds.
TypeScript config — enforces strict type‑checking and improves project stability.
ESLint config — maintains reliable linting rules and consistent code quality.
Prettier — ensures uniform formatting across the entire codebase.
Architecture
The architecture follows a clean, modular structure with a clear separation of concerns across the application.
Core logic is structured in responsibility‑based folders, with related functionality grouped consistently.
Barrel files centralize exports, providing consistent and straightforward access to modules within each folder.
Global UI elements — modals, loaders, notifications — are mounted through dedicated portal roots to avoid interfering with the main layout, and they operate independently from the route‑level rendering tree.
Responsive Context — a global context exposes device type and pixel density, allowing components to adapt layout, assets, and behavior dynamically.
UX & Accessibility
LQIP Approach — blurred low‑quality previews improve perceived loading speed and provide a smoother visual transition.
Loading indicators integrated throughout the interface clarify ongoing processes and keep the experience predictable.
Notifications deliver immediate feedback for key user actions, helping maintain clarity and reducing uncertainty.
Transitions shape a smoother interaction flow and keep the interface fluid and natural.
Animations enhance visual perception and make interactions feel more dynamic and engaging.
Semantic roles and live regions are applied across the interface to ensure proper announcements and accessible navigation for assistive technologies.
Keyboard navigation is fully supported, with clear focus-visible states that keep interactions predictable and accessible.
Modal interactions follow accessible patterns, including controlled focus behavior, consistent dismissal rules, and full isolation from the underlying layout.
Form behavior provides a clear and guided submission flow, helping users enter information confidently while the interface manages validation, limits, and action availability.
UI & Styling
Styling is managed with styled‑components, providing predictable behavior, clean component structure, and scoped styles.
Cross‑browser compatibility is achieved through modern‑normalize, custom reset rules, and manual vendor prefixes for consistent rendering across browsers.
Responsive Design — built on a mobile‑first foundation, the interface adapts seamlessly across devices and screen sizes.
Responsive Assets — images are served in 1x/2x resolutions and breakpoint‑specific variants for optimal visual quality on all displays.
Images are manually optimized with Squoosh, converted to modern WebP formats, and compressed to achieve a balanced quality–size ratio.
Videos are processed with HandBrake and re‑encoded using H.264 to ensure reduced file size and reliable cross‑browser playback.
Media Delivery — all media assets are delivered through Cloudinary, benefiting from automatic format optimization, smart compression, and CDN‑level performance.
Sprite Technique — all vector icons are consolidated into a single SVG sprite to reduce network requests and keep icon management consistent and efficient.
Routing
The project runs as a Single Page Application, with React Router managing all client‑side navigation.
Restricted Routes — pages accessible only when the user is not authenticated, with a redirect guard preventing access once a session is active.
Protected Routes — core application pages become available only after authentication, and unauthorized access is automatically redirected to the login flow.
Fallback Route — any unmatched path is routed to a dedicated Not Found page, ensuring consistent handling of invalid URLs.
State Management
Redux — manages the application’s global state in a centralized way.
Redux Toolkit — simplifies the Redux setup with slice‑based logic and reduced boilerplate.
Redux Persist — persists the application state across sessions and page reloads.
API Layer
The API layer operates through an Axios instance that handles all backend requests, configured with a baseURL and credential support for authenticated communication.
Request Interceptor — looks for a session token in session storage and, if present, includes it in the Authorization header as a Bearer token.
Response Interceptor — stores the session token if the server returns one and triggers an automatic logout when a 401 error occurs.
Session Logic — the server issues secure HTTP‑Only cookies to handle authentication and assigns each browser tab a unique session identifier, ensuring a single active session per user and proper isolation across tabs.
Other Details
Local caching applies user ownership and TTL‑based invalidation to maintain fresh, isolated, and reliable client‑side data.
Font and media delivery are optimized through preconnect and DNS-prefetch directives, reducing latency for Google Fonts and Cloudinary assets.
