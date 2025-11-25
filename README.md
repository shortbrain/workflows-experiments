# Workflows Experiments

A simple Go application with GitHub Actions workflows demonstrating CI/CD and ChatOps slash commands.

## Application

This repository contains a basic "Hello, World!" Go application.

### Running locally

```bash
go run main.go
```

### Building

```bash
go build -v ./...
```

### Testing

```bash
go test -v ./...
```

## GitHub Actions Workflows

### Build Workflow

Located at `.github/workflows/build.yaml`, this workflow runs automatically on:
- Push to `main` branch
- Pull requests to `main` branch

It performs three jobs in parallel:
1. **Build**: Compiles the Go application
2. **Test**: Runs all tests
3. **Lint**: Runs golangci-lint for code quality

### Slash Commands (ChatOps)

This repository implements slash commands for pull requests, similar to [tektoncd/pipeline](https://github.com/tektoncd/pipeline).

#### Available Commands

Comment on a pull request with these commands:

- `/retest` - Reruns failed GitHub Actions checks for the PR
- `/build` - Triggers an on-demand build and test of the PR

#### How It Works

1. **Slash Command Routing** (`.github/workflows/slash.yaml`)
   - Listens for issue comments
   - Dispatches commands to appropriate workflows
   - Requires write permission (maintainers only)

2. **Command Handlers**
   - `.github/workflows/chatops_retest.yaml` - Handles `/retest`
   - `.github/workflows/chatops_build.yaml` - Handles `/build`

3. **Permissions**
   - Only users with write access can trigger slash commands
   - Commands only work on pull requests

#### Example Usage

On a pull request, comment:

```
/retest
```

The bot will:
- Find failed workflow runs for the PR
- Rerun the failed jobs
- React with 👍 on success
- Add a comment with logs if it fails

## References

- Slash command implementation inspired by [tektoncd/pipeline](https://github.com/tektoncd/pipeline)
- Uses [peter-evans/slash-command-dispatch](https://github.com/peter-evans/slash-command-dispatch)
