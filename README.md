# Command-Line Text Editor

A lightweight, feature-rich command-line text editor built entirely in C++. This project is designed for efficient text manipulation directly from the terminal. It supports a range of fundamental and advanced text editing functionalities such as file operations, real-time editing, undo/redo, clipboard support, and search capabilities.

## Features

- **File Operations**
  - Open, create, save, and delete text files via terminal commands.

- **Text Manipulation**
  - Insert, delete, copy, cut, and paste text efficiently within the editor.

- **Undo/Redo**
  - Revert or reapply recent actions for better control while editing.

- **Search Functionality**
  - Locate and navigate to specific words or phrases with ease.

- **Clipboard Support**
  - Store and reuse text snippets for flexible editing workflows.

- **Real-time Keypress Detection**
  - Interactive input handling using `termios` to manage terminal behavior.

## Technologies Used

- **Programming Language:** C++
- **Libraries:**
  - Standard Template Library (STL)
  - `termios.h` for low-level terminal input/output control

## Getting Started

### Prerequisites

- A C++ compiler (e.g., `g++`)
- A UNIX-like terminal (Linux, macOS)

### Clone the Repository

```bash
git clone https://github.com/your-username/command-line-text-editor.git
cd command-line-text-editor
