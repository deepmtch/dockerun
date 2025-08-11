# dockerun

Universal Docker runner for Python and Node.js projects. Run any project in Docker without writing Dockerfiles or docker-compose configs.

## What It Does

Auto-detects Python and Node.js projects, finds entry points, detects ports, and runs them in isolated Docker containers with per-project dependency caching.

## Installation

Download and install:

```bash
# Download
wget https://github.com/deepmtch/dockerun/raw/main/dockerun

# Make executable  
chmod +x dockerun

# Move to PATH
mv dockerun ~/.local/bin/
```

Make sure `~/.local/bin` is in your PATH.

## Usage

```bash
# Auto-detect everything
dockerun

# Override options
dockerun --port 8080 --entry app.py
dockerun --dry-run  # See what it would do
dockerun --python-version 3.9
dockerun --node-version 16
```

## How It Works

1. Detects project type (Python/Node.js) by scanning for `requirements.txt`, `package.json`, etc.
2. Finds entry point (`main.py`, `app.py`, `npm run dev`, etc.)
3. Detects port from your code or uses defaults
4. Creates per-project cache directory (`.pip-cache/`, `.npm-cache/`)
5. Builds and runs Docker command with proper volume mounts

## Supported Projects

**Python:**
- Flask, Django, FastAPI applications
- Detects: `requirements.txt`, `pyproject.toml`, `setup.py`
- Entry points: `main.py`, `app.py`, `server.py`, `manage.py`

**Node.js:**
- React, Next.js, Express applications  
- Detects: `package.json`, `yarn.lock`
- Uses `npm run dev` for Next.js, `npm run start` for others

## Examples

```bash
# Flask project
cd my-flask-app
dockerun
# → python main.py --host=0.0.0.0 --port=5000

# Next.js project  
cd my-nextjs-app
dockerun
# → npm run dev (on port 3000)

# Override port
dockerun --port 8080
```

## Requirements

- Python 3.6+
- Docker installed and running
- Project files in current directory

## Cache Behavior

Creates `.pip-cache/` or `.npm-cache/` in your project directory for faster subsequent runs. These directories are automatically added to `.gitignore`.

## Contributing

Contributions welcome. The codebase uses a modular handler system - adding support for new languages requires implementing a new handler class.

## Architecture

```
dockerun → detect project → select handler → build docker command → run container
```

Each handler implements: `detect()`, `find_entry_point()`, `detect_port()`, `build_docker_cmd()`

## License

MIT
