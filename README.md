# Directory Listing App

## Introduction

The Directory Listing App is a versatile tool for managing and navigating hierarchical directory structures. It allows users to view directories and files, add new directories or files, and delete existing ones. The app features a dynamic interface where users can explore directories, view detailed information, and interact with a responsive UI.

## Features

- **Hierarchical Directory View**: Display and navigate through nested directories and files.
- **Add New Items**: Easily add new directories or files.
- **Delete Items**: Remove directories or files with proper handling of nested structures.
- **Info Tooltip**: Hover over the info icon to view detailed information about a directory.
- **Dynamic Panels**: Automatically manage and display panels based on selected directories.
- **Responsive Design**: User-friendly and accessible interface across devices.

## Installation

To set up the Directory Listing App locally, follow these steps:

1. **Clone the Repository**

    ```
    git clone https://github.com/yourusername/directory-listing-app.git
    ```

2. **Navigate to the Project Directory**

    ```
    cd directory-listing-app
    ```

3. **Install Dependencies**

    ```
    npm install
    ```

4. **Start the Development Server**

    ```
    npm run dev
    ```

    Open your browser and go to `http://localhost:3000` to view the app.

## Usage

1. **Adding Items**

    - Click the "+" button to add a new directory or file.
    - Select the type (Directory or File) and enter the name.
    - The new item will appear under the selected directory or the root.

2. **Navigating Directories**

    - Click on a directory name to view its contents.
    - The selected directory will open in a new panel.

3. **Deleting Items**

    - Click the delete button on a directory or file to remove it.
    - Deleting a directory will also remove all its nested sub-directories and files.

4. **Viewing Information**

    - Hover over the info icon (`i`) to see a tooltip with:
        - Name of the directory
        - Total number of immediate sub-directories
        - Total number of immediate files
        - Total number of all sub-directories
        - Total number of all files