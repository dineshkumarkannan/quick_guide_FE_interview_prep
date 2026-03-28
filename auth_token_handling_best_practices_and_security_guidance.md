In a production-ready application, authentication state should be handled with both user experience and security in mind. Here are best practices and security guidance:

### 1. Do not store access tokens in Redux or localStorage:
- Access tokens are sensitive. Storing them in Redux or localStorage exposes them to XSS attacks.
- Instead, use httpOnly, secure cookies for tokens. These are not accessible via JavaScript and are sent automatically with requests.

### 2. Use refresh tokens with httpOnly cookies:
- Store a short-lived access token in memory (not persisted).
- Store a long-lived refresh token in an httpOnly cookie. When the access token expires, use the refresh token to get a new one.

### 3. On page refresh:
- On app load, attempt to refresh the access token using the refresh token (via a secure cookie and a backend endpoint).
- If refresh succeeds, update the in-memory access token and allow the user to continue.
- If refresh fails (token expired or invalid), log the user out and redirect to login.

### 4. Logout:
- Clear the access token from memory and instruct the backend to clear the refresh token cookie.

### 5. Security guidance:
- Always use HTTPS.
- Set cookies with Secure, SameSite=Strict, and httpOnly flags.
- Never expose tokens to JavaScript if possible.
- Validate tokens on the backend for every request.
- Implement CSRF protection for sensitive endpoints.

### Summary:
- Use `httpOnly` cookies for refresh tokens.
- Store access tokens in memory only.
- Refresh tokens on app load to maintain sessions.
- Never store tokens in localStorage/sessionStorage for production.

