# XFT Authentication Implementation

```
We're adding v1 API endpoints to allow frontend apps to authenticate without requiring the Firebase SDK.

The new endpoints:
/api/v1/signin: Handles email login completely server-side
/api/v1/signup: Creates user accounts and returns ready-to-use tokens
/api/v1/wallet-auth: Processes wallet signatures and manages wallet-based accounts
/api/v1/refresh-token: Extends user sessions without reauthentication
/api/v1/user-profile: Retrieves user data
/api/v1/verify-token: Validates authentication tokens

This works by:
Creating Firebase users on the server
Handling all token generation server-side
Returning tokens directly usable in API requests
Managing session persistence through refresh tokens

Frontend apps can now authenticate with simple fetch calls without importing Firebase libraries, simplifying integration and reducing bundle size.
```

### AUTH SERVER 
Base URL: auth.xft.finance

POST /api/wallet-auth
- Request: address, signature, message
- Response: customToken
- Security: None

POST /api/signup
- Request: email, password
- Response: message, uid
- Security: None

POST /api/verify-token
- Request: None
- Headers: Authorization (Bearer token)
- Response: verified (boolean), user (object)
- Security: Bearer

GET /health
- Request: None
- Response: status, timestamp
- Security: None



## Core Authentication Files

### client/src/lib/auth.ts - Authentication API Integration:
- Create functions to interact with auth.xft.finance
- Implement wallet and email authentication methods
- Handle token storage and management
- Set up token verification and refresh logic

### client/src/contexts/AuthContext.tsx - Authentication State:
- Manage user authentication state
- Provide login/logout methods for both wallet and email
- Leverage existing wallet integration for signing messages
- Store tokens and handle Firebase integration

### client/src/pages/Auth.tsx - Authentication UI:
- Create a login/signup page with two tabs
- Implement wallet authentication using existing wallet.ts
- Add simple forms for email/password login and signup

### client/src/components/shared/ProtectedRoute.tsx - Route Protection:
- Create a component to protect routes requiring authentication
- Redirect unauthenticated users to login
- Handle loading states during token verification

## Integration Points

### App.tsx Changes:
- Wrap entire app in AuthProvider
- Add Auth route for login/signup
- Protect dashboard and admin routes

### Header.tsx Changes:
- Modify to show login button or user info
- Update header state based on auth context
- Add dropdown for authenticated users

### API Integration:
- Update queryClient to include auth tokens in requests
- Add interceptor for authentication headers

## Authentication Flow

### Wallet Authentication:
- Use existing wallet connection
- Sign message "Login to XFT" with wallet
- Call auth.xft.finance/api/wallet-auth with address, signature
- Store returned Firebase custom token
- Sign in to Firebase with custom token

### Email Authentication:
- Call auth.xft.finance/api/signup for registration
- Store Firebase UID
- Use Firebase email/password for authentication

### Token Management:
- Store tokens in localStorage
- Handle token refresh
- Verify tokens with auth.xft.finance/api/verify-token

### Protected Routes:
- Check for valid token before rendering
- Redirect to login if token is invalid or missing
- Support different permission levels if needed

This implementation will create a complete authentication system integrated with the existing wallet functionality while adding email-based authentication for users without wallets. All authentication will flow through the central auth.xft.finance API.

```
|--admin
|--|--01_index.js
|--|--README.md
|--|--script.js
|--|--style.css
|--attached_assets
|--|--image_1744174883423.png
|--|--image_1744174925344.png
|--.cache
|--|--nix
|--|--|--binary-cache-v6.sqlite
|--|--|--binary-cache-v6.sqlite-journal
|--|--replit
|--|--|--env
|--|--|--|--latest
|--|--|--|--latest.json
|--|--|--modules
|--|--|--|--nodejs-22.res
|--|--|--|--replit.res
|--|--|--modules.stamp
|--|--|--nix
|--|--|--|--env.json
|--|--|--__replit_disk_meta.json
|--|--|--toolchain.json
|--|--typescript
|--|--|--5.0
|--|--|--|--package.json
|--|--|--|--package-lock.json
|--|--|--5.3
|--|--|--|--package.json
|--|--|--|--package-lock.json
|--|--|--5.4
|--|--|--|--package.json
|--|--|--|--package-lock.json
|--|--|--5.5
|--|--|--|--package.json
|--|--|--|--package-lock.json
|--|--|--5.6
|--|--|--|--package.json
|--|--|--|--package-lock.json
|--.config
|--|--configstore
|--|--|--firebase-tools.json
|--|--|--update-notifier-firebase-tools.json
|--|--npm
|--|--|--node_global
|--|--|--|--bin
|--|--|--|--lib
|--generated-icon.png
|--index.js
|--.local
|--|--share
|--openapi.json
|--package.json
|--package-lock.json
|--public
|--|--assets
|--|--|--css
|--|--|--|--blockchain.css
|--|--|--|--mobile.css
|--|--|--|--style.css
|--|--|--|--test.css
|--|--|--img
|--|--|--|--favicon.ico
|--|--|--|--favicon.png
|--|--|--|--logo.png
|--|--|--js
|--|--|--|--blockchain.js
|--|--|--|--ethersLoader.js
|--|--|--|--redirect.js
|--|--|--|--script.js
|--|--|--|--validation.js
|--|--index.html
|--|--test2.html
|--|--test.html
|--README.md
|--.replit
|--.upm
|--|--store.json
~/workspace$
```
