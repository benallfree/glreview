# glreview

Review everything a branch would merge, from the command line.

`glreview` opens a git worktree in VS Code (or Cursor) and shows a
[GitLens](https://www.gitkraken.com/gitlens) **Search & Compare** of its working tree against the merge base with
`main`. You see every change the merge would bring in: committed work and uncommitted edits, in one file list, the
way a pull request would show it.

It's handy when you keep one worktree per branch (or an agent works in one) and want to look over all of it before
you merge.

## Install

```bash
brew install benallfree/tap/glreview
```

Or copy the `glreview` script anywhere on your `PATH`; it is one Bash file.

You also need:

- VS Code with its `code` shell command (or Cursor with `cursor`)
- the GitLens extension, signed in to a GitKraken account (GitLens calls its links a Preview feature)

## Use

```bash
glreview                      # the worktree you're in
glreview ../worktrees/my-fix  # another worktree
glreview -b develop           # compare against develop's merge base
glreview -e cursor            # open in Cursor
glreview -n                   # print the GitLens link, open nothing
```

| Option | What it does |
|---|---|
| `-b`, `--base <ref>` | Branch to compare against. Default `main`, else `master`. |
| `-e`, `--editor <name>` | `code`, `code-insiders` or `cursor`. Default `$GLREVIEW_EDITOR`, else `code`. |
| `-n`, `--dry-run` | Print the link instead of opening anything. |

## How it works

1. Finds the merge base: `git merge-base main HEAD`.
2. Opens the worktree folder in your editor.
3. Sends GitLens a link to compare the merge base with the working tree:

   ```text
   vscode://eamodio.gitlens/link/r/-/compare/<merge-base>...?path=<worktree>
   ```

   The empty ref after `...` means the working tree, the same as GitLens's own **Compare Working Tree with...**
   command, and `path=` makes it pick this worktree, not another checkout of the same repository.

If the editor was not running yet, the link can arrive before the window has loaded the repository. Run
`glreview` again.

## License

MIT
