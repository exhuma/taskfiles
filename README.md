# Taskfiles

A collection of reusable [Taskfile.dev](https://taskfile.dev/) tasks organized by category for easy inclusion in your projects.

## Purpose

This repository provides a centralized collection of common task definitions that can be shared across multiple projects. Instead of duplicating task definitions in every project, you can include these taskfiles and immediately have access to standardized commands for building, testing, linting, and more.

## Structure

The repository is organized into category-based folders, each containing a `Taskfile.yml` with related tasks:

```
taskfiles/
├── docker/         # Docker and container-related tasks
├── git/            # Git operations and utilities
├── golang/         # Go development tasks
├── nodejs/         # Node.js and npm tasks
├── python/         # Python development tasks
└── testing/        # Generic testing tasks
```

This structure allows for **selective inclusion** – you only include the taskfiles you need for your specific project.

## Usage

There are two main ways to use these taskfiles in your project:

### Method 1: Git Submodule

Add this repository as a git submodule to your project:

```bash
# Add the submodule
git submodule add https://github.com/exhuma/taskfiles.git .taskfiles

# Update submodule
git submodule update --init --recursive
```

Then include the desired taskfiles in your project's `Taskfile.yml`:

```yaml
version: '3'

includes:
  py: .taskfiles/python/Taskfile.yml
  docker: .taskfiles/docker/Taskfile.yml
  git: .taskfiles/git/Taskfile.yml

tasks:
  # Your project-specific tasks here
```

### Method 2: Remote Includes

You can directly include taskfiles from GitHub without cloning:

```yaml
version: '3'

includes:
  py:
    taskfile: https://raw.githubusercontent.com/exhuma/taskfiles/main/python/Taskfile.yml
  docker:
    taskfile: https://raw.githubusercontent.com/exhuma/taskfiles/main/docker/Taskfile.yml
  git:
    taskfile: https://raw.githubusercontent.com/exhuma/taskfiles/main/git/Taskfile.yml

tasks:
  # Your project-specific tasks here
```

**Note:** When using remote includes, tasks are prefixed with the namespace. For example, `task py:test` to run Python tests.

## Examples

### Python Project

```yaml
version: '3'

includes:
  py: .taskfiles/python/Taskfile.yml

tasks:
  start:
    desc: Start the application
    cmds:
      - python main.py
```

Now you can run:
- `task py:install` - Install dependencies
- `task py:test` - Run tests
- `task py:lint` - Run linters
- `task py:format` - Format code
- `task start` - Your custom task

### Multi-Language Project

```yaml
version: '3'

includes:
  py: .taskfiles/python/Taskfile.yml
  node: .taskfiles/nodejs/Taskfile.yml
  docker: .taskfiles/docker/Taskfile.yml

vars:
  IMAGE_NAME: myapp
  IMAGE_TAG: v1.0.0

tasks:
  setup:
    desc: Setup the entire project
    cmds:
      - task: py:install-dev
      - task: node:install
```

Run:
- `task setup` - Setup everything
- `task py:test` - Run Python tests
- `task node:build` - Build Node.js assets
- `task docker:build` - Build Docker image

### Go Project with Custom Variables

```yaml
version: '3'

includes:
  go: .taskfiles/golang/Taskfile.yml

vars:
  OUTPUT: bin/myapp

tasks:
  deploy:
    desc: Deploy the application
    deps: [go:build]
    cmds:
      - ./deploy.sh bin/myapp
```

## Available Tasks

Each category provides a set of commonly-used tasks. To see all available tasks in a category:

```bash
task --list py    # List Python tasks
task --list docker # List Docker tasks
task --list go     # List Go tasks
```

## Customization

You can override or extend tasks from included taskfiles in your project's `Taskfile.yml`:

```yaml
version: '3'

includes:
  py: .taskfiles/python/Taskfile.yml

tasks:
  py:test:
    desc: Run Python tests with custom options
    cmds:
      - pytest -v --cov=src tests/
```

## Contributing

Contributions are welcome! If you have useful task definitions that could benefit others:

1. Fork the repository
2. Create a new category folder (if needed) or add to an existing one
3. Ensure tasks are generic and reusable
4. Submit a pull request

## License

MIT License - see [LICENSE](LICENSE) file for details.
