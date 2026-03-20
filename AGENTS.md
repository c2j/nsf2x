# AGENTS.md - NSF2X Development Guidelines

## Project Overview
NSF2X is a Python/Tkinter Windows application that converts Lotus Notes NSF files to EML, MBOX, and PST formats. It uses Windows COM interfaces (Lotus Notes, Outlook) and the MAPI API.

## Build Commands

### Build Distribution
```bash
# Build main executable and installer (requires NSIS)
python create_exe.py

# Build helper utility (32-bit or 64-bit based on Python architecture)
python create_helper.py
```

### Run Application
```bash
# Run from source (requires Lotus Notes and optionally Outlook)
python nsf2x.py
```

### Testing
```bash
# Manual test script (requires editing paths in file first)
python testmapiex.py
```

**Note:** No formal test framework exists. Testing is manual and requires Windows with Lotus Notes/Outlook installed.

## Code Style Guidelines

### Python Compatibility
- **Must be Python 2.7+ and 3.4+ compatible**
- Use `try/except ImportError` for version-specific imports (see nsf2x.py lines 54-61)
- Use `list(range())` for Python 2/3 compatibility (see Format class example)

### File Header Template
All Python files must include:
```python
# -*- coding: utf-8 -*-

# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA

# Copyright (C) 2016 Free Software Foundation
# Author : David Bateman <dbateman@free.fr>
```

### Naming Conventions
- **Classes**: `CamelCase` (e.g., `EncryptionType`, `mapiobject`)
- **Functions/Methods**: `CamelCase` (e.g., `GetProperty`, `EnumerateSubFolders`)
- **Variables**: `lowercase` or `camelCase` (e.g., `notesDllPathList`, `mapi`)
- **Constants**: `UPPER_CASE` (e.g., `RT_BITMAP`, `MSGFLAG_READ`)

**Note:** This project intentionally deviates from PEP8 naming conventions (see pylint disable C0103 comments).

### Code Formatting
- **Indentation**: 4 spaces
- **Line length**: No strict limit (pylint C0301 disabled), keep under ~120 chars for readability
- **Pylint disables**: Common ones used:
  - `# pylint: disable=C0103` - Naming conventions
  - `# pylint: disable=C0301` - Line length
  - `# pylint: disable=R0903` - Too few public methods

### Imports Order
1. Standard library imports
2. Third-party imports (pywintypes, win32com, tkinter)
3. Local imports (e.g., `import mapiex`)

Example:
```python
import gettext
import os
import sys
import pywintypes
import win32com.client
try:
    import tkinter
except ImportError:
    import Tkinter as tkinter
import mapiex
```

### Class Structure
Use classes as namespaces for enums:
```python
class Format:  # pylint: disable=R0903
    """Enum for format to write to"""
    EML, MBOX, PST = list(range(3))
```

### Error Handling
- Use specific exceptions (`OSError`, `IOError`)
- Print errors to stdout for GUI display
- Use `traceback` module for debugging info

### Windows-Specific Guidelines
- COM interface calls start with uppercase (e.g., `Outlook.GetNamespace`)
- Use raw strings for Windows paths: `r'c:/program files/notes'`
- Registry access via `winreg` module
- Check `platform.architecture()` for 32/64-bit handling

## Dependencies
- Python 2.7+ or 3.4+
- pywin32 (with MAPI support)
- py2exe (for building)
- Windows with Lotus Notes and/or Outlook installed

## Translation/i18n
- Use `gettext` for translations
- Strings wrapped with `_()` function
- Translation files in `locale/` directory
- Update translations with:
  ```bash
  py pygettext.py -d nsf2x -o locale/new.pot -a nsf2x.py
  ```

## Key Files
- `nsf2x.py` - Main GUI application
- `mapiex.py` - MAPI interface wrapper
- `eml2pst.py` - EML to PST conversion utility
- `create_exe.py` - Build script for distribution
- `create_helper.py` - Build script for helper utility
- `testmapiex.py` - Manual test script
