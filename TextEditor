#include <iostream>
#include <fstream>
#include <string>
#include <stack>
#include <termios.h>
#include <unistd.h>

// Global variables
std::string text;
std::stack<std::string> undoStack;
std::stack<std::string> redoStack;
std::string clipboard;
int cursorPos = 0;

// Function to get a key press without waiting for Enter (non-blocking input)
char getKeyPress() {
    struct termios oldt, newt;
    char ch;
    tcgetattr(STDIN_FILENO, &oldt);
    newt = oldt;
    newt.c_lflag &= ~(ICANON | ECHO); // Disable canonical mode and echoing
    tcsetattr(STDIN_FILENO, TCSANOW, &newt);
    ch = getchar();
    tcsetattr(STDIN_FILENO, TCSANOW, &oldt);
    return ch;
}

// Function to open an existing file or create a new file if it doesn't exist
void openFile(const std::string& filename) {
    std::ifstream file(filename);
    if (file) {
        std::string line;
        while (std::getline(file, line)) {
            text += line + "\n";
        }
        file.close();
    } else {
        std::cout << "File not found. Creating a new file.\n";
    }
}

// Function to save the current text to a file
void saveFile(const std::string& filename) {
    std::ofstream file(filename);
    if (file) {
        file << text;
        file.close();
        std::cout << "File saved successfully.\n";
    } else {
        std::cerr << "Failed to save the file.\n";
    }
}

// Function to undo the last change
void undo() {
    if (!undoStack.empty()) {
        redoStack.push(text);  // Save current text to redo stack
        text = undoStack.top();  // Load previous version
        undoStack.pop();
    }
}

// Function to redo the last undone change
void redo() {
    if (!redoStack.empty()) {
        undoStack.push(text);  // Save current text to undo stack
        text = redoStack.top();  // Load the next version
        redoStack.pop();
    }
}

// Function to search for a term in the text
void searchText(const std::string& searchTerm) {
    size_t pos = text.find(searchTerm);
    if (pos != std::string::npos) {
        std::cout << "Found at position: " << pos << "\n";
    } else {
        std::cout << "Not found.\n";
    }
}

// Function to copy text to the clipboard
void copy() {
    clipboard = text.substr(cursorPos, 5);  // Copy 5 characters for example
    std::cout << "Copied: " << clipboard << std::endl;
}

// Function to paste text from the clipboard
void paste() {
    text.insert(cursorPos, clipboard);  // Insert clipboard content at cursor
    std::cout << "Pasted: " << clipboard << std::endl;
}

// Function to cut text and save to the clipboard
void cut() {
    clipboard = text.substr(cursorPos, 5);  // Cut 5 characters for example
    text.erase(cursorPos, 5);  // Remove the text from the editor
    std::cout << "Cut: " << clipboard << std::endl;
}

// Function to insert text at the cursor position
void insertText(const std::string& inputText) {
    undoStack.push(text);  // Save current state before change
    text.insert(cursorPos, inputText);
    cursorPos += inputText.length();  // Move cursor forward after insertion
}

// Main editor loop
void editorLoop(const std::string& filename) {
    openFile(filename);

    while (true) {
        system("clear");  // Clear the terminal screen
        std::cout << text; // Display the current content

        std::cout << "\nCursor at position: " << cursorPos << "\n";
        std::cout << "Enter command: (i) Insert, (d) Delete, (s) Save, (q) Quit, (l) Left, (r) Right, (u) Undo, (r) Redo, (c) Copy, (x) Cut, (v) Paste, (f) Search\n";

        char key = getKeyPress();

        if (key == 'q') {  // Quit the editor
            break;
        } else if (key == 's') {  // Save the file
            saveFile(filename);
        } else if (key == 'i') {  // Insert text at the cursor position
            std::cout << "Enter text to insert: ";
            std::string inputText;
            std::getline(std::cin, inputText);
            insertText(inputText);
        } else if (key == 'd') {  // Delete text before the cursor
            if (cursorPos > 0) {
                undoStack.push(text);  // Save state before deletion
                text.erase(cursorPos - 1, 1);
                cursorPos--;  // Move cursor back after deletion
            }
        } else if (key == 'l') {  // Move cursor left
            if (cursorPos > 0) cursorPos--;
        } else if (key == 'r') {  // Move cursor right
            if (cursorPos < text.length()) cursorPos++;
        } else if (key == 'u') {  // Undo action
            undo();
        } else if (key == 'r') {  // Redo action
            redo();
        } else if (key == 'c') {  // Copy text
            copy();
        } else if (key == 'x') {  // Cut text
            cut();
        } else if (key == 'v') {  // Paste text
            paste();
        } else if (key == 'f') {  // Search for text
            std::string searchTerm;
            std::cout << "Enter text to search: ";
            std::getline(std::cin, searchTerm);
            searchText(searchTerm);
        }
    }
}

int main() {
    std::cout << "Welcome to the Command-Line Text Editor!\n";
    std::string filename;
    
    std::cout << "Enter filename to open/create: ";
    std::getline(std::cin, filename);

    editorLoop(filename);
    std::cout << "Exiting the editor. Goodbye!\n";
    return 0;
}
