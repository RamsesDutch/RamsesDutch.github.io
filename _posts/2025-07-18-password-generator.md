---
title: "Building a Flexible and Secure Password Generator in Python"
date: 2025-07-18
categories: [Projects, Python, Cybersecurity]
tags: [python, security, cli, scripting, automation, password]
excerpt: "Created a command-line password generator in Python that supports multiple options like length, character types, file saving, and clipboard export. Here's a breakdown of the code, key takeaways, and ideas for future development."
---

Creating secure passwords is a fundamental part of cybersecurity, but most online generators don’t offer much flexibility or transparency. So I decided to build my own command-line password generator using Python.

The goal was to develop something functional, customizable, and secure—a utility I could actually use in daily tasks or build upon in future projects. I also wanted to sharpen my skills in Python scripting, CLI development, and simple automation.

---

## Features

The script supports a wide range of options:

- Generate multiple passwords at once  
- Customizable length for each password  
- Include or exclude uppercase, lowercase, digits, and symbols  
- Automatically save results to a timestamped `.txt` file  
- Optional clipboard copying (requires `pyperclip`)  
- `--no-save` flag to skip writing to file  

---

## Example Usage

```bash
"path_to_python_executable" "path_to_your_python_script" [options]
```
This command generates 10 passwords, each 16 characters long by default (parameters can be changed in [options]) and copies them to your clipboard (if pyperclip is installed). The passwords are also saved to a file like:
passwords_20250718_144233.txt


![Output password generator censored](/assets/images/output-password-generator-censored.PNG)

---

Setup
You only need Python 3 to run the script. To enable clipboard functionality:

```bash
pip install pyperclip
```
If you already installed pyperclip you should see: 

![Pip install](/assets/images/pip-install-censored.PNG)

---

## Behind the Script key components:

- Uses argparse for command-line argument parsing
- Character pools from string module (ascii_uppercase, digits, etc.)
- Random generation using random.choice()
- Input validation (length ≥ 4, at least one character type selected)
- Timestamps output for file traceability
- Clipboard export with optional pyperclip support

<details>
<summary>View Full Python Code</summary>

```python
import random
import string
import argparse
from datetime import datetime
import sys
import os

# Try importing pyperclip for clipboard copy; if not available, disable that feature
try:
    import pyperclip
    CLIPBOARD_AVAILABLE = True
except ImportError:
    CLIPBOARD_AVAILABLE = False

def generate_password(length, use_upper, use_lower, use_digits, use_symbols):
    characters = ''
    if use_upper:
        characters += string.ascii_uppercase
    if use_lower:
        characters += string.ascii_lowercase
    if use_digits:
        characters += string.digits
    if use_symbols:
        characters += string.punctuation

    if not characters:
        raise ValueError("No character types selected for password generation. "
                         "Use flags to include at least one character set.")

    return ''.join(random.choice(characters) for _ in range(length))

def save_passwords_to_file(passwords):
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    # Save file in the same directory as the script
    script_dir = os.path.dirname(os.path.abspath(__file__))
    filename = os.path.join(script_dir, f"passwords_{timestamp}.txt")
    try:
        with open(filename, 'w') as f:
            for i, pwd in enumerate(passwords, 1):
                f.write(f"{i}. {pwd}\n")
    except Exception as e:
        print(f"Error saving passwords to file: {e}")
        return None
    return filename

def parse_args():
    parser = argparse.ArgumentParser(
        description="Generate secure random passwords.",
        formatter_class=argparse.ArgumentDefaultsHelpFormatter)

    parser.add_argument('-n', '--number', type=int, default=10,
                        help='Number of passwords to generate')
    parser.add_argument('-l', '--length', type=int, default=16,
                        help='Length of each password')
    parser.add_argument('--no-upper', action='store_true',
                        help='Exclude uppercase letters (A-Z)')
    parser.add_argument('--no-lower', action='store_true',
                        help='Exclude lowercase letters (a-z)')
    parser.add_argument('--no-digits', action='store_true',
                        help='Exclude digits (0-9)')
    parser.add_argument('--no-symbols', action='store_true',
                        help='Exclude symbols (e.g. !@#$%)')
    parser.add_argument('--no-save', action='store_true',
                        help="Don't save passwords to a file")
    parser.add_argument('--copy', action='store_true',
                        help='Copy generated passwords to clipboard (requires pyperclip)')

    return parser.parse_args()

def main():
    args = parse_args()

    # Validate length and number
    if args.length < 4:
        print("Error: Password length should be at least 4 for security.")
        sys.exit(1)
    if args.number < 1:
        print("Error: Number of passwords must be at least 1.")
        sys.exit(1)

    use_upper = not args.no_upper
    use_lower = not args.no_lower
    use_digits = not args.no_digits
    use_symbols = not args.no_symbols

    try:
        passwords = [generate_password(args.length, use_upper, use_lower, use_digits, use_symbols)
                     for _ in range(args.number)]
    except ValueError as e:
        print(f"Error: {e}")
        sys.exit(1)

    # Print passwords cleanly
    print("\n=== Generated Passwords ===")
    for i, pwd in enumerate(passwords, 1):
        print(f"{i:2d}. {pwd}")
    print("=" * 25)

    # Save to file unless --no-save
    if not args.no_save:
        filename = save_passwords_to_file(passwords)
        if filename:
            print(f"\nPasswords saved to file: {filename}")
        else:
            print("\nFailed to save passwords to file.")
    else:
        print("\nPassword saving skipped (--no-save used).")

    # Clipboard copy option
    if args.copy:
        if CLIPBOARD_AVAILABLE:
            joined_passwords = '\n'.join(passwords)
            pyperclip.copy(joined_passwords)
            print("\nPasswords copied to clipboard!")
        else:
            print("\nClipboard copy requested, but 'pyperclip' module not installed.")
            print("Install it with: pip install pyperclip")

    print("\nDone.")
    input("\nPress Enter to exit...")

if __name__ == "__main__":
    main()

</details>
```

Sample Output
bash
Copy
Edit
=== Generated Passwords ===
1. jFv9P#ALZb&cmk8Rt2u5
2. Y7rL*zC8uDw#VR13amQj
...

Passwords saved to file: passwords_20250718_144233.txt
Passwords copied to clipboard!

Done.


What I Learned
How to structure Python CLI tools using argparse

How to support optional third-party libraries gracefully

Timestamp-based file naming for traceability and automation

Best practices in defensive scripting and user feedback

Importance of small tools in building a security-focused workflow

Even a small, well-designed script like this can provide real-world utility and become a foundational building block in a cybersecurity toolkit.

Project Files
bash
Copy
Edit
password_generator.py
passwords_20250718_144233.txt
README.md

Future Improvements
Here are some ideas for extending the project:

Functional Features
Add password entropy scoring to inform users of strength

Support for CSV or JSON output (especially for automation use cases)

Add a regex-safe mode to avoid symbols that conflict with shell scripts

Development and Distribution
Convert into a Python package (setup.py, pip installable)

Publish on PyPI for easy installation and sharing

Build a simple GUI using Tkinter, or

Wrap into a Flask API to allow remote requests (e.g., internal use cases)

Portfolio Enhancements
Turn this into a live demo on Replit or Render

Create a short video demo or GIF for GitHub README

Add this project to your personal GitHub and tag with #cli-tool, #infosec

You can also create a README.md for GitHub based on this post, or use a starter template to speed up the process.

Final Thoughts
This project was a great opportunity to combine core cybersecurity principles with practical scripting. Developing tools like this not only streamlines secure workflows but also demonstrates technical initiative—something that’s essential in the cybersecurity field.

Thanks for reading. Feel free to fork, improve, or use it in your own projects.

You can view or clone the project here:
https://github.com/ramsesdutch/password-generator
