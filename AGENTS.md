# AFTEREFFECT_REMOVE — classroom starter

Read `.agents/skills/aftereffect-remove/SKILL.md` for this repository's workflow
and `docs/rules.md` for the removal rules. For a computer without prerequisites,
start with `START-HERE.md` and only the current OS setup. Do not attempt Python
commands before locating Python. This repository is the source of its project
skill; do not install or change global agent rules.

- Teach one next step at a time. Inspect the student's actual footage, files and
  installed tools before choosing commands. Preserve student edits and originals.
- Mocha AE is the planar tracker; the student runs it in After Effects. OpenCV
  checks and locally refines Mocha; it does not replace it.
- Use Python and generated JSX, never an AE MCP server or a new bridge.
- The student works in their own fork: `origin` is the fork, `upstream` is
  `hyun2xyz/AFTEREFFECT_REMOVE`. Follow `docs/fork-and-update.md`. Update with
  `git fetch upstream` + `git merge upstream/main`; never reset, force-push or
  discard the student's commits. Before a commit, check `git status` and stop if
  footage, AEP, generated media, weights or `tools.local.json` would be included.
- Codex asks for approval before network commands (git, gh, pip, brew, winget)
  and writes outside this folder. Request it and explain why; never change
  sandbox or security settings to avoid it.
- Keep footage, generated images, reports and AEP files in ignored `work/` or a
  student-selected folder. Never commit private data, weights or app binaries.
- Missing frames, unmeasured frames and unknown app results are not passes.
- Summarize large arrays with Python; read short JSON reports, not long logs.
- Add a script only after it worked on a real shot. Keep shot values in
  `work/<shot>/contract.json`, not in code.
