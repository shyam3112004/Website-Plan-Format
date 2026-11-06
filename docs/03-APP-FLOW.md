<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 03 — App Flow

# App Flow
**Related:** [PRD](./01-PRD.md) · [TRD](./02-TRD.md) · [Authentication](./06-AUTHENTICATION.md)

## 1. Site Map
```
/ (Home)
├── #intro           Hero title + scroll hint
├── #scroll-section  Sticky canvas + overlays (the main experience)
├── #features        Static feature content
├── #signup          Lead capture form
└── footer           Links: Privacy, Terms
/privacy
/terms

/admin            Sign-in + dashboard (noindex, not linked from the public site)
/admin/leads      Leads viewer (admin)
/admin/audit      Audit log (admin)
```

## 2. Visitor Journey
```mermaid
flowchart TD
    A[Visitor lands on /] --> B{prefers-reduced-motion?}
    B -- Yes --> R[Static mode: poster image + stacked text]
    B -- No --> C[Show loader, lock scroll]
    C --> D[Fetch page config]
    D --> E{Mobile?}
    E -- Yes --> F[Load mobile frames]
    E -- No --> G[Load desktop frames]
    F & G --> H{Keyframes loaded?}
    H -- No, >10s --> I[Show 'Continue anyway']
    I --> J
    H -- Yes --> J[Hide loader, unlock scroll]
    J --> K[Intro section]
    K --> L[Scroll section: video scrubs + overlays]
    L --> M[Features section]
    M --> N[Signup form]
    R --> N
    N --> O{Valid + consent?}
    O -- No --> N
    O -- Yes --> P[POST /api/leads]
    P --> Q[Success state]
```

## 3. Loader State Machine
```mermaid
stateDiagram-v2
    [*] --> Idle
    Idle --> FetchingConfig: DOMContentLoaded
    FetchingConfig --> LoadingFrames: config ok / fallback
    LoadingFrames --> Ready: keyframes loaded
    LoadingFrames --> Stalled: 10s timeout
    Stalled --> Ready: user clicks "Continue"
    Ready --> BackgroundFill: remaining frames load
    BackgroundFill --> Complete
    Complete --> [*]
```

## 4. Scroll → Render Loop
```mermaid
sequenceDiagram
    participant U as User
    participant L as Lenis
    participant ST as ScrollTrigger
    participant C as scrollCanvas
    participant O as overlays
    U->>L: wheel / touch
    L->>ST: scroll event (smoothed)
    ST->>C: progress 0–1
    C->>C: frame = round(progress × (count-1))
    C->>C: drawImage(nearestLoaded(frame))
    ST->>O: timeline.progress(progress)
    O->>O: fade overlays in/out
```

## 5. Scroll Section Timeline (default content)
```
progress  0.0 ──── 0.25 ──── 0.50 ──── 0.75 ──── 1.0
frames    1 ───────────────────────────────────── 150
overlay1  [██ Introducing X ██]
overlay2            [██ Designed for performance ██]
overlay3                      [██ Every detail ██]
overlay4                                [██ Available now + CTA ██]
canvas                                        [scale 1 → 0.85]
analytics ▲25%       ▲50%       ▲75%       ▲100%   (scroll_milestone)
```

## 6. Lead Form Flow
```mermaid
sequenceDiagram
    participant V as Visitor
    participant F as leadForm.js
    participant API as /api/leads
    participant DB as Supabase
    V->>F: Submit (email, name, consent)
    F->>F: Client validation
    alt invalid
        F-->>V: Inline errors
    else valid
        F->>API: POST JSON
        API->>API: Honeypot + rate limit + validate
        alt rejected
            API-->>F: 400 / 429
            F-->>V: Error message
        else ok
            API->>DB: INSERT lead (upsert on email)
            API-->>F: 201
            F-->>V: "You're on the list" ✓
            F->>API: track('lead_submit')
        end
    end
```

## 7. Editor & Admin Flow

### 7.1 Sign-in
```mermaid
sequenceDiagram
    participant E as Editor
    participant A as /admin (admin.html)
    participant S as Supabase Auth
    participant API as /api/admin/*
    participant DB as Postgres
    E->>A: Enter email
    A->>S: signInWithOtp(shouldCreateUser: false)
    A-->>E: "If your email is registered, a link is on its way"
    S-->>E: Magic link email
    E->>A: Click link (PKCE code in URL)
    A->>S: Exchange code → session (JWT)
    A->>API: GET /api/admin/me (Bearer JWT)
    API->>S: getUser(JWT)
    API->>DB: SELECT role, disabled FROM editors
    alt not an editor, or disabled
        API-->>A: 403
        A->>S: signOut()
        A-->>E: "You don't have access"
    else admin without MFA
        API-->>A: 403 mfa_required
        A-->>E: TOTP enroll / challenge
    else ok
        API-->>A: { id, email, role }
        A-->>E: Admin dashboard
    end
```

### 7.2 Request authorization
```mermaid
flowchart TD
    R[Request to /api/admin/*] --> T{Bearer token?}
    T -- No --> U[401 unauthenticated]
    T -- Yes --> V{getUser valid?}
    V -- No --> U
    V -- Yes --> L{Row in editors and not disabled?}
    L -- No --> F[403 forbidden]
    L -- Yes --> P{Route needs admin?}
    P -- No --> OK[Handle request, write audit_log]
    P -- Yes --> Q{role = admin and aal2?}
    Q -- No --> F
    Q -- Yes --> OK
```

### 7.3 Editor lifecycle
```mermaid
flowchart LR
    A[Admin: POST /api/admin/editors] --> B[Supabase invite email]
    B --> C[Editor clicks link, signs in]
    C --> D[Edits draft content]
    D --> E[Publish → status = published]
    E --> F[API cache expires ≤ 5 min]
    F --> G[Live site shows new copy]
    A --> H[Admin disables editor → blocked on next request]
    A --> I[Leads → view / export CSV / delete]
```

Until FR-12 to FR-16 ship, editors use the Supabase dashboard instead (FR-11): edit the `overlays` table, set the page `status = published`, export leads as CSV.

## 8. Error & Edge States
| Situation | What the user sees |
|---|---|
| JS disabled | `<noscript>` poster + all text |
| API unavailable | Normal experience from static JSON |
| Slow network | Progress bar → "Continue anyway" button |
| Window resized mid-scroll | Canvas resizes, frame redrawn, ScrollTrigger refreshes |
| Rotating a mobile device | Same as resize; frame set does not switch mid-session |
| Duplicate email | Success message (no leak of existing emails) |
| Not signed in at `/admin` | Sign-in form |
| Unknown or unregistered email at sign-in | Same message as a registered email |
| Disabled editor, or not in `editors` | "You don't have access", then signed out |
| Admin without MFA | TOTP enroll / challenge step |
| Expired session | Silent refresh; if that fails, back to sign-in |

---
