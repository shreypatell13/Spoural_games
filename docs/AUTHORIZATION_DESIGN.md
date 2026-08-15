# Spoural Games — Authorization Design

## 1. Purpose

The authorization system determines what authenticated users
are permitted to access and modify.

Authentication answers "Who is the user?"

Authorization answers "What may the user do?"

---

## 2. Authorization Model

Spoural will use:

- Role-Based Access Control (RBAC)
- Resource-level authorization
- Least privilege
- Deny-by-default behavior

---

## 3. Roles

### SUPER_ADMIN

System-wide administrative privileges.

### ADMIN

Broad tournament management privileges.

### AUCTION_MANAGER

Controls permitted auction operations.

### SCOREKEEPER

Controls permitted scoring and result-submission operations.

### TEAM_MANAGER

Manages assigned team resources and permitted auction activities.

### PLAYER

Manages permitted personal/player information.

### STUDENT

General authenticated participant/viewer.

---

## 4. Permission Categories

### Users

- users.read
- users.manage

### Games

- games.read
- games.manage

### Players

- players.read
- players.manage
- players.self.manage

### Teams

- teams.read
- teams.manage
- teams.self.manage

### Auctions

- auction.read
- auction.manage
- auction.bid
- auction.bid.manage

### Matches

- matches.read
- matches.manage
- matches.score

### Results

- results.read
- results.submit
- results.manage
- results.approve

### Announcements

- announcements.read
- announcements.manage

---

## 5. Resource Scope

Authorization may be restricted by:

- GLOBAL
- TOURNAMENT
- GAME
- TEAM
- MATCH
- SELF

Examples:

SUPER_ADMIN → GLOBAL

ADMIN → GLOBAL or assigned tournament

TEAM_MANAGER → assigned team

SCOREKEEPER → assigned match/game

PLAYER → SELF

---

## 6. Server-Side Enforcement

Authorization must always be enforced by the backend.

Frontend visibility is not considered a security control.

A user who manually sends an API request must receive
403 Forbidden when the operation is not authorized.

---

## 7. Resource Ownership

A role does not automatically grant access to every resource
of the same type.

For example, a Team Manager for Team A must not automatically
be allowed to modify Team B.

---

## 8. Deny by Default

If the system cannot establish that an operation is explicitly
allowed, the operation must be denied.

---

## 9. Authorization Flow

1. Validate the request.
2. Authenticate the user.
3. Identify the user's roles.
4. Identify the required permission.
5. Check the user's permission.
6. Check resource ownership or scope.
7. Check business rules.
8. Allow or deny the operation.
9. Record appropriate security/audit events.

---

## 10. Failure Behavior

Unauthenticated protected requests should return:

401 Unauthorized

Authenticated users without sufficient permission should return:

403 Forbidden

Resource access must not leak unnecessary private information.

---

## 11. Sensitive Operations

The following require strict authorization:

- Starting an auction
- Pausing or ending an auction
- Accepting bids
- Marking a player sold
- Changing team budgets
- Editing finalized results
- Changing user roles
- Managing administrators
- Managing tournament configuration

---

## 12. Security Principles

- Never trust the frontend.
- Never trust user-provided ownership identifiers.
- Never authorize solely from a client-provided role.
- Never use hidden UI elements as access control.
- Deny by default.
- Use least privilege.
- Verify authorization on every protected operation.

---

## 13. Review

The authorization design must be reviewed whenever a
new protected feature, role, permission, or resource type
is introduced.