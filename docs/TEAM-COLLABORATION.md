# SideQuest --- Team Collaboration & Dependency Guide

**Repository:** `https://github.com/Pavan67u/side-quest`\
**Local Windows path (current developer):** `D:\Pavan\sidequest`\
**Default branch:** `main`\
**Package manager:** npm workspaces\
**Document purpose:** Help multiple developers work in parallel while
keeping Git history, dependencies, and shared code consistent.

------------------------------------------------------------------------

## 1. Repository structure

``` text
sidequest/
├── apps/
│   ├── web/                 # Next.js + React + TypeScript
│   └── mobile/              # Expo + React Native + TypeScript
├── packages/
│   └── shared/              # Shared TypeScript types and validation
├── supabase/
│   ├── migrations/          # Database migrations
│   └── functions/           # Supabase Edge Functions
├── docs/                    # Product and technical documentation
├── .github/workflows/       # CI workflows (as added)
├── .env.example             # Names of required environment variables; no secrets
├── .gitignore
├── package.json             # npm workspace root
└── package-lock.json        # Root workspace lockfile
```

The repository uses **npm workspaces**. Keep the root
`package-lock.json` as the single lockfile for the monorepo. Do not keep
separate `apps/web/package-lock.json` or `apps/mobile/package-lock.json`
files.

## 2. One-time setup for each developer

1.  Get added as a collaborator to the GitHub repository and accept the
    invitation.

2.  Install the Node.js version agreed by the team. Use the same Node.js
    major version on every developer machine; record it in the
    repository when the team chooses one.

3.  Clone the repository:

    ``` bash
    git clone https://github.com/Pavan67u/side-quest.git
    cd side-quest
    ```

4.  Install dependencies from the root:

    ``` bash
    npm ci
    ```

    `npm ci` installs the versions recorded in the root lockfile and is
    the preferred clean setup/CI command. If the lockfile has not been
    generated or updated yet, run `npm install` once and commit the
    resulting root lockfile.

5.  Start the web app:

    ``` bash
    npm run dev:web
    ```

6.  Start the mobile app in a separate terminal:

    ``` bash
    npm run dev:mobile
    ```

7.  Copy required variable names from `.env.example` into the
    appropriate local environment file when backend configuration is
    introduced. Never commit `.env`, `.env.local`, service-role keys,
    access tokens, or other credentials.

## 3. Branch and Pull Request rules

### Golden rules

-   **Never work directly on `main`.**
-   **Never force-push to `main`.**
-   Create one branch per feature, bug fix, or focused maintenance task.
-   Keep a branch focused; avoid combining unrelated changes.
-   Pull the latest `main` before starting a new task.
-   Push your branch and open a Pull Request (PR).
-   Review and test a PR before merging it.
-   Only merge changes that pass the agreed checks.
-   Do not commit generated folders such as `node_modules`, `.next`,
    `.expo`, `dist`, or `coverage`.

### Start a new task

Run from the repository root:

``` bash
git switch main
git pull --ff-only origin main
git switch -c feature/short-feature-name
```

Use branch prefixes consistently:

-   `feature/...` --- new functionality
-   `fix/...` --- bug fixes
-   `docs/...` --- documentation-only changes
-   `chore/...` --- tooling, dependencies, maintenance
-   `test/...` --- test-only changes

Examples:

``` bash
git switch -c feature/sidequest-discovery
git switch -c feature/user-authentication
git switch -c fix/join-capacity-check
```

### Commit and push your work

Before committing:

``` bash
git status
git diff
```

Stage only files related to your task. Prefer specific paths over
`git add .` when you have unrelated changes:

``` bash
git add apps/web/src/app/page.tsx
git commit -m "feat(web): add SideQuest discovery page"
git push -u origin feature/sidequest-discovery
```

Then open a PR from your feature branch into `main`. Explain what
changed, how it was tested, and any migration or configuration impact.

Suggested commit prefixes: `feat`, `fix`, `docs`, `chore`, `test`,
`refactor`. Example: `fix(mobile): handle empty activity list`.

### Keep your branch up to date

Before starting work, or before opening a PR, fetch the latest changes:

``` bash
git fetch origin
git switch main
git pull --ff-only origin main
git switch feature/your-branch
git merge main
```

If the team prefers rebasing and the branch is only yours, use
`git rebase main` instead of `git merge main`. The team must agree on
one approach. Do not rebase a shared branch that another developer is
actively using unless everyone coordinates.

After resolving a merge conflict:

``` bash
git status
# Open each conflicted file and resolve the conflict markers.
git add <resolved-file>
git commit
```

Do not blindly choose "ours" or "theirs". Read the competing changes and
keep the correct combined implementation. If unsure, ask the author of
the other change.

## 4. How to split work to reduce conflicts

Assign ownership of files and agree on interfaces before parallel
implementation.

A reasonable initial split:

-   Developer A: web UI in `apps/web/`
-   Developer B: mobile UI in `apps/mobile/`
-   Coordinate shared types, validation, database migrations, root
    configuration, and dependency changes before editing them.

Rules for shared files:

-   `package.json` and root `package-lock.json`: coordinate changes;
    ideally one person handles dependency updates at a time.
-   `packages/shared/`: agree on exported type names and schemas before
    both apps consume them.
-   `supabase/migrations/`: use uniquely named, ordered migrations;
    never silently edit a migration that has already been applied to a
    shared environment. Add a new migration to change the schema.
-   `.env.example`: update variable names only; never place real values
    there.
-   Product decisions in `docs/`: discuss material changes and update
    the documentation deliberately.

If two people need the same shared file, have one person make the
interface change first, merge it, then let the other branch pull that
change.

## 5. Dependency versions currently recorded in the scaffold

The following versions are based on the `package.json` contents shown
during setup. Confirm them against the repository after the initial
push; dependency versions can change as the project evolves.

### Root workspace

  Setting           Value
  ----------------- ------------------------
  Package manager   npm workspaces
  Workspaces        `apps/*`, `packages/*`
  Root lockfile     `package-lock.json`

The root package currently defines scripts for `dev:web`, `dev:mobile`,
and `typecheck:shared`.

### Web app --- `apps/web/package.json`

  Dependency                   Declared version
  -------------------------- ------------------
  `next`                               `16.4.2`
  `react`                              `19.3.0`
  `react-dom`                          `19.3.0`
  `@tailwindcss/turbopack`                 `^4`
  `tailwindcss`                            `^4`
  `eslint`                                 `^9`
  `eslint-config-next`                 `16.4.2`
  `typescript`                             `^5`
  `@types/node`                           `^20`
  `@types/react`                          `^19`
  `@types/react-dom`                      `^19`

### Mobile app --- `apps/mobile/package.json`

  Dependency            Declared version
  ------------------- ------------------
  `expo`                      `~58.0.17`
  `expo-status-bar`            `~58.0.4`
  `react`                       `19.3.0`
  `react-native`                `0.88.0`
  `@types/react`               `~19.3.0`
  `typescript`                  `~6.0.3`

### Shared package --- `packages/shared/package.json`

  Setting          Value
  ---------------- ---------------------------
  Package name     `@sidequest/shared`
  Version          `0.1.0`
  Package status   Private workspace package

**Version notation:** an exact version such as `19.3.0` requests that
version; `^` permits compatible updates within the major-version range;
`~` permits patch-level updates within the specified minor version. The
root lockfile records the resolved dependency tree used by `npm ci`.

**Important:** Web and mobile use different declared TypeScript ranges
in the current scaffold. Do not independently "fix" versions or upgrade
React, React Native, Expo, or Next.js to the latest release without
checking framework compatibility first. Expo projects should use
Expo-compatible versions. Treat dependency upgrades as a dedicated PR.

## 6. Safe dependency update procedure

Only one developer should change dependencies at a time.

1.  Create a maintenance branch:

    ``` bash
    git switch main
    git pull --ff-only origin main
    git switch -c chore/update-dependencies
    ```

2.  Make the package change in the correct workspace. From the
    repository root, for example:

    ``` bash
    npm install <package-name> --workspace=apps/web
    ```

    For a development dependency:

    ``` bash
    npm install <package-name> --save-dev --workspace=apps/web
    ```

    For mobile dependencies, use the Expo-compatible install process
    when appropriate. Check compatibility before updating Expo-managed
    packages.

3.  Let npm update the root lockfile. Do not hand-edit
    `package-lock.json`.

4.  Run validation and relevant app checks:

    ``` bash
    npm ci
    npm run dev:web
    ```

    Also run the mobile app and tests affected by the change. Add
    project-wide lint, typecheck, and test scripts as they are
    established.

5.  Review exactly what changed:

    ``` bash
    git diff -- package.json apps/web/package.json apps/mobile/package.json packages/shared/package.json package-lock.json
    git status
    ```

6.  Commit the manifest and root lockfile changes together, open a PR,
    and document why the upgrade is needed.

Never delete the root lockfile just to fix an install issue. Diagnose
the error first.

## 7. Conflict-prevention checklist

Before pushing a branch:

-   [ ] `git status` shows only files for your task.
-   [ ] You are not on `main`.
-   [ ] You pulled or merged the latest `main`.
-   [ ] No `.env` file, secret, `node_modules`, or build output is
    staged.
-   [ ] The root `package-lock.json` is included if dependency manifests
    changed.
-   [ ] You did not add nested app lockfiles.
-   [ ] The app you changed starts and relevant checks pass.
-   [ ] Shared types, API contracts, and database migrations are
    coordinated.
-   [ ] The commit message explains the change.
-   [ ] A PR is opened and reviewed before merging.

## 8. If a push is rejected

Do not force-push to solve a normal non-fast-forward rejection. Run:

``` bash
git fetch origin
git status
```

Then integrate the current `main` into your feature branch using the
team's agreed merge or rebase approach, resolve conflicts, test again,
and push.

If you accidentally changed files on `main` but have not committed, stop
and ask for help before using reset or restore commands. Those commands
can discard work.

------------------------------------------------------------------------

## Team agreement

Before coding in parallel, agree on:

1.  Who owns web, mobile, shared code, and database migrations.
2.  Whether feature branches are updated by merge or rebase.
3.  Required PR checks and who approves merges.
4.  Which Node.js version the project standardizes on.
5.  Who is allowed to merge dependency upgrades and schema changes.

**Default policy:** feature branches + Pull Requests + one root
lockfile + coordinated shared-code changes + no direct pushes to `main`.
