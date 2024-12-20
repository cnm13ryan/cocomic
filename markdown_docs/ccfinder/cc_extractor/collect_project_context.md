## ClassDef ProjectModuleSet
**ProjectModuleSet**: This class extends ModuleSet and is designed to manage a set of Python modules within a project. It provides methods to retrieve both local (intra-package) and non-local imports for each module.

attributes:
· path_list: A list of file paths representing the Python modules in the project.

Code Description: The ProjectModuleSet class initializes with a list of module paths, inheriting from ModuleSet. It includes two primary methods:

1. **get_imports**: This method takes a PyModule object and an optional boolean parameter `return_fqn`. It parses the import statements within the given module to identify all imported modules. If `return_fqn` is True, it returns the fully qualified names (FQN) of these imports; otherwise, it returns their file paths. The method handles both intra-package imports (using relative import levels) and absolute imports.

2. **get_non_local_imports**: This method also takes a PyModule object as input. It identifies imports that are not part of the current project by checking if they can be resolved within the ProjectModuleSet. If an import cannot be found, it is considered non-local, and only the top-level package name is added to the set of non-local imports.

Note: The class is primarily used in conjunction with `analyze_file_dependency` function to extract local and non-local dependencies from Python files within a project directory.

Output Example: 
Assuming a project structure with modules `module_a.py`, `module_b.py`, and an external library `external_lib`, the output might look like this:

{
    "project_location": "/path/to/project/",
    "files_dependencies": {
        "module_a.py": ["module_b.py"],
        "module_b.py": []
    },
    "local_dep_stats": {
        "total_py_files": 2,
        "num_files_with_local_dep": 1,
        "mean": 0.5,
        "std": 0.7071067811865476,
        "max": 1
    },
    "non_local_imports": {
        "module_a.py": ["external_lib"],
        "module_b.py": []
    },
    "non_local_imports_set": {"external_lib"},
    "might_miss_files": False
}
### FunctionDef __init__(self, path_list)
**__init__**: Initializes a new instance of the ProjectModuleSet class.
parameters:
· path_list: A list of paths (strings) representing directories or files to be included in the project module set.

Code Description: The __init__ function is the constructor for the ProjectModuleSet class. It takes one parameter, path_list, which should be a list containing strings that represent file paths or directory paths relevant to the project modules. This method calls the superclass's initializer (super().__init__(path_list)) with the provided path_list argument, likely setting up internal state in the parent class that manages these paths.

Note: Usage points include creating an instance of ProjectModuleSet by passing a list of file or directory paths. This setup is typically used to initialize a project context for further processing, such as code analysis or extraction tasks within the cc_extractor toolset. Developers should ensure that path_list contains valid and accessible paths to avoid errors during subsequent operations on the ProjectModuleSet instance.
***
### FunctionDef get_imports(self, module, return_fqn)
**get_imports**: This function retrieves a list of imports from a specified Python module. It can return either the full qualified name (FQN) of the imported modules or their file paths, depending on the `return_fqn` parameter.

parameters:
· module: An instance representing the Python module for which to retrieve imports.
· return_fqn: A boolean flag indicating whether to return the fully qualified names of the imported modules (`True`) or their file paths (`False`). Defaults to `False`.

Code Description: The function initializes two empty data structures, a set named `imports` and a list named `imports_list`, to store unique import statements. It then calls an external function `ast_imports(module.path)` to parse the module's file for import statements, which returns a list of tuples representing each import.

For each import entry in this list, the function constructs a full import statement by joining the 'from' and 'import' parts using dots. It checks if there is an import level specified (indicating intra-package imports) and adjusts the import path accordingly. The function then attempts to retrieve the imported module using `_get_imported_module`, which presumably resolves the import based on the constructed full import statement.

If a valid imported module is found, the function determines whether to return its FQN or file path based on the `return_fqn` parameter. It adds this information to both the set and list of imports if it hasn't been added already, ensuring that each import is unique in the final result.

Note: This function is utilized within the `analyze_file_dependency` method to gather local dependencies for Python files within a specified project directory. It plays a crucial role in mapping out how different modules within a project depend on one another.

Output Example: Assuming the module has imports from 'os', 'sys', and an intra-package import from '.utils.helper', with `return_fqn=False`, the function might return:
['/path/to/project/os.py', '/path/to/project/sys.py', '/path/to/project/utils/helper.py']

If `return_fqn=True` and assuming the FQNs are 'os', 'sys', and 'project.utils.helper', it would return:
['os', 'sys', 'project.utils.helper']
***
### FunctionDef get_non_local_imports(self, module)
**get_non_local_imports**: This function identifies and returns a set of non-local modules imported by a given Python module within a project context.

parameters:
· module: Represents an instance of PyModule, which encapsulates information about a specific Python file/module being analyzed.

Code Description: The function starts by initializing an empty set named `imports` to store the names of non-local modules. It then retrieves all import statements from the specified module using the `ast_imports` function, which presumably parses the abstract syntax tree (AST) of the module's source code to extract import information.

For each import statement in `raw_imports`, the function constructs a full import path by joining the 'from' and 'import' parts of the statement. It also considers the import level specified in the import statement to determine whether the import is relative or absolute. If an import level is present, it calculates the intra-package import path based on the module's fully qualified name (fqn) and the given import level.

The function then attempts to resolve the imported module using `_get_imported_module`, a method of `ProjectModuleSet`. If the module cannot be resolved within the project context (i.e., it is non-local), the function extracts the top-level package or module name from the import statement and adds it to the `imports` set.

Note: This function is crucial for analyzing dependencies in a Python project, specifically identifying which external libraries or packages are used by each module. It helps in understanding the external dependencies of a project, which can be useful for tasks such as dependency management, refactoring, and documentation generation.

Output Example: If the analyzed module imports `numpy` and `pandas`, along with some local modules, the function might return a set like `{'numpy', 'pandas'}`. This indicates that these are non-local modules imported by the given Python file/module.
***
## FunctionDef analyze_file_dependency(project_dir)
**analyze_file_dependency**: This function analyzes file dependencies within a specified Python project directory. It identifies both local (intra-package) and non-local (external package/library) imports for each Python module in the project, providing detailed statistics about local dependencies.

parameters:
· project_dir: A string representing the path to the root directory of the Python project whose dependencies are to be analyzed.

Code Description: The function begins by logging a message indicating that file dependency extraction has started. It ensures that the provided `project_dir` ends with a slash for consistent path handling. Two dictionaries, `file_dependencies` and `non_local_imports`, along with a set `non_local_imports_set`, are initialized to store local dependencies, non-local imports per file, and unique non-local imports respectively.

The function uses `pathlib.Path(project_dir).glob('**/*.py')` to find all Python files in the project directory recursively. These paths are converted to strings and passed to an instance of `ProjectModuleSet`, which is designed to manage a set of Python modules within a project and provides methods to retrieve local and non-local imports.

For each module path, the function attempts to retrieve its local imports using `module_set.get_imports`. The retrieved import paths are adjusted relative to the project directory and stored in `file_dependencies`. If an error occurs during this process, it is logged but does not halt execution.

Similarly, for each module, non-local imports are identified using `module_set.get_non_local_imports` and added to both `non_local_imports` (per file) and `non_local_imports_set` (unique across the project).

Finally, the function compiles a dictionary containing all collected data, including local dependencies, unique non-local imports, and statistics about local dependencies such as the number of files with no dependencies.

Note: This function is essential for understanding how different modules within a Python project depend on each other and which external libraries or packages are used. It aids in tasks like dependency management, refactoring, and documentation generation.

Output Example: A possible return value from this function might look like:
{
    "file_dependencies": {
        "/path/to/project/module1.py": ["/path/to/project/utils/helper.py"],
        "/path/to/project/module2.py": ["/path/to/project/os.py", "/path/to/project/sys.py"]
    },
    "non_local_imports": {
        "/path/to/project/module1.py": {"numpy"},
        "/path/to/project/module2.py": {"pandas"}
    },
    "unique_non_local_imports": {"numpy", "pandas"},
    "local_dependency_stats": {
        "total_files": 5,
        "files_with_no_dependencies": 2
    }
}
## FunctionDef collect_file_context(file, project_prefix)
# Project Documentation

## Overview

This document provides a comprehensive guide to understanding, setting up, and using the [Project Name]. It is designed for both developers and beginners who wish to integrate this project into their applications or learn more about its functionalities.

## Table of Contents

1. **Introduction**
2. **System Requirements**
3. **Installation Guide**
4. **Configuration**
5. **Usage**
6. **API Documentation**
7. **Examples**
8. **Troubleshooting**
9. **Contributing**
10. **License**

---

### 1. Introduction

[Project Name] is a [brief description of what the project does, its purpose, and key features]. It is built using [list major technologies or frameworks used], ensuring high performance and scalability.

### 2. System Requirements

To run [Project Name], your system must meet the following requirements:

- **Operating System**: Windows 10/11, macOS Mojave (10.14) or later, Ubuntu 18.04 LTS or later.
- **Hardware**:
  - Processor: Minimum 2 GHz dual-core processor
  - Memory: Minimum 4 GB RAM
  - Storage: At least 500 MB of available disk space
- **Software**: [List any software dependencies such as specific versions of a programming language, database server, etc.]

### 3. Installation Guide

#### 3.1 Prerequisites

Ensure you have installed the following before proceeding:

- [Prerequisite Software]
- [Version Control System] (e.g., Git)

#### 3.2 Steps to Install

1. **Clone the Repository**

   Open your terminal or command prompt and run:
   
   ```bash
   git clone https://github.com/your-repo/project-name.git
   ```

2. **Navigate to Project Directory**

   Change into the project directory:
   
   ```bash
   cd project-name
   ```

3. **Install Dependencies**

   Run the following command to install all necessary dependencies:
   
   ```bash
   npm install  # or yarn install, depending on your package manager
   ```

4. **Build the Project**

   Build the project using:
   
   ```bash
   npm run build  # or yarn build
   ```

### 4. Configuration

Configuration settings are typically found in a file named `config.json` or `.env`. Here’s how to configure:

- Open the configuration file.
- Modify the necessary fields such as API keys, database connection strings, etc.

#### Example Configuration File

```json
{
  "API_KEY": "your_api_key_here",
  "DATABASE_URL": "mongodb://localhost:27017/projectdb"
}
```

### 5. Usage

Provide a brief overview of how to use the project:

- **Running the Application**

  Start the application with:
  
  ```bash
  npm start  # or yarn start
  ```

- **Accessing Features**

  [Brief description on how to access and use different features]

### 6. API Documentation

#### 6.1 Endpoints

List all available endpoints along with their descriptions, request methods, parameters, and response formats.

**Example Endpoint:**

- **Endpoint**: `/api/data`
- **Method**: `GET`
- **Description**: Fetches data from the database.
- **Parameters**:
  - `id` (optional): ID of the specific record to fetch.
- **Response**:

```json
{
  "status": "success",
  "data": [
    {
      "id": "1",
      "name": "Sample Data"
    }
  ]
}
```

### 7. Examples

Provide code snippets or examples demonstrating how to use the project.

#### Example: Fetching Data via API

```javascript
fetch('/api/data?id=1')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### 8. Troubleshooting

List common issues and their solutions.

#### Issue: Application Crashes on Startup

**Solution**: Ensure all dependencies are correctly installed by running `npm install` again. Check the configuration file for any errors.

### 9. Contributing

We welcome contributions from the community! To contribute:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/AmazingFeature`).
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4. Push to the branch (`git push origin feature/AmazingFeature`).
5. Open a pull request.

### 10. License

This project is licensed under the [License Name] License - see the `LICENSE` file for details.

---

Thank you for choosing [Project Name]. We hope this documentation helps you get started and make the most out of our project. If you have any questions or need further assistance, feel free to reach out to us via [Contact Information].

---
## FunctionDef collect_project_context(proj)
**collect_project_context**: This function collects comprehensive context information about a Python project by analyzing file dependencies and extracting detailed contexts from each Python file within the project.

parameters:
· proj: A tuple containing two elements - `input_project` (a string representing the path to the root directory of the Python project) and `output_file` (a string representing the path where the collected context information will be saved as a JSON file).

Code Description: The function begins by unpacking the input tuple into `input_project` and `output_file`. It then attempts to analyze the file dependencies within the specified project using the `analyze_file_dependency` function, which provides detailed statistics about local dependencies. A dictionary named `project_context` is initialized with an empty list under the key `"project_context"`.

The function uses `pathlib.Path(input_project).glob('**/*.py')` to recursively find all Python files in the project directory. It adjusts the file paths by removing the project prefix, ensuring that the paths are relative to the project root for consistency. For each Python file found, it calls `collect_file_context`, passing the file path and the adjusted project prefix as arguments. The returned context information for each file is appended to the `"project_context"` list in the `project_context` dictionary.

After processing all files, the function writes the complete `project_context` dictionary to a JSON file specified by `output_file`. If any exceptions occur during this process, they are caught and logged as errors using the logger.

Note: This function is essential for generating detailed context information about a Python project, including its internal dependencies and individual file contexts. It aids in tasks such as code analysis, refactoring, and documentation generation. Users should ensure that the `input_project` path points to a valid Python project directory and that they have write permissions for the specified `output_file`.
## FunctionDef main
**main**: The main function serves as the entry point for executing various tasks related to analyzing Python projects. It handles different tasks such as analyzing file dependencies, collecting context from individual files, generating project-wide context, and processing multiple projects in batch mode.

parameters:
· args.task: A string indicating the task to be performed. Valid values include "files_dependency", "file_context", "project_context", and "batch_project_context".
· args.input_project: A string representing the path to the root directory of the Python project. Required for tasks involving project-level analysis.
· args.input_file: A string representing the path to a specific Python file. Required for the "file_context" task.
· args.project_folder: A string representing the path to a folder containing multiple Python projects. Required for the "batch_project_context" task.
· args.output: A string representing the output file or directory where results will be saved.
· args.nprocs: An integer specifying the number of processes to use when processing multiple projects in batch mode.

Code Description: The main function begins by checking the value of `args.task` to determine which operation to perform. If the task is "files_dependency", it asserts that `args.input_project` is provided, then calls `analyze_file_dependency` with this project path. The resulting dependencies are written to a JSON file specified by `args.output`.

For the "file_context" task, the function ensures `args.input_file` is provided and then calls `collect_file_context` on this file. The context information for the file is saved in a JSON format to the location specified by `args.output`.

When the task is "project_context", the function asserts that `args.input_project` is provided and then calls `collect_project_context`, passing both the project path and output file path as arguments. This function analyzes the entire project, collecting detailed context information for each file and saving it to the specified output file.

In the case of "batch_project_context", the function ensures `args.project_folder` is provided. It generates a list of all Python projects within this folder and corresponding output file paths. If the output directory does not exist, it creates one. The function then uses multiprocessing with a pool size defined by `args.nprocs` to call `collect_project_context` on each project concurrently, saving their context information to individual JSON files.

If an unsupported task is specified, the function raises a `NotImplementedError`.

Note: This function is crucial for automating various analyses and context collections within Python projects. Users should ensure that all required arguments are correctly provided based on the selected task. For batch processing, having sufficient permissions and resources (such as CPU cores) to handle multiple processes simultaneously is recommended.
