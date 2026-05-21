# AGENTS.md

Guidance for human and AI contributors working in this repository.

## Git commit rules

- After completing a feature, small functionality, test change, or bug fix, and after the necessary validation passes, default to running `git commit` and `git push` to the current remote branch.
- Continue using the repository's Conventional Commit format for commit messages (for example `feat:`, `fix:`, `test:`, `chore:`, `pref:`).
- If there are unrelated dirty changes in the working tree, default to committing only the files changed for the current task instead of asking for confirmation. YOU MUST COMMIT AFTER YOU WORK.

## Skill layout and install docs

- Keep top-level skill folders at the repository root. Do not add new maintained skills under the repository root `.agents/skills` path.
- Every top-level skill folder or skill collection needs its own `README.md`.
- The root README should stay as a navigation page: link to each folder README and show the folder-specific install commands.
- Use `npx skills add Undertone0809/zee-agent-skills/<folder>` as the default install shape. The CLI detects skills under that folder and can let users choose one skill, several skills, or all skills.
- Do not document `npx skills add Undertone0809/zee-agent-skills` as the install command for nested collections such as `meta-skills` or `flomo-skills`; it will not inspect those collection folders correctly.
