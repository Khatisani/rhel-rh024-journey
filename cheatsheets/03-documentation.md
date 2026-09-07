# 03: Getting Help & Reading Manual Pages (man)

## 1. The `man` Command
Manual pages ("man pages") are the primary built-in reference documentation on RHEL and Linux systems. They provide full technical specifications, available options, configuration syntax, and operational examples without requiring internet access.

```bash
# General syntax
man <command_name>

# View the documentation for the man tool itself
man man
```

## 2. Navigating Within a Man Page

- Search forward (<keyword>): Type / followed by the search term and press Enter

```Bash
# Search for keyword matches across all sections
man -k <keyword>
# (e.g., man -k passwd)
```

- Next Math (n): Jump forward to the next occurrence of the search pattern

- Previous Match (N): Jump backward to the previous occurrence

- Scroll Down (Space / j / Down Arrow): Advance by full page (Space) or line-by-line (j)

- Scroll Up (b / k / Up Arrow): Move back by full page (b) or line-by-line (k)

- Top of Page (g): Jump directly to the very beginning of the document

- End of Page (G): Jump directly to the bottom of the document

- Exit (q): Quit the pager and return to the shell prompt

## 3. Understanding Man Page Sections 

Manual pages are organized into numbered sections. When searching for commands vs. configuration file formats, section numbers ensure you read the right document. 

```bash
# General syntax
man <section_number> <topic>
```

## 4. How to Find Which Numbers Are Avilable

Before opening a page, check all available sections for a term (whatis):
```bash 
# Example Usage 
whatis passwd

#Output: 
# passwd (1)       - change user password
# passwd (5)       - the password file
```
