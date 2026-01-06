Auth (BFF)

VITE_USER_API_URL=https://user-api.devmojos.com

VITE_USER_API_REALM=callquark (use exactly this env var name/value convention)

Note: earlier note mentioned xema—ignore that; use the latest: callquark.

2) Authentication requirements (Invite-only BFF)

Rule: React app never talks to Keycloak directly.

All auth calls go to:

Base URL: VITE_USER_API_URL

Every request is POST JSON

Every request must include realm (taken from VITE_USER_API_REALM)

Handle errors in the shape: { "error": string }

Required endpoints (BFF)

POST /signin

POST /refresh-token

POST /signout

Required auth files to create

You must create these files exactly:

src/auth/userApiClient.ts

Implements callUserApi():

Adds realm automatically in the JSON body for every auth request

Sends POST with Content-Type: application/json

Base URL from VITE_USER_API_URL

Parses JSON response

On non-2xx:

attempt to read { error } and throw a typed error

Must be reusable for signin/refresh/signout

src/auth/authService.ts

Implements:

signin(credentials) → calls POST /signin (include realm)

signup(...) exists as a function but is not exposed in UI for invite-only (keep as placeholder)

refreshTokens(refreshToken) → calls POST /refresh-token (include realm)

logout(refreshToken|session) → calls POST /signout (include realm)

Assume the BFF returns tokens like:

accessToken

refreshToken

maybe expiresIn / tokenType
If the exact shape is unknown, implement robust handling via runtime checks and keep types flexible.

src/auth/AuthProvider.tsx

Implements an Auth Context Provider that:

stores tokens in memory

provides:

signIn(email,password) (or generic credentials)

signOut()

isAuthenticated

accessToken

on app load:

attempt a “silent refresh” if there is a refresh token in memory (initially none)

if refresh token persistence is desired later, isolate it behind a single interface

Route guard component: RequireAuth

If not authenticated → redirect to /signin

If authenticated → render children

Must work with React Router

4) Auth UI routes
/signin

A dedicated sign-in page:

Minimal form (email + password)

On submit:

calls signIn

handles { error: string } gracefully

On success:

redirect to /workspaces (or last intended route)

Optional: /request-invite

Mock submission page for invite requests

No real backend call required (just store form state / show success toast)

Linked from /signin as “Request invite”