
Paste this in the repo you want to update

```
Analyze and prepare an update plan for my project `<repo_name>` using DannFlow as the upstream template repo.

Project repo:
`<local_project_path>`

DannFlow upstream repo:
`<[dannflow_repo_url](https://github.com/Danncode10/DannFlow)>`

Important: Do not code, edit files, create branches, commit, push, or open PRs yet. This is an audit and planning phase only.

Your goals:

1. Read the local project rules first:
   - `AGENTS.md`
   - `SKILLS.md` if present
   - `dannflow.json` if present
   - `package.json`
   - relevant command folders such as `.claude/commands/`, `.codex/commands/`, and `.agents/skills/`

2. Inspect the DannFlow upstream repo:
   - verify the `upstream` remote points to `<dannflow_repo_url>`
   - fetch the latest upstream metadata
   - read these upstream files directly from DannFlow:
     - `.claude/commands/sync-upstream.md`
     - `.claude/commands/update-dannflow.md`
     - `.claude/commands/adopt-dannflow.md`
     - `.claude/commands/sync-commands.md` if present
     - `docs/dannflow_docs/` files relevant to updating old projects
   - summarize the intended DannFlow update workflow in plain English

3. Compare my project against DannFlow:
   - First check whether `dannflow.json` exists at the project root.

   If `dannflow.json` exists:
   - read `dannflow_commit`, `synced_at`, `repo`, `base_branch`, and `dev_branch`
   - verify that `dannflow_commit` exists in the fetched DannFlow history
   - use `dannflow_commit` as the project’s last synced DannFlow anchor
   - compare that commit against the latest `upstream/main`
   - list new commits from DannFlow since my project’s anchor

   If `dannflow.json` does not exist:
   - do not assume the project is already safely adopted
   - check whether an `upstream` remote exists and whether it points to `<dannflow_repo_url>`
   - inspect local files to determine whether the project appears to already contain DannFlow files, such as:
     - `.claude/commands/`
     - `.codex/commands/`
     - `.agents/`
     - `SKILLS.md`
     - `AGENTS.md`
     - `docs/dannflow_docs/`
     - `guide.sh`
   - read DannFlow’s `.claude/commands/adopt-dannflow.md` and `.claude/commands/update-dannflow.md`
   - recommend one of these approaches:
     - **Adopt DannFlow** if the project has no reliable DannFlow anchor
     - **Recreate `dannflow.json`** if the project clearly already uses DannFlow but the anchor file is missing
     - **Manual review first** if the project has partial DannFlow files but no clear sync history
   - do not create `dannflow.json` yet during the planning phase
   - propose what the initial `dannflow_commit` should be:
     - latest `upstream/main` if treating the current project as already up to date
     - a user-selected older DannFlow commit if the project appears to match an earlier version
     - unknown/manual if there is not enough evidence

   Then identify changed or missing files in safe sync areas only:
   - `.claude/commands/`
   - `.codex/commands/`
   - `.agents/`
   - `.github/`
   - `docs/dannflow_docs/`
   - `scripts/`
   - `SKILLS.md`
   - `AGENTS.md`
   - `CLAUDE.md`
   - `PROJECT_CONTEXT.md`
   - `guide.sh`
   - `src/prompts/features/`

4. Protect app-specific code:
   Do not plan automatic overwrites for:
   - `src/app/`
   - `src/components/`
   - `src/services/`
   - `src/lib/`
   - `supabase/`
   - `public/`
   - `.env*`
   - `package.json`
   - lockfiles
   - `next.config.*`
   - `tsconfig.json`

   If DannFlow changes touch these areas, mark them as “manual review only.”

5. Produce a clean branch and update strategy:
   - recommended branch name, for example `feat/sync-dannflow-<short_sha>`
   - base branch, preferably the `dev_branch` from `dannflow.json`
   - PR target branch, preferably `dev`, not `main`
   - confirm whether `dev_branch` exists before planning work
   - never merge or rebase directly from DannFlow upstream
   - never land DannFlow updates directly on `main`
   - whether this repo needs `/adopt-dannflow`, `/update-dannflow`, or `/sync-upstream`
   - which files should be copied directly from DannFlow
   - which files should be manually merged
   - which files should be skipped
   - which files require user approval before touching

6. Design a strict staged commit history:

Do not propose one large “update DannFlow” commit.

Create a commit plan where each commit represents one clear feature, command group, documentation group, CI update, or configuration update. The commit history should let me understand what DannFlow added at every stage.

Commit grouping rules:
- Group related files together by purpose, not by convenience.
- Do not mix command updates, docs updates, CI updates, agent/skill updates, and version-anchor updates in the same commit.
- Do not mix app-specific code with DannFlow template updates.
- Keep `dannflow.json` in its own final commit unless it must be committed with a sync metadata change.
- If a group is too large, split it further by feature area.
- If upstream added multiple unrelated commands, split them into separate commits by command category.
- If a file requires manual merge, put it in a separate proposed commit marked “manual review.”

For each proposed commit, include:
- commit number
- conventional commit message
- purpose of the commit
- exact files included
- upstream DannFlow commits or features represented
- risk level: low / medium / high
- whether it can be applied automatically or needs manual review
- verification to run after the commit
- DannFlow provenance trailer:
  `DannFlow-Action: sync-upstream`
  `DannFlow-Source: Danncode10/DannFlow@<upstream_sha>`

The final plan must include a “Do Not Squash” instruction:
- Do not squash these commits into one commit.
- Do not use `git add -A`.
- Stage only the exact files listed for each commit.
- Commit after each logical stage before moving to the next one.

If the proposed commit has more than 10-15 files or combines unrelated folders, split it into smaller commits.

7. Final output format:
   Give me:
   - Current repo status
   - DannFlow upstream status
   - What changed upstream
   - Safe files to sync
   - Manual-review files
   - Files to skip
   - Recommended branch and PR flow
   - Proposed staged commit history
   - Exact next prompt I should give you when I am ready to let you create the branch and apply the updates

8. Produce an execution checklist for the next prompt:
   - exact branch creation command
   - exact file groups per commit
   - exact staging list per commit
   - exact commit message per commit
   - verification command after each commit
   - files that must never be staged automatically
   - final `dannflow.json` update step
   - final status check
Again: do not modify anything yet. Only analyze and prepare the update plan.

Reliability rules for the later execution phase:

When I later approve implementation, do not claim the project is updated unless:
- every changed DannFlow file was either synced, manually reviewed, or intentionally skipped
- every skipped file has a reason
- every manual-review file has a recommended next action
- `dannflow.json` matches the actual upstream SHA applied
- no protected app-specific files were overwritten
- `git status` is clean after commits
- the branch contains multiple logical commits, not one squashed commit
- each commit contains only the files listed in the plan
- relevant checks were run and results were reported

For this current planning phase, include these reliability checks as an execution checklist, but do not perform them yet.
```
