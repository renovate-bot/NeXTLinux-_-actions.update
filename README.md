# actions.update [![Lint status][workflow-badge]][workflow-url]

Update (i.e. commit and push) files on GitHub.

The action is perfectly suitable for updating files generated by scripts or other actions (e.g. distribution files).

## Usage

The action requires GitHub token for authentication; no username or e-mail are required.

### Basic usage

Here is an example of a workflow using `actions_update`:

```yml
name: Resources
on: repository_dispatch
jobs:
    resources:
        name: Update resources
        runs-on: ubuntu-latest
        steps:
            - uses: actions/checkout@v1

            - uses: actions/setup-node@v1

            - name: Fetch resources
              run: ./scripts/fetch-resources.sh

            - name: Update resources
              uses: nextlinux/actions.update@v1
              with:
                  file-path: path/to/file
                  commit-msg: Update resources
                  github-token: ${{ secrets.GITHUB_TOKEN }}
```

Note that this action does not change files. They should be changed with scripts and/or other actions.

### Update multiple files

You can also update multiple files:

```yml
- name: Update resources
  uses: nextlinux/actions.update@v1
  with:
      file-path: |
          path/to/file1
          path/to/file2
          path/to/file3
      commit-msg: Update resources
      github-token: ${{ secrets.GITHUB_TOKEN }}
```

### Use glob patterns

The action supports glob patterns as well:

```yml
- name: Update resources
  uses: nextlinux/actions.update@v1
  with:
      # Include all JS files from the `dist` directory
      file-path: dist/*.js
      commit-msg: Update resources
      github-token: ${{ secrets.GITHUB_TOKEN }}
```

See the `fast-glob` [documentation][glob-docs] for glob syntax.

### Inputs

#### Required inputs

-   `commit-msg`: a text used as a commit message
-   `file-path`: a path to file(s) or a glob pattern
-   `github-token`: GitHub token

#### Optional inputs

-   `branch`: branch to push changes (the default branch is used if no `branch` is specified)
-   `allow-dot`: allow glob patterns to match entries that begin with a period (`false` by default)
-   `allow-removing`: allow to remove file if local copy is missing
    (`false` by default)
-   `committer-name`: The name of the author (or committer) of the commit. (`github-actions[bot]` by default)
-   `committer-email`: The email of the author (or committer) of the commit. (`github-actions[bot]@users.noreply.github.com` by default)

Note that the action will produce an error if a local copy of a given file is missing, and the `allow-removing` flag is `false`.

### Outputs

-   `commit-sha`: the hash of the commit created by this action

## Development

```sh
# Install dependencies
> npm install

# Build the action
> npm run dist

# Lint project files
> npm run lint
```

Don't push dist files; they're updated automatically by the action itself.

## License

Licensed under the [MIT License](./LICENSE.md).
[version-url]: https://github.com/marketplace/actions/actions.update
[workflow-badge]: https://img.shields.io/github/workflow/status/nextlinux/actions.update/Lint?label=lint
[workflow-url]: https://github.com/nextlinux/actions.update/actions
