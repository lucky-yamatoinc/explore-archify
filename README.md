# explore-archify

Interactive [Archify](https://github.com/tt-a1i/archify) diagrams of the Hashitaro repositories.
Each diagram cites its evidence (path and line range) at one pinned commit of a repository's
`main` branch.

## Diagrams

| Diagram | Type | Evidence |
|---|---|---|
| [Hashitaro cross-repo report pipeline](diagrams/hashitaro-report-pipeline/) | architecture | `IRISA-Robot/hashitaro-docs` @ `0f0ca3a` (main) |

Each folder under `diagrams/` holds:

- `index.html`: the standalone diagram. Open it in a browser; it needs no server.
- `<type>.json`: the Archify source. Edit this file, then re-render.

## Regenerate a diagram

Requires Node ≥ 18, Chrome, and the Archify skill at `~/.claude/skills/archify`
(v3.2.0, commit `11b0d60`).

1. Make a clean clone of the evidence repository at `origin/main`. Archify checks evidence
   against committed bytes, so a feature-branch checkout with local edits will not do.
2. Update `meta.repository.revision` in the JSON source to that commit and re-check the cited lines.
3. Render from this repository's root:

   ```bash
   ARCHIFY_UPDATE_CHECK_DISABLED=1 node ~/.claude/skills/archify/bin/archify.mjs finalize \
     architecture diagrams/hashitaro-report-pipeline/architecture.json \
     diagrams/hashitaro-report-pipeline/index.html \
     --repo-root <clean-clone-of-hashitaro-docs> --quality showcase --json
   ```

`finalize` writes receipts (`*.delivery.json` and others) next to the HTML. They contain
absolute local paths and are ignored by git.

## Limits

- One diagram can pin only one repository. The cross-repo map therefore cites
  `hashitaro-docs`; per-repo diagrams cite their own code.
- A diagram is a snapshot. Regenerate it when the cited files change on `main`.
- Source links go to private repositories and open only for people with access.

## GitHub Pages

The site is static: `index.html` at the root links each diagram, and `.nojekyll` makes
Pages serve the files as they are. To publish, choose Settings → Pages → Deploy from a branch
→ `main` / `(root)`. Check your plan first: a Pages site is public unless private Pages
(GitHub Enterprise Cloud) is available.
