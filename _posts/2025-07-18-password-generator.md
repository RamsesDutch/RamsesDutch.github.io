---
title: "Building a Flexible and Secure Password Generator in Python"
date: 18-07-2025
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
python password_generator.py -n 5 -l 20 --copy
```
This command generates 5 passwords, each 20 characters long, and copies them to your clipboard (if pyperclip is installed). The passwords are also saved to a file like:

Copy
Edit
passwords_20250718_144233.txt

<sub>Screenshot: CLI output of generated passwords</sub>

Setup
You only need Python 3 to run the script. To enable clipboard functionality:

bash
Copy
Edit
pip install pyperclip

<sub>Screenshot: Installing pyperclip with pip</sub>

Behind the Script
Key components:
Uses argparse for command-line argument parsing

Character pools from string module (ascii_uppercase, digits, etc.)

Random generation using random.choice()

Input validation (length ≥ 4, at least one character type selected)

Timestamps output for file traceability

Clipboard export with optional pyperclip support

<details> <summary>📄 View Full Python Code</summary>
python
Copy
Edit
# Full password_generator.py script here
# See earlier message for complete source
</details>
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

<sub>Screenshot: Opened .txt file showing generated passwords</sub>

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

<sub>Screenshot: Project folder with script and output files</sub>
