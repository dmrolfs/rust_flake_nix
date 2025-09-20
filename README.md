# Comprehensive Rust Development Environment

A complete, modern Rust development environment using Nix flakes with containerization tools, modern CLI utilities, and advanced build systems.

## Features

- 🦀 **Complete Rust toolchain** with rust-analyzer, clippy, rustfmt
- 🐳 **Container development** with podman, podman-compose, drill
- ✨ **Modern CLI tools** replacing traditional Unix utilities
- 🏗️ **Advanced build systems** with switchable Crane integration
- 🎨 **Beautiful shell** with Starship prompt and aliases
- 🔒 **Quality gates** with pre-commit hooks and comprehensive checks
- 📦 **Multiple shells** optimized for different development scenarios

## Quick Start

```bash
# Clone and enter the development environment
nix develop

# Or use a specific shell for your workflow
nix develop .#containerdev  # Container/API development
nix develop .#ci           # Minimal CI environment
nix develop .#crane        # Advanced Nix builds
```

## Directory Structure

```
project-root/
├── flake.nix                    # Main Nix flake configuration
├── flake.lock                   # Lock file for reproducible builds
├── .envrc                       # Direnv configuration
├── .pre-commit-config.yaml      # Pre-commit hooks
├── justfile                     # Task runner configuration
├── compose.yml                  # Container orchestration
├── Cargo.toml                   # Rust project configuration
│
├── config/                      # Externalized configurations
│   ├── starship.toml           # Starship prompt configuration
│   ├── shell-aliases.nix       # Shell aliases
│   ├── env-vars.nix            # Environment variables
│   ├── shell-hook.nix          # Shell initialization
│   └── pre-commit-template.yaml # Pre-commit template
│
├── stress-tests/               # API stress test configurations
├── src/                        # Rust source code
├── data/                       # Local data (gitignored)
└── docs/                       # Documentation
```

## Development Shells

The flake provides multiple development shells optimized for different workflows:

### 🔧 Default Shell (`nix develop`)
**Best for:** General Rust development, learning, experimenting

**Includes:**
- Complete Rust toolchain with all extensions
- All testing and coverage tools (nextest, tarpaulin, mutants)
- Container tools (podman, podman-compose)
- Modern CLI replacements
- Development utilities (just, drill, pre-commit)
- Documentation tools (mdbook)
- Security tools (cargo-audit, cargo-deny)

**Use when:**
- Starting new projects
- Working on complex projects
- Learning Rust development
- Need maximum flexibility

### ⚡ CI Shell (`nix develop .#ci`)
**Best for:** Automated builds, CI/CD pipelines, lightweight containers

**Includes:**
- Core Rust toolchain (rustc, cargo, clippy, rustfmt)
- Essential build dependencies
- Testing tools (nextest, tarpaulin)

**Excludes:** Modern CLI tools, container tools, documentation tools

**Use when:**
- Setting up CI/CD environments
- Creating lightweight Docker containers
- Automated testing scenarios
- Need minimal dependencies and fast setup

### 🐳 Container Development Shell (`nix develop .#containerdev`)
**Best for:** Microservices, API development, containerized applications

**Includes:**
- Rust toolchain with essential components
- Container orchestration (podman, podman-compose)
- API development tools (curl, jq, httpie)
- Performance testing (drill)
- Modern CLI tools
- Build essentials (protobuf, just)

**Use when:**
- Building REST APIs or web services
- Working with microservices architecture
- Need container orchestration during development
- API stress testing and performance analysis

### 🏗️ Crane Shell (`nix develop .#crane`)
**Best for:** Advanced Nix builds, reproducible builds, large projects

**Includes:**
- Everything from default shell
- Integration with Crane build system
- Advanced Nix build capabilities
- Incremental build caching

**Use when:**
- Using Crane for reproducible builds
- Need advanced Nix integration
- Building complex multi-crate workspaces
- Want hermetic, cacheable builds

## Shell Selection Guide

```
Are you building containerized applications or APIs?
├── Yes → Use `nix develop .#containerdev`
└── No
    ├── Is this for CI/automated builds?
    │   ├── Yes → Use `nix develop .#ci`
    │   └── No
    │       ├── Do you need advanced Nix builds with Crane?
    │       │   ├── Yes → Use `nix develop .#crane`
    │       │   └── No → Use `nix develop` (default)
```

## Modern CLI Tools

The environment includes modern replacements for traditional Unix utilities:

| Traditional | Modern Alternative | Description |
|-------------|-------------------|-------------|
| `ls` | `eza` | Better directory listings with colors and icons |
| `cat` | `bat` | Syntax highlighting and paging |
| `grep` | `ripgrep (rg)` | Faster, smarter text search |
| `find` | `fd` | Simpler, faster file finding |
| `ps` | `procs` | Modern process viewer |
| `du` | `dust` | Intuitive disk usage visualization |
| `ping` | `gping` | Ping with real-time graphs |
| `top` | `htop` | Interactive process viewer |

### Available Aliases

**File Operations:**
- `ls` → `eza --color=always --group-directories-first`
- `ll` → `eza -la --color=always --group-directories-first`
- `cat` → `bat --paging=never`
- `tree` → `eza --tree --color=always`

**Development:**
- `cr` → `cargo run`
- `ct` → `cargo test`
- `cb` → `cargo build`
- `cc` → `cargo check`
- `cf` → `cargo fmt`

**Git:**
- `gs` → `git status`
- `ga` → `git add`
- `gc` → `git commit`
- `gp` → `git push`
- `gl` → `git log --oneline`

**Container:**
- `pc` → `podman-compose`
- `pcup` → `podman-compose up -d`
- `pcdown` → `podman-compose down`

## Build Systems

### Traditional Cargo

Default Rust build system:
```bash
cargo build    # Build project
cargo test     # Run tests
cargo run      # Run application
```

### Crane Build System

Advanced Nix-based build system with dependency caching.

#### Enabling Crane

Edit `flake.nix`:
```nix
useCrane = true; # Set to false for traditional cargo
```

#### Crane Benefits

- **Incremental builds**: Only rebuilds changed components
- **Dependency caching**: Dependencies built once, reused multiple times
- **Quality gates**: Built-in comprehensive checks
- **Reproducible**: Identical builds across environments

#### Crane Commands

```bash
# Enter crane environment
nix develop .#crane

# Build with caching
nix build

# Build documentation
nix build .#doc

# Run all quality checks
nix flake check
```

#### Available Crane Checks

- **workspace-build** - Build all targets
- **workspace-test** - Run tests with nextest
- **fmtCheck** - Code formatting
- **clippyCheck** - Linting with strict settings
- **doc-check** - Documentation with warnings as errors
- **audit-check** - Security audit
- **coverage-check** - Code coverage analysis

#### When to Use Crane

**Use Crane when:**
- Large dependency trees
- CI/CD pipelines needing build caching
- Team development with shared binary caches
- Reproducible builds required

**Use Traditional Cargo when:**
- Small projects with fast dependency builds
- Rapid prototyping
- Learning Rust basics
- Simple CI requirements

## Container Development

### Starting Services

```bash
# Start containerized services
just start
# or
podman-compose up -d

# Check service status
just status

# View logs
just logs [service-name]
```

### API Development Workflow

```bash
# Enter container development environment
nix develop .#containerdev

# Start services (database, cache, etc.)
podman-compose up -d

# Run your API
cargo run

# Stress test the API
drill --benchmark stress-tests/api.yml

# Test endpoints
http GET localhost:3000/health
curl -X POST localhost:3000/api/endpoint
```

## Quality Assurance

### Pre-commit Hooks

Set up automated quality checks:
```bash
# Create pre-commit configuration
nix run .#create-precommit-config

# Install hooks
nix run .#setup-precommit

# Hooks will run automatically on git commit
git commit -m "Your changes"
```

### Manual Quality Checks

```bash
# Code formatting
cargo fmt

# Linting
cargo clippy -- -D warnings

# Tests
cargo test
cargo nextest run  # Faster alternative

# Security audit
cargo audit

# Code coverage
cargo tarpaulin

# Performance benchmarking
hyperfine "cargo test"
```

## Environment Configuration

All configurations are externalized in the `config/` directory:

### `config/starship.toml`
Starship prompt configuration with Git integration, command duration, and directory styling.

### `config/shell-aliases.nix`
Shell aliases mapping traditional commands to modern alternatives.

### `config/env-vars.nix`
Environment variables for Rust development, container tools, and build optimization.

### `config/shell-hook.nix`
Shell initialization script with welcome message and tool setup.

### `config/pre-commit-template.yaml`
Template for pre-commit hooks including Rust-specific checks.

## Usage Examples

### New Project Setup

```bash
# Initialize project structure
just init

# Set up quality gates
nix run .#create-precommit-config
nix run .#setup-precommit

# Start development
nix develop
cargo init --name my-project
```

### API Development

```bash
# Container-focused environment
nix develop .#containerdev

# Start supporting services
podman-compose up -d

# Develop and test
cargo run &
sleep 5
http GET localhost:3000/health
drill --benchmark stress-tests/basic.yml
```

### CI/Production Builds

```bash
# Minimal build environment
nix develop .#ci

# Run CI pipeline
cargo test --all-features
cargo clippy -- -D warnings
cargo build --release
```

### Advanced Builds with Crane

```bash
# Enable Crane in flake.nix
# useCrane = true;

# Use Crane build system
nix develop .#crane
nix build              # Incremental build with caching
nix flake check        # Comprehensive quality checks
```

## Performance and Benchmarking

### Built-in Tools

```bash
# Benchmark commands
hyperfine "cargo test"
hyperfine --warmup 3 "cargo build"

# Code statistics
tokei

# Disk usage analysis
dust

# Network monitoring (requires sudo)
sudo bandwhich

# System monitoring
htop
procs
```

### API Stress Testing

```bash
# Basic stress test
drill --benchmark stress-tests/basic.yml

# Custom stress test
drill --benchmark stress-tests/heavy-load.yml --stats
```

## Troubleshooting

### Common Issues

**Crane build failures:**
```bash
nix build --rebuild
nix store gc  # Clean cache if needed
```

**Pre-commit hooks not working:**
```bash
pre-commit install --install-hooks
pre-commit run --all-files
```

**Container services not starting:**
```bash
podman-compose down
podman-compose up -d
just status
```

**Environment not loading correctly:**
```bash
nix flake lock    # Update lock file
direnv reload     # Reload direnv
nix develop       # Re-enter shell
```

### Performance Optimization

**For large projects:**
- Use Crane build system (`useCrane = true`)
- Enable binary caches in Nix configuration
- Use `cargo nextest` instead of `cargo test`

**For CI/CD:**
- Use the `ci` shell for minimal dependencies
- Cache Nix store between builds
- Use multi-stage Docker builds

## Advanced Usage

### Custom Tool Integration

Add your own tools by modifying the `devTools` list in `flake.nix`:

```nix
devTools = with pkgs; [
  # Existing tools...
  your-custom-tool
  another-development-tool
];
```

### Environment Customization

Customize environment variables in `config/env-vars.nix`:

```nix
{
  # Your custom environment variables
  MY_API_URL = "https://api.example.com";
  CUSTOM_CONFIG_PATH = "./config/custom.toml";
}
```

### Shell Hooks Customization

Modify `config/shell-hook.nix` to add custom initialization:

```bash
# Your custom shell initialization
export CUSTOM_VARIABLE="value"
echo "🎯 Custom setup complete!"
```

## Contributing

1. Fork the repository
2. Make your changes in a feature branch
3. Run quality checks: `nix flake check`
4. Submit a pull request

### Development Workflow

```bash
# Enter development environment
nix develop

# Make changes to flake.nix or config files
vim flake.nix

# Test changes
nix flake check
nix develop .#ci        # Test CI environment
nix develop .#crane     # Test Crane environment

# Update documentation
vim README.md

# Commit with quality checks
git add .
git commit -m "feat: add new development tool"
```

## License

MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

**Note:** Individual tools and dependencies included in this development environment maintain their respective licenses.

---

**Happy Rust Development!** 🦀✨

For issues or contributions, please refer to the project repository or create an issue with your specific use case and environment details.