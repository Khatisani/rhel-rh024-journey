# 07: Editing Files with Vim

A practical reference guide for opening, navigating, editing, and saving files using **Vim** (Vi IMproved)—the ubiquitous modal text editor packaged with almost every Linux distribution.

---

## 1. Opening and Creating Files

Vim acts as both an editor and file creator. If the file exists, Vim opens it; if it does not, Vim opens a new buffer and writes the file to disk once saved.

```bash
# Open an existing file or prepare a new one
vim filename.txt

# Open a configuration file directly
vim /etc/hosts

# Open a file at a specific line number (e.g., line 45)
vim +45 /var/log/messages
```

## 2. The Modal Architecture of Vim
Unlike modeless text editors (like Nano or standard GUI editors), Vim operates in different operational modes. Keystrokes behave differently depending on the active mode.

```plaintext
                  ┌──────────────────────┐
                  │     NORMAL MODE      │◄────────────────────┐
                  │ (Navigation/Commands)│                     │
                  └──────────┬───────────┘                     │
                             │                                 │
                 Press [i]   │   Press [Esc]       Press [Esc] │
                             ▼                                 │
                  ┌──────────────────────┐                     │
                  │     INSERT MODE      │                     │
                  │   (Typing text)      │                     │
                  └──────────────────────┘                     │
                             │                                 │
                 Press [:]   │                                 │
                             ▼                                 │
                  ┌──────────────────────┐                     │
                  │     COMMAND MODE     │─────────────────────┘
                  │   (:w, :q, search)   │
                  └──────────────────────┘

```

**Normal Mode:** The default landing mode. Keystrokes are interpreted as navigation shortcuts or execution commands, not literal text insertion.

**Insert Mode:** The typing mode. Entered by pressing i. Text entered appears directly in the file buffer. Press Esc to return to Normal mode.

**Command-line Mode:** Used for file operations (saving, exiting, configuring settings). Entered by pressing : from Normal mode.

## 3. Core Normal Mode Operations 

From Normal Mode (press `Esc` anytime if unsure which mode you are in):

`yy`,Yank (Copy)
Copies the current line into the Vim register.

`p`,Paste
Pastes the copied line immediately below the current cursor position.

`P`,Paste Above
Pastes the copied line directly above the cursor line.

`10p`,Multi-Paste
Pastes the copied line 10 times in succession.

`dd`,Delete (Cut),Deletes the current line and stores it in the register (cut operation).

`5dd`,Multi-Delete,Cuts 5 lines starting from the current cursor position.

`u`,Undo
Reverts the last change. Can be pressed repeatedly to walk back history.

`Ctrl + r`,Redo
Re-applies changes that were previously undone with u.

## 4. Command-Line Mode (:) — Saving & Quitting
Press : from Normal mode to invoke bottom-line system instructions:

`:w`,Write
Saves (writes) the buffer contents to disk without exiting.

`:q`,Quit
Closes Vim. Fails if unsaved changes exist in the buffer.

`:wq`,Write & Quit
Saves current changes to disk and exits back to the terminal.

`:x`,Exit
Writes and quits only if changes were made (preserves timestamps if untouched).

`:q!`,Force Quit
Discards all unsaved edits and exits immediately.

`:w!`,Force Write
Attempts to overwrite read-only files if file permissions allow.

## 5. In-Editor Search 

From Normal mode:

- **`/<search_pattern>`:** Search forward for a specific word or string (e.g., /Listen).

- **`?<search_pattern>`:** Search backward from cursor position.

- **`n`**: Jump to the next search occurrence.

- **`N`**: Jump to the previous occurrence.

## 6. Interactive Learning: vimtutor
To build mechanical muscle memory for modal editing, Linux includes an interactive 30-minute terminal tutorial:

```bash
vimtutor
```

Running vimtutor launches an editable copy of a tutorial file where you practice navigation, substitution, deletion, and insertion without modifying system files.
