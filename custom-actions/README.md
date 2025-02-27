# Types of Custom Actions

## JavaScript Actions

Use Node.js and can interact with GitHub APIs.
Run directly on the runner, without requiring Docker.

## Docker Actions

Use a Dockerfile to create an isolated environment.
Suitable for dependencies not available in the GitHub runner.

## Composite Actions

Combine multiple shell commands or other actions.
Useful for sharing scripts across workflows.
