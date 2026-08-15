# Spoural Games — Authentication Design

## 1. Purpose

The authentication system establishes and maintains the
authenticated identity of users accessing the Spoural Games
platform.

## 2. Authentication Model

Spoural will use a server-side session model with secure
browser cookies for authenticated sessions.

Authentication decisions are always performed by the backend.

## 3. Login

The login process will:

1. Validate submitted credentials.
2. Find the corresponding user.
3. Check the account status.
4. Verify the submitted password against the stored password
   hash.
5. Create a new authenticated session.
6. Store the session information server-side.
7. Send a secure authentication cookie.
8. Record the appropriate security event.
9. Return only necessary user information.

## 4. Password Storage

Passwords must never be stored in plaintext.

The preferred password hashing algorithm for the final system
is Argon2id.

bcrypt may be used during development or where Argon2id is
not available, subject to appropriate configuration.

## 5. Session Storage

Authenticated sessions will be represented by server-side
session records.

Session records will include:

- session identifier/derived value
- user identifier
- creation time
- expiration time
- last activity time
- revocation time

Raw session secrets must not be stored unnecessarily.

## 6. Cookie Security

Production authentication cookies must use appropriate:

- HttpOnly
- Secure
- SameSite
- Path

attributes.

The exact SameSite configuration will be selected based on
the final deployment architecture.

## 7. HTTPS

Production authentication and authenticated application
traffic must use HTTPS.

## 8. Logout

Logout must:

1. Revoke the server-side session.
2. Clear the browser authentication cookie.
3. Record the appropriate security event.

## 9. Password Reset

Password reset tokens must:

- be cryptographically random
- expire
- be single-use
- not reveal unnecessary account information
- result in appropriate session invalidation after a
  successful password change

## 10. Administrator Security

Production administrator accounts should support
multi-factor authentication.

## 11. Failed Authentication

Failed login attempts must return safe generic responses
and must not unnecessarily reveal whether an account exists.

Authentication-related failures should be logged as
security events.

## 12. Session Lifecycle

Sessions must have an appropriate expiration policy and
must support explicit revocation.

Sensitive authentication state must not be stored in
localStorage or sessionStorage.

## 13. Reauthentication

Sensitive account and administrative operations may require
recent authentication or additional verification.

## 14. CSRF

Because browser authentication uses cookies, appropriate
CSRF protections must be implemented for state-changing
operations.

## 15. Security Review

Authentication must be reviewed against:

- OWASP Authentication guidance
- OWASP Session Management guidance
- OWASP ASVS requirements
- Spoural security requirements

This document must be updated when the authentication
architecture changes.