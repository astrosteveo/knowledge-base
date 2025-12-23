---
title: fzf
category: Languages/Shell
tags:
  - shell
  - unix
  - cli
  - productivity
  - fuzzy-search
  - interactive
difficulty: beginner
status: evergreen
date-created: 2025-12-10
date-updated: 2025-12-10
sources:
  - https://github.com/junegunn/fzf
  - https://junegunn.github.io/fzf/
---

# fzf

> [!summary]
> fzf is a fuzzy finder—an interactive filter for any list. Pipe anything in, fuzzy search through it, get your selection out. It transforms any command that outputs a list into an interactive, searchable menu. Once you understand the pattern `something | fzf | xargs whatever`, you'll see applications everywhere.

## Theory

### The Core Concept

fzf takes lines of text as input, presents an interactive fuzzy search interface, and outputs the selected line(s). That's it.

```mermaid
flowchart LR
    A[Any Command] -->|stdout lines| B[fzf]
    B -->|Interactive Selection| C[Selected Line]
    C -->|pipe| D[Next Command]
```

```bash
# The pattern
something | fzf              # Select from list
something | fzf | xargs cmd  # Select then act
```

### Why "Fuzzy"?

You don't need exact matches. Type partial strings in any order:

- Searching for `kubectl` → type `kbc` or `ctl` or `kube`
- Searching for `deployment-nginx-prod` → type `nginx prod` or `dep prod`

fzf scores matches by how well characters align, prioritizing consecutive matches and matches at word boundaries.

## Practical Examples

### Basic Usage

```bash
# Find files in current directory
fzf

# Filter any command output
ps aux | fzf
kubectl get pods | fzf
git branch | fzf
docker ps | fzf
history | fzf
```

### Selection → Action Pattern

```bash
# Select a file and open it
vim $(fzf)

# Select a process and kill it
ps aux | fzf | awk '{print $2}' | xargs kill

# Select a branch and check it out
git branch | fzf | xargs git checkout

# Select a container and exec into it
docker ps --format '{{.Names}}' | fzf | xargs -I {} docker exec -it {} sh
```

### Useful Flags

| Flag | Purpose |
|------|---------|
| `--height=N%` | Don't take full screen |
| `--reverse` | Show list from top |
| `--preview 'cmd {}'` | Show preview of selection |
| `--multi` / `-m` | Allow multiple selections |
| `--query='text'` | Start with initial search |
| `--select-1` | Auto-select if only one match |
| `--exit-0` | Exit immediately if no matches |
| `--header='text'` | Show header line |
| `--prompt='text'` | Custom prompt |

### Preview Window

See details about the current selection in real-time:

```bash
# Preview file contents
fzf --preview 'cat {}'

# Preview with syntax highlighting (requires bat)
fzf --preview 'bat --color=always {}'

# Preview git commits
git log --oneline | fzf --preview 'git show --color=always {1}'

# Preview kubectl resources
kubectl get pods | fzf --preview 'kubectl describe pod {1}'
```

### Multiple Selection

```bash
# Select multiple items with Tab
fzf --multi

# Delete multiple selected files
ls | fzf -m | xargs rm

# Add multiple files to git
git status --short | fzf -m | awk '{print $2}' | xargs git add
```

## Shell Integration

When installed, fzf adds powerful keybindings:

| Keybinding | Action |
|------------|--------|
| `Ctrl+R` | Fuzzy search command history |
| `Ctrl+T` | Fuzzy find file, insert path |
| `Alt+C` | Fuzzy cd into directory |

These alone are worth the install.

## Keyboard Navigation

| Key | Action |
|-----|--------|
| `Ctrl+J` or `Ctrl+N` | Move down |
| `Ctrl+K` or `Ctrl+P` | Move up |
| `Enter` | Select current |
| `Tab` | Mark for multi-select |
| `Shift+Tab` | Unmark |
| `Ctrl+A` | Select all (multi mode) |
| `Esc` or `Ctrl+C` | Cancel |

## Reusable Functions

Add these to your shell rc file:

### Kubernetes

```bash
# Switch namespace
fkns() {
  local ns=$(kubectl get ns -o jsonpath='{.items[*].metadata.name}' | tr ' ' '\n' | fzf --height=40% --reverse --prompt="Namespace: ")
  [ -n "$ns" ] && kubectl config set-context --current --namespace="$ns"
}

# Switch context
fkctx() {
  local ctx=$(kubectl config get-contexts -o name | fzf --height=40% --reverse --prompt="Context: ")
  [ -n "$ctx" ] && kubectl config use-context "$ctx"
}

# Pod logs
fklogs() {
  local pod=$(kubectl get pods --no-headers -o custom-columns=":metadata.name" | fzf --height=40% --reverse --prompt="Pod: ")
  [ -n "$pod" ] && kubectl logs -f "$pod"
}

# Exec into pod
fkexec() {
  local pod=$(kubectl get pods --no-headers -o custom-columns=":metadata.name" | fzf --height=40% --reverse --prompt="Pod: ")
  [ -n "$pod" ] && kubectl exec -it "$pod" -- sh
}
```

### Docker

```bash
# Exec into container
fdock() {
  local c=$(docker ps --format '{{.Names}}' | fzf --height=40% --reverse)
  [ -n "$c" ] && docker exec -it "$c" sh
}

# Container logs
fdlogs() {
  local c=$(docker ps -a --format '{{.Names}}' | fzf --height=40% --reverse)
  [ -n "$c" ] && docker logs -f "$c"
}
```

### Git

```bash
# Checkout branch
fgit() {
  local branch=$(git branch -a | sed 's/^[* ]*//' | fzf --height=40% --reverse)
  [ -n "$branch" ] && git checkout "$branch"
}

# Browse git log
fglog() {
  git log --oneline --color=always | fzf --ansi --preview 'git show --color=always {1}'
}
```

### General

```bash
# Kill process
fkill() {
  local pid=$(ps aux | fzf --height=40% --reverse | awk '{print $2}')
  [ -n "$pid" ] && kill -9 "$pid"
}

# SSH to host
fssh() {
  local host=$(grep "^Host " ~/.ssh/config | awk '{print $2}' | grep -v '\*' | fzf)
  [ -n "$host" ] && ssh "$host"
}

# Find and edit file
fe() {
  local file=$(fzf --preview 'head -100 {}')
  [ -n "$file" ] && ${EDITOR:-vim} "$file"
}

# Find and cd
fcd() {
  local dir=$(find . -type d 2>/dev/null | fzf)
  [ -n "$dir" ] && cd "$dir"
}
```

## Configuration

Set defaults in your shell rc:

```bash
export FZF_DEFAULT_OPTS='--height=40% --layout=reverse --border --info=inline'

# Use fd instead of find (faster, respects .gitignore)
export FZF_DEFAULT_COMMAND='fd --type f'

# Ctrl+T preview
export FZF_CTRL_T_OPTS="--preview 'head -100 {}'"
```

## The Universal Pattern

Any time you:
1. Run a command that outputs a list
2. Manually scan/copy something from that list
3. Use it in another command

**Replace with:** `cmd1 | fzf | xargs cmd2`

## "Pipe Anything" Examples

```bash
# Pick a host from /etc/hosts
cat /etc/hosts | fzf

# Pick an environment variable
env | fzf

# Pick an alias
alias | fzf

# Pick a disk
lsblk | fzf

# Pick a systemd service
systemctl list-units | fzf

# Pick an installed package
rpm -qa | fzf      # RHEL/Rocky
dpkg -l | fzf      # Debian/Ubuntu

# Pick any kubernetes resource
kubectl get all -A | fzf
```

> [!tip]
> fzf doesn't care what the input is. If it's lines of text, fzf can filter it. The power comes from combining it with other commands via pipes and xargs.

## Quick Reference

```bash
# Basic filter
cmd | fzf

# With action
cmd | fzf | xargs action

# With preview
cmd | fzf --preview 'preview_cmd {}'

# Multi-select
cmd | fzf -m | xargs action

# Height and layout
cmd | fzf --height=40% --reverse

# Shell keybindings
Ctrl+R  # History search
Ctrl+T  # File finder
Alt+C   # Directory changer
```
