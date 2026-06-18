# File Handling in Python


File handling is an essential part of programming that allows applications to store, retrieve, and manipulate data persistently. In Python, file handling is straightforward and user-friendly due to its built-in functions and rich standard library. Whether you are working with text files, binary files, logs, or structured data, Python provides powerful tools to manage files efficiently.

This article explores file handling in Python in detail, covering file operations, modes, reading and writing techniques, working with binary files, error handling, and best practices.

## Introduction to File Handling

File handling refers to the process of interacting with files stored on a storage device such as a hard disk. It includes operations such as:

* Creating files
* Opening files
* Reading data from files
* Writing data to files
* Appending data
* Closing files

Unlike variables that store data temporarily in memory, files allow data to persist even after the program ends.

---

## Table of Contents

1. [Opening a File](#1-opening-a-file)
2. [The `with` Statement](#2-the-with-statement)
3. [Reading Files](#3-reading-files)
4. [Writing Files](#4-writing-files)
5. [Working with File Paths (pathlib)](#5-working-with-file-paths-pathlib)
6. [File Pointer & Seeking](#6-file-pointer--seeking)
7. [Binary Files](#7-binary-files)
8. [Working with CSV Files](#8-working-with-csv-files)
9. [Working with JSON Files](#9-working-with-json-files)
10. [File & Directory Operations (os module)](#10-file--directory-operations-os-module)
11. [Common Errors & How to Handle Them](#11-common-errors--how-to-handle-them)
12. [Quick Reference Summary](#quick-reference-summary)

---

## 1. Opening a File

The `open()` function is the gateway to file handling. It takes a filename and a mode, and returns a **file object** you can work with. Think of it like opening a physical folder — you need to specify which folder (filename) and what you intend to do with it (mode: read, write, etc.).

```python
f = open("filename.txt", mode)
content = f.read()
f.close()
```

You can also specify the **encoding** explicitly, which is important for files with special characters (accents, emojis, non-English text):

```python
f = open("filename.txt", "r", encoding="utf-8")
f.close()
```

> **Why encoding matters:** Without specifying `encoding="utf-8"`, Python uses the system's default encoding, which can vary between Windows, Mac, and Linux — leading to `UnicodeDecodeError` on some machines.

### File Modes

| Mode | Description |
|------|-------------|
| `'r'` | Read (default) — error if file doesn't exist |
| `'w'` | Write — creates file or overwrites existing |
| `'a'` | Append — creates file or adds to end |
| `'x'` | Create — error if file already exists |
| `'b'` | Binary mode (combine: `'rb'`, `'wb'`) |
| `'+'` | Read + Write (combine: `'r+'`, `'w+'`) |

### Choosing the Right Mode

- Use `'r'` when you only need to **read** data and don't want to risk modifying the file.
- Use `'w'` when you want to **start fresh** — it will erase the file if it already exists.
- Use `'a'` when you want to **add to** an existing file, like logging events over time.
- Use `'x'` when you want to **guarantee you're creating a new file** and not overwriting anything.
- Use `'r+'` when you need to **both read and write** without truncating the file first.

---

## 2. The `with` Statement

The `with` block (also called a **context manager**) manages the file's lifecycle automatically. Once the block ends, Python closes the file for you — even if an exception or error occurs inside the block.

```python
with open("file.txt", "r") as f:
    content = f.read()
# File is automatically closed here — no f.close() needed
```

### Why Not Just Use `open()` Without `with`?

Without `with`, you'd have to close the file manually:

```python
# ❌ Risky — if an error happens before f.close(), the file stays open
f = open("file.txt", "r")
content = f.read()
f.close()
```

Leaving files open wastes system resources, and on some systems can lock the file so other programs can't access it. The `with` statement guarantees the file is closed, making your code safer and cleaner.

### Opening Multiple Files at Once

You can open more than one file in a single `with` block:

```python
with open("input.txt", "r") as infile, open("output.txt", "w") as outfile:
    for line in infile:
        outfile.write(line.upper())
```

---

## 3. Reading Files

Python gives you several ways to read a file depending on how much control you need and how large the file is.

- `f.read()` — loads the **entire file** into one string. Simple, but uses a lot of memory for large files.
- `f.read(n)` — reads only the **next `n` characters**. Useful for chunked processing.
- `f.readline()` — reads **one line** at a time. Each call moves the internal pointer to the next line.
- `f.readlines()` — reads **all lines into a list**. Each item in the list is one line, including the `\n` at the end.
- `for line in f` — the **most memory-efficient** approach. Reads one line at a time without loading the whole file.

```python
with open("file.txt", "r") as f:
    content = f.read()        # Entire file as a single string
    line = f.readline()       # One line (moves the pointer forward)
    lines = f.readlines()     # All lines as a list ['line1\n', 'line2\n', ...]

# Best approach for large files — reads line by line without loading all at once
with open("file.txt", "r") as f:
    for line in f:
        print(line.strip())   # strip() removes the trailing newline character
```

### When to Use Each Method

| Method | Best For |
|--------|----------|
| `f.read()` | Small files where you need the whole content as one string |
| `f.read(n)` | Processing files in fixed-size chunks (e.g., streaming) |
| `f.readline()` | Reading a file header or just the first few lines |
| `f.readlines()` | When you need all lines and want to access them by index |
| `for line in f` | Large files — memory efficient, always preferred for logs or data |

### Stripping Whitespace

Lines read from a file include the newline character `\n` at the end. Use `.strip()` to remove it:

```python
line = "Hello, World!\n"
print(line.strip())   # → "Hello, World!"
```

---

## 4. Writing Files

Use `'w'` mode to create a new file or completely replace an existing one. Use `'a'` mode when you want to add content without erasing what's already there.

- `f.write(string)` — writes a single string. You must add `\n` manually if you want a new line.
- `f.writelines(list)` — writes a list of strings all at once. It does **not** add newlines between items automatically.

```python
# 'w' mode — creates the file if it doesn't exist, overwrites if it does
with open("file.txt", "w") as f:
    f.write("Hello, World!\n")                      # Writes one string with a newline
    f.writelines(["Line 1\n", "Line 2\n"])          # Writes each item in the list

# 'a' mode — keeps existing content and adds new content at the end
with open("file.txt", "a") as f:
    f.write("This line is added at the end.\n")
```

### Common Mistake: Forgetting `\n`

`f.write()` does not add a newline automatically. If you write two strings without `\n`, they'll end up on the same line:

```python
with open("file.txt", "w") as f:
    f.write("Hello")
    f.write("World")
# Result in file: HelloWorld  ← no newline between them!

# Fix:
with open("file.txt", "w") as f:
    f.write("Hello\n")
    f.write("World\n")
# Result: Hello
#         World
```

### Writing Multiple Lines Cleanly

A convenient pattern is to join a list of strings with newlines:

```python
lines = ["apple", "banana", "cherry"]
with open("fruits.txt", "w") as f:
    f.write("\n".join(lines))
```

---

## 5. Working with File Paths (`pathlib`)

`pathlib.Path` is the modern, object-oriented way to handle file paths, available since Python 3.4. It's cleaner than string-based paths and works across Windows, Mac, and Linux without worrying about `/` vs `\`.

```python
from pathlib import Path

path = Path("folder/file.txt")
```

### Inspecting Paths

```python
path.exists()        # Returns True if the file or folder exists, False otherwise
path.is_file()       # Returns True only if it's a file (not a folder)
path.is_dir()        # Returns True only if it's a directory
path.suffix          # The file extension → '.txt'
path.stem            # Filename without extension → 'file'
path.name            # Full filename with extension → 'file.txt'
path.parent          # The folder containing the file → PosixPath('folder')
path.resolve()       # The full absolute path → e.g. /home/user/folder/file.txt
```

### Reading and Writing with pathlib

```python
# Directly read or write without manually opening/closing
text = path.read_text(encoding="utf-8")          # Reads entire file as string
path.write_text("Hello!", encoding="utf-8")      # Writes string to file (overwrites)

bytes_data = path.read_bytes()                   # Read file as raw bytes
path.write_bytes(b"\x89PNG...")                  # Write raw bytes to file
```

### Building Paths Safely

Use the `/` operator to join paths — it works correctly on all operating systems:

```python
base = Path("/home/user")
full_path = base / "documents" / "report.txt"
# → /home/user/documents/report.txt
```

This is much better than string concatenation like `"/home/user" + "/" + "documents"`, which can break on Windows.

### Creating Directories

```python
# Creates the full nested folder structure; no error if it already exists
Path("new/nested/dir").mkdir(parents=True, exist_ok=True)
```

- `parents=True` — creates all intermediate directories (like `mkdir -p` in Unix).
- `exist_ok=True` — doesn't raise an error if the directory already exists.

### Listing Files in a Directory

```python
folder = Path("my_folder")

# List all files
for file in folder.iterdir():
    print(file.name)

# List only .txt files
for file in folder.glob("*.txt"):
    print(file)
```

---

## 6. File Pointer & Seeking

Every open file has an internal **pointer** (also called a cursor) that tracks your current position in the file. As you read data, the pointer moves forward automatically. You can check its position and jump to any location manually.

```python
with open("file.txt", "r") as f:
    f.read(5)       # Reads first 5 characters; pointer is now at position 5
    print(f.tell()) # → 5 (current byte position)
    f.seek(0)       # Moves pointer back to the very beginning of the file
    f.read()        # Reads the full file again from position 0
```

### `tell()` — Where Am I?

`f.tell()` returns the current byte position of the pointer. Useful when you want to save your place and return to it later.

### `seek(offset, whence)` — Jump to a Position

`f.seek()` moves the pointer to a specific byte position.

| `whence` value | Meaning |
|----------------|---------|
| `0` (default) | Offset from the **beginning** of the file |
| `1` | Offset from the **current position** |
| `2` | Offset from the **end** of the file |

```python
with open("file.txt", "r") as f:
    f.seek(0, 2)        # Jump to the end of the file
    size = f.tell()     # File size in bytes
    print(f"File is {size} bytes")

    f.seek(0)           # Go back to the beginning
```

> **Note:** In text mode (`'r'`), only `seek(0)` and positions returned by `tell()` are reliable. For arbitrary seeking, open in binary mode (`'rb'`).

---

## 7. Binary Files

Binary mode (`'b'`) is used for non-text files like images, audio, videos, PDFs, or any file where you need to work with raw bytes. In binary mode, Python reads/writes `bytes` objects instead of `str` — no encoding or decoding happens at all.

```python
# Read an image file as raw bytes
with open("photo.jpg", "rb") as f:
    data = f.read()           # 'data' is a bytes object, e.g. b'\xff\xd8\xff...'

# Write those bytes to a new file — creates an exact copy of the image
with open("copy.jpg", "wb") as f:
    f.write(data)
```

### Text Mode vs Binary Mode

| Feature | Text Mode (`'r'`) | Binary Mode (`'rb'`) |
|---------|-------------------|----------------------|
| Returns | `str` | `bytes` |
| Newlines | Translated (`\r\n` → `\n`) | Not translated |
| Encoding | Applies | Does not apply |
| Use for | `.txt`, `.csv`, `.json` | `.jpg`, `.mp3`, `.pdf`, `.exe` |

### Why Not Use Text Mode for Everything?

If you open a binary file (like a JPEG image) in text mode, Python will try to decode the bytes as text and will likely raise a `UnicodeDecodeError` — or worse, silently corrupt the data.

### Reading in Chunks

For large binary files (like videos), read in chunks instead of loading everything into memory:

```python
with open("large_video.mp4", "rb") as f:
    chunk_size = 1024 * 1024  # 1 MB at a time
    while chunk := f.read(chunk_size):
        process(chunk)   # Handle each chunk
```

---

## 8. Working with CSV Files

CSV (Comma-Separated Values) is a common format for tabular data — think spreadsheets. The `csv` module handles all the formatting rules for you: commas inside quoted fields, escaped characters, and Windows-style line endings.

- `csv.writer` — writes rows as Python lists.
- `csv.reader` — reads rows as Python lists.
- `csv.DictWriter` — writes rows from dictionaries using column names as keys.
- `csv.DictReader` — reads each row as a dictionary using the header row as keys.

```python
import csv

# Writing CSV — newline="" prevents extra blank lines on Windows
with open("data.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["Name", "Age", "City"])     # Header row
    writer.writerow(["Alice", 30, "New York"])   # Data row
    writer.writerow(["Bob", 25, "London"])

# Reading CSV with DictReader — each row becomes a dictionary
with open("data.csv", "r") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["Name"], row["Age"])   # → Alice 30, then Bob 25
```

### Why `newline=""` When Writing?

On Windows, the `csv` module handles newlines itself. If you don't pass `newline=""`, Python will also add its own newlines, resulting in a blank line between every row.

### Writing from Dictionaries with `DictWriter`

```python
with open("data.csv", "w", newline="") as f:
    fieldnames = ["Name", "Age", "City"]
    writer = csv.DictWriter(f, fieldnames=fieldnames)

    writer.writeheader()    # Writes the header row automatically
    writer.writerow({"Name": "Alice", "Age": 30, "City": "New York"})
    writer.writerow({"Name": "Bob", "Age": 25, "City": "London"})
```

### Handling Different Delimiters

Not all "CSV" files use commas. Some use tabs (`\t`) or semicolons (`;`). You can specify the delimiter:

```python
# Read a tab-separated file
with open("data.tsv", "r") as f:
    reader = csv.reader(f, delimiter="\t")
    for row in reader:
        print(row)
```

---

## 9. Working with JSON Files

JSON (JavaScript Object Notation) is the most common format for storing and transferring structured data. Python's `json` module maps JSON types directly to Python types:

| JSON | Python |
|------|--------|
| `object {}` | `dict` |
| `array []` | `list` |
| `string` | `str` |
| `number` | `int` / `float` |
| `true` / `false` | `True` / `False` |
| `null` | `None` |

```python
import json

data = {"name": "Alice", "age": 30, "hobbies": ["reading", "coding"]}

# Write Python object to a JSON file; indent=4 makes it human-readable
with open("data.json", "w") as f:
    json.dump(data, f, indent=4)

# Read JSON file back into a Python object
with open("data.json", "r") as f:
    loaded = json.load(f)
    print(loaded["name"])      # → Alice
    print(loaded["hobbies"])   # → ['reading', 'coding']
```

### `json.dump()` vs `json.dumps()`

| Function | Output |
|----------|--------|
| `json.dump(data, f)` | Writes JSON to a **file** |
| `json.dumps(data)` | Returns JSON as a **string** (useful for APIs or printing) |

```python
json_string = json.dumps(data, indent=4)
print(json_string)   # Prints the formatted JSON to the console
```

### `json.load()` vs `json.loads()`

| Function | Input |
|----------|-------|
| `json.load(f)` | Reads JSON from a **file** |
| `json.loads(string)` | Parses JSON from a **string** |

```python
json_string = '{"name": "Bob", "age": 25}'
data = json.loads(json_string)
print(data["name"])   # → Bob
```

### Pretty-Printing JSON

```python
# indent=4 makes the file human-readable
# sort_keys=True sorts keys alphabetically
with open("data.json", "w") as f:
    json.dump(data, f, indent=4, sort_keys=True)
```

---

## 10. File & Directory Operations (`os` module)

The `os` module lets you interact with the file system itself — renaming, deleting, listing, and navigating files and folders programmatically. It works on all platforms (Windows, Mac, Linux).

```python
import os

os.rename("old.txt", "new.txt")    # Renames 'old.txt' to 'new.txt'
os.remove("file.txt")              # Permanently deletes 'file.txt' (no recycle bin!)
os.mkdir("new_folder")             # Creates a single new directory
os.makedirs("a/b/c")               # Creates nested directories (like mkdir -p)
os.rmdir("empty_folder")           # Removes a directory (must be empty)
os.listdir(".")                    # Lists all files/folders in current directory
os.path.exists("file.txt")         # Returns True if path exists, False otherwise
os.path.getsize("file.txt")        # Returns file size in bytes → e.g., 1024
os.getcwd()                        # Returns the current working directory
os.chdir("/some/path")             # Changes the current working directory
```

### `os.remove()` vs `os.rmdir()`

- `os.remove("file.txt")` — deletes a **file**.
- `os.rmdir("folder")` — deletes an **empty directory**. Fails if the directory has contents.
- To delete a directory and all its contents, use `shutil.rmtree()`:

```python
import shutil
shutil.rmtree("folder_with_contents")   # Deletes folder and everything inside it
```

> ⚠️ **Warning:** `os.remove()` and `shutil.rmtree()` are permanent. There is no recycle bin or undo.

### Checking Before Acting

Always check if a file exists before trying to delete or read it, to avoid errors:

```python
if os.path.exists("file.txt"):
    os.remove("file.txt")
    print("Deleted.")
else:
    print("File not found.")
```

### Getting File Information

```python
os.path.getsize("file.txt")          # File size in bytes
os.path.isfile("file.txt")           # True if it's a file
os.path.isdir("folder")              # True if it's a directory
os.path.abspath("file.txt")          # Full absolute path
os.path.basename("/folder/file.txt") # → 'file.txt'
os.path.dirname("/folder/file.txt")  # → '/folder'
```

### Walking a Directory Tree

`os.walk()` lets you recursively visit every folder and file in a directory:

```python
for dirpath, dirnames, filenames in os.walk("my_project"):
    for filename in filenames:
        full_path = os.path.join(dirpath, filename)
        print(full_path)
```

---

## 11. Common Errors & How to Handle Them

File operations can fail for many reasons. Always handle potential errors gracefully using `try/except`.

### `FileNotFoundError`

Raised when you try to open a file that doesn't exist in `'r'` mode.

```python
try:
    with open("missing.txt", "r") as f:
        content = f.read()
except FileNotFoundError:
    print("File not found. Please check the filename.")
```

### `PermissionError`

Raised when your program doesn't have permission to read or write a file.

```python
try:
    with open("/etc/shadow", "r") as f:
        content = f.read()
except PermissionError:
    print("You don't have permission to access this file.")
```

### `FileExistsError`

Raised when using `'x'` mode and the file already exists.

```python
try:
    with open("output.txt", "x") as f:
        f.write("New content")
except FileExistsError:
    print("File already exists. Use 'w' to overwrite or 'a' to append.")
```

### `UnicodeDecodeError`

Raised when the file contains characters that can't be decoded with the given encoding.

```python
try:
    with open("file.txt", "r", encoding="utf-8") as f:
        content = f.read()
except UnicodeDecodeError:
    # Try a different encoding
    with open("file.txt", "r", encoding="latin-1") as f:
        content = f.read()
```

### Catching All File-Related Errors

```python
try:
    with open("data.txt", "r") as f:
        content = f.read()
except OSError as e:
    print(f"File error: {e}")
```

---

## Quick Reference Summary

| Task | Code |
|------|------|
| Open a file safely | `with open("f.txt", "r") as f:` |
| Read whole file | `f.read()` |
| Read line by line | `for line in f:` |
| Read all lines as list | `f.readlines()` |
| Write to file | `f.write("text\n")` |
| Write list of lines | `f.writelines(lines)` |
| Append to file | `open("f.txt", "a")` |
| Check if file exists | `Path("f").exists()` |
| Get file size | `os.path.getsize("f")` |
| Delete file | `os.remove("f")` |
| List directory | `os.listdir(".")` |
| Read JSON | `json.load(f)` |
| Write JSON | `json.dump(data, f, indent=4)` |
| Read CSV | `csv.DictReader(f)` |
| Write CSV | `csv.writer(f).writerow(row)` |
| Join paths safely | `Path("folder") / "file.txt"` |
| Current position | `f.tell()` |
| Jump to position | `f.seek(0)` |

---

> **Golden Rule:** Always use `with open(...)` so your files are properly closed after use, keeping your program safe and your data intact.
