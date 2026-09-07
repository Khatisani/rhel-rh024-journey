# 02: Introduction to the Linux Shell

## 1. What is the Shell?
The shell is a command line interpreter that sits between the user and the operating system. It accepts human readable commands, translates them into system calls, and executes them via the Linux kernel.

- **Default Shell:** **Bash** is the default interactive shell in RHEL.
- **Prompt:** Standard prompt displays user context (e.g., `$` for unprivileged users, `#` for `root`).


## 2. Break down of a Linux Command

Every standard command follows a three-part syntax structure:

```text
command   [options]   [arguments]
```

- Command: The binary executable or built-in utility being invoked (e.g., du, ls, cat).

- Options (Flags): Modify or influence the default behavior of the command.

- - Short form: single hyphen with single letters (e.g., -s, -h, or combined -sh).

- - Long form: double hyphens with descriptive words (e.g., --summarize, --human-readable).

- Arguments: The target entities the command acts upon (such as files, directories, users, or text strings).

### Example Usage:
 ```bash 
 du sh /var/log 
 ```

```text
du             -sh                   /var/log
┌────┐          ┌────┐                ┌────────┐
│Cmd │          │Opts│                │Argument│
└────┘          └────┘                └────────┘
  │               │                        │
Disk Usage      -s (summarize)         Target directory path
             -h (human-readable)
```

## 3. Terminal Productivity & Efficiency
Tab Completion (Autocomplete)

- Press Tab once to auto-complete unique file paths, directory names, or executable commands.

- Press Tab twice to display all available candidates when multiple matches exist.

- Prevents typos and verifies file paths in real time.