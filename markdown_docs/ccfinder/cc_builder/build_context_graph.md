## FunctionDef construct_adj_matrix(input_graph_path, input_nodes_path, output_adj_path, encode, node_emb_lm, batch_size, max_seq_length)
**construct_adj_matrix**: This function constructs an adjacency matrix from a project graph and node information, optionally encoding nodes using a language model, and saves the resulting matrix to a file.

parameters:
· input_graph_path: A string representing the file path to the serialized graph object (in gpickle format) that contains the project's structural data.
· input_nodes_path: A string representing the file path to the JSON file containing node information for the project.
· output_adj_path: A string representing the file path where the constructed adjacency matrix will be saved in pickle format.
· encode: A boolean flag indicating whether nodes should be encoded using a language model. This parameter is currently not utilized within the function and defaults to False.
· node_emb_lm: An optional parameter that would represent a language model used for encoding nodes if 'encode' were True. It is not used in the current implementation.
· batch_size: An optional integer specifying the batch size for processing nodes, which is also not utilized in the current implementation.
· max_seq_length: An optional integer indicating the maximum sequence length for node encodings, similarly unused in the current function.

Code Description: The function begins by logging a message indicating that it is creating an adjacency matrix for the project specified by the input_nodes_path. It then loads the graph from the file located at input_graph_path using NetworkX's read_gpickle method and reads the nodes from the JSON file specified by input_nodes_path. Each line in the node file is expected to be a valid JSON object representing a node.

The function initializes an adjacency matrix 'adj' with dimensions (n_rel, n_node, n_node), where n_rel is the number of edge types defined in the EDGE_TYPE constant and n_node is the total number of nodes in the project. The matrix is initialized with zeros and has a data type of np.uint8.

The function then iterates over all pairs of nodes (s, t) to check if there is an edge between them in the graph. If an edge exists, it examines the attributes of that edge for each relationship type ('rel'). If the relationship type index is within the valid range [0, n_rel), it sets the corresponding entry in the adjacency matrix to 1.

An assertion checks that the number of edges in the project graph matches the count of non-zero entries in the adjacency matrix, ensuring consistency between the two data structures. The function then creates a dictionary 'adj_mtx' containing both the adjacency matrix and the list of nodes, logs a message indicating where the adjacency matrix will be saved, and writes this dictionary to the file specified by output_adj_path using Python's pickle module.

Note: Usage points include calling this function with appropriate paths for graph data, node information, and desired output location. The 'encode', 'node_emb_lm', 'batch_size', and 'max_seq_length' parameters are currently not used in the function implementation provided. This function is typically invoked after constructing a project's context graph using another function like create_cxt_graph, which prepares necessary files such as edge lists and node information before passing them to construct_adj_matrix for adjacency matrix construction.
## FunctionDef construct_graph(edge_jsonl_path, node_path, output_graph_path)
**construct_graph**: This function constructs a project context graph from node and edge data, then saves it into a pickle file using NetworkX's MultiDiGraph structure.

parameters:
· edge_jsonl_path: A string representing the path to the JSON Lines (JSONL) file containing edge information.
· node_path: A string representing the path to the file containing node information.
· output_graph_path: A string representing the path where the constructed graph will be saved as a pickle file.

Code Description: The function starts by logging an informational message indicating that it is constructing a project context graph for a specific project, derived from the basename of the node_path. It initializes two dictionaries, `node2id` and `id2node`, to map nodes to unique integer identifiers and vice versa. Nodes are read from the file specified by `node_path` using ujson for efficient parsing.

Next, it creates a mapping (`edge2id`) from edge types defined in the global variable `EDGE_TYPE` to unique integer identifiers. A NetworkX MultiDiGraph object is instantiated to hold the graph data.

Edges are then read from the JSONL file specified by `edge_jsonl_path`. For each line (representing an edge), the function parses it, maps the relationship type and nodes to their respective IDs using the previously created mappings (`edge2id` and `node2id`), and adds this edge to the graph if it does not already exist. If the relationship type is reversible (as defined in the global variable `REVERSE_EDGE_TYPE`), a reverse edge is also added with an adjusted relation ID.

After all edges are processed, the function asserts that the number of unique attributes (edges) matches the number of edges in the graph to ensure no duplicates were inadvertently added. Finally, it saves the constructed graph to the file specified by `output_graph_path` using NetworkX's gpickle format and logs a confirmation message indicating where the graph file was saved.

Note: This function is typically called within another function, such as `create_cxt_graph`, which handles the preparation of node and edge files before constructing the graph. It is essential to ensure that the input files (`edge_jsonl_path` and `node_path`) are correctly formatted and contain valid data for the function to operate successfully.
## FunctionDef extract_node_edge_coarse_grained(proj_cxt, output_edge_path, output_nodes_path)
# Project Documentation

## Overview

This document provides a comprehensive guide to understanding, setting up, and using the [Project Name] software. It is designed for both developers who wish to integrate this project into their applications and beginners looking to explore its capabilities.

## Table of Contents

1. **Introduction**
2. **System Requirements**
3. **Installation Guide**
4. **Configuration**
5. **Usage**
6. **API Documentation**
7. **Troubleshooting**
8. **Contributing**
9. **License**

---

### 1. Introduction

[Project Name] is a [brief description of what the project does, its purpose, and key features]. It is built using [mention technologies or programming languages used], ensuring high performance and reliability.

### 2. System Requirements

To run [Project Name], your system must meet the following requirements:

- **Operating Systems**: Windows 10+, macOS Mojave+, Linux (Ubuntu 18.04+)
- **Hardware**:
  - Minimum: 2GB RAM, 50MB disk space
  - Recommended: 4GB RAM, 100MB disk space
- **Software Dependencies**:
  - [List any software dependencies required]

### 3. Installation Guide

#### Prerequisites

Ensure that you have installed the following prerequisites before proceeding with the installation:

- [Prerequisite 1]
- [Prerequisite 2]

#### Steps to Install

1. **Download the Package**: Visit the official website or repository and download the latest version of [Project Name].
2. **Extract Files**: Extract the downloaded files into a directory of your choice.
3. **Run Installer**:
   - On Windows: Double-click `setup.exe` and follow the on-screen instructions.
   - On macOS: Open the `.dmg` file, drag the application to your Applications folder.
   - On Linux: Use the terminal command `sudo dpkg -i [filename].deb` for Debian-based systems or `sudo rpm -i [filename].rpm` for Red Hat-based systems.

### 4. Configuration

After installation, you may need to configure certain settings to suit your needs:

- **Configuration File**: Locate and edit the configuration file (`config.json`) in the installation directory.
- **Environment Variables**: Set any required environment variables as specified in the configuration file.

### 5. Usage

#### Basic Operations

To start using [Project Name], follow these basic steps:

1. **Launch Application**:
   - On Windows: Open from Start Menu.
   - On macOS: Open from Applications folder.
   - On Linux: Launch from terminal or applications menu.
2. **Explore Features**: Navigate through the application to explore its features and functionalities.

#### Advanced Usage

For advanced usage, refer to the API documentation for detailed instructions on how to interact with [Project Name] programmatically.

### 6. API Documentation

The API provides a set of endpoints that allow developers to integrate [Project Name] into their applications. Detailed information about each endpoint is provided below:

- **Endpoint 1**: Description, parameters, and example usage.
- **Endpoint 2**: Description, parameters, and example usage.

For more detailed documentation, visit the official API reference page on our website.

### 7. Troubleshooting

If you encounter any issues while using [Project Name], refer to the following troubleshooting tips:

- **Common Issues**:
  - Issue 1: Solution
  - Issue 2: Solution
- **Contact Support**: If you cannot resolve your issue, contact support at [support email or phone number].

### 8. Contributing

We welcome contributions from developers of all skill levels! To contribute to [Project Name], follow these steps:

1. **Fork the Repository**: Create a fork of the repository on GitHub.
2. **Create a Branch**: Checkout a new branch for your feature or bug fix.
3. **Make Changes**: Implement your changes and commit them with clear messages.
4. **Push Changes**: Push your changes to your forked repository.
5. **Submit a Pull Request**: Open a pull request against the main repository.

### 9. License

[Project Name] is released under the [License Type]. For more information, see the `LICENSE` file in the project directory.

---

This documentation aims to provide all necessary information for users and developers to effectively use and contribute to [Project Name]. If you have any questions or need further assistance, please do not hesitate to contact us.
### FunctionDef add_node_edge(check_node, node_type, head, rel, tail)
**add_node_edge**: This function is designed to add a node and an edge to a graph structure based on the given parameters. It checks if a node should be added to a list of seen nodes, increments the count for that type of node, and writes an edge definition to a file if both head and tail nodes are valid.

**parameters**:
· check_node: The node to be considered for addition to the graph. This node must either be the head or the tail of the edge being added.
· node_type: A string representing the type of the node, used to categorize and count nodes in a dictionary.
· head: The starting point of the edge in the graph.
· rel: The relationship or label of the edge connecting the head and tail nodes. This must be one of the predefined edge types stored in EDGE_TYPE.
· tail: The ending point of the edge in the graph.

**Code Description**: The function begins by asserting that check_node is either the head or the tail of the edge, ensuring consistency with the intended use. It also verifies that rel is a valid edge type. If check_node has not been seen before and is not an empty string, it adds the node to the nodes list, marks it as seen in the node_seen set, increments the count for its type in node_cnt, and sets added_node to True. The function then checks if both head and tail are non-empty strings. If so, it asserts that both nodes have already been seen (ensuring they exist in the graph), writes a JSON representation of the edge to an output file using ujson.dumps with ensure_ascii set to False for proper handling of non-ASCII characters, and sets added_edge to True. Finally, the function returns a tuple containing the boolean values of added_node and added_edge, indicating whether a node was added and whether an edge was written to the file.

**Note**: This function is typically used within graph-building processes where nodes represent entities (like functions or variables) and edges represent relationships between them. It ensures that each node is only added once and that all edges connect existing nodes, maintaining the integrity of the graph structure.

**Output Example**: If add_node_edge("func1", "function", "file1", "contains", "func1") is called and both "file1" and "func1" are already in node_seen, the function will write '{"head": "file1", "rel": "contains", "tail": "func1"}' to the output file and return (False, True), indicating no new node was added but an edge was written.
***
### FunctionDef build_function(func_list, func_parent, rel)
**build_function**: This function extracts nodes and edges for a list of functions within a graph structure. It processes each function's details, creating a node for the function itself and another node for its signature, then establishes an edge connecting the parent node to the function node.

**parameters**:
· func_list: A list containing dictionaries with detailed information about each function.
· func_parent: The identifier of the parent node to which all function nodes will be connected.
· rel: A string representing the relationship type between the parent node and the function nodes. By default, this is set to "function".

**Code Description**: The function iterates over each dictionary in `func_list`, where each dictionary represents a function with at least two keys: "func_name" for the function's name and "func_orig_str" for its original string representation or signature. For each function, it constructs a unique node identifier by concatenating `func_parent` with the function's name using a dot as a separator.

The function then calls `add_node_edge` to add two elements:
1. An edge from `func_parent` (the head) to `func_node` (the tail), labeled with the relationship type specified in `rel`. This edge signifies that the parent node contains or is related to the function node.
2. A node representing the function's signature (`func_details`). This node is connected to the function node itself, with an edge labeled "func_signature". The purpose of this node and edge is to associate the function's detailed signature with its corresponding function node in the graph.

**Note**: This function is typically used during the construction of a context graph where functions are represented as nodes, and their relationships (such as containment within files or modules) are represented as edges. By processing each function in `func_list` and establishing these connections, `build_function` contributes to building a comprehensive and detailed graph structure that can be used for various analyses, such as code comprehension, refactoring suggestions, or dependency analysis. It ensures that each function is uniquely identified within the context of its parent node and that its signature is properly associated with it in the graph.
***
## FunctionDef create_cxt_graph(proj)
# Project Documentation

## Overview

This document provides a comprehensive guide to understanding and utilizing the [Project Name] software application. It is designed for both developers who wish to integrate this project into their applications or modify its functionality, as well as beginners looking to understand how to use it effectively.

## Table of Contents

1. **Installation**
2. **Configuration**
3. **Usage**
4. **API Documentation**
5. **Examples**
6. **Troubleshooting**
7. **Contributing**
8. **License**

---

### 1. Installation

#### Prerequisites
- Ensure you have [Prerequisite Software] installed on your system.
- Verify that your operating system meets the minimum requirements.

#### Steps to Install

**For Developers:**
1. Clone the repository from GitHub:
   ```
   git clone https://github.com/yourusername/projectname.git
   ```
2. Navigate into the project directory:
   ```
   cd projectname
   ```
3. Install dependencies using [Dependency Manager]:
   ```
   [Dependency Manager] install
   ```

**For End Users:**
1. Download the latest release from the [Releases Page](https://github.com/yourusername/projectname/releases).
2. Follow the installation instructions provided in the downloaded package.

---

### 2. Configuration

Configuration settings are managed via a configuration file typically named `config.json`. Below is an example of what this file might look like:

```json
{
    "api_key": "YOUR_API_KEY",
    "database_url": "http://localhost:5432/dbname",
    "logging_level": "INFO"
}
```

**Configuration Options:**
- **api_key**: Your unique API key for accessing external services.
- **database_url**: URL pointing to your database instance.
- **logging_level**: Controls the verbosity of logs. Options include DEBUG, INFO, WARNING, ERROR.

---

### 3. Usage

#### Basic Workflow
1. Initialize the application by running:
   ```
   ./projectname init
   ```
2. Start the application server:
   ```
   ./projectname start
   ```

#### Command Line Interface (CLI)
The CLI provides various commands to interact with the application:

- **init**: Initializes the project setup.
- **start**: Starts the application server.
- **stop**: Stops the running server.
- **status**: Checks the current status of the application.

---

### 4. API Documentation

#### Endpoints
- **GET /api/data**
  - Description: Retrieves data from the database.
  - Parameters:
    - `id`: Unique identifier for the data entry (optional).
  - Response: JSON object containing requested data.

- **POST /api/data**
  - Description: Adds new data to the database.
  - Body: JSON object with data fields.
  - Response: Confirmation message and ID of created entry.

#### Authentication
API requests require an API key included in the request headers:
```
Authorization: Bearer YOUR_API_KEY
```

---

### 5. Examples

**Example Request:**
```bash
curl -X GET "http://localhost:3000/api/data?id=123" \
-H "Authorization: Bearer YOUR_API_KEY"
```

**Expected Response:**
```json
{
    "id": 123,
    "name": "Sample Data",
    "value": 456
}
```

---

### 6. Troubleshooting

#### Common Issues
- **Server not starting**: Ensure all dependencies are installed and the configuration file is correctly set.
- **API requests failing**: Verify that your API key is correct and included in the request headers.

#### Logs
Check the application logs for detailed error messages:
```
tail -f /var/log/projectname.log
```

---

### 7. Contributing

We welcome contributions from the community! To contribute:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Make your changes and commit them with clear, descriptive messages.
4. Push to your fork and submit a pull request.

#### Code Style
- Follow the [Code Style Guide](https://github.com/yourusername/projectname/wiki/Code-Style-Guide).
- Ensure all code is well-documented and includes unit tests where applicable.

---

### 8. License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/yourusername/projectname/blob/master/LICENSE) file for details.

---

For further assistance, please refer to our [FAQ](https://github.com/yourusername/projectname/wiki/FAQ) or contact us via [Support Email].

Thank you for choosing [Project Name]! We hope you find it useful and easy to integrate into your projects.
## FunctionDef main
**main**: The main function serves as the entry point for building context graphs from project context JSON files. It handles two primary tasks based on the specified task type: processing a single project or batch processing multiple projects.

parameters:
· args.task: Specifies the task to perform, either "single_proj" for processing a single project or "batch_proj" for processing multiple projects.
· args.proj_cxt_json: Path to the JSON file containing context information for a single project. Required if `args.task` is "single_proj".
· args.proj_cxt_folder: Directory containing multiple JSON files with context information for different projects. Required if `args.task` is "batch_proj".
· args.output_dir: Directory where the output files (context graphs) will be saved.
· args.nprocs: Number of processes to use for parallel processing when handling multiple projects.

Code Description: The function begins by checking the value of `args.task`. If it is set to "single_proj", it asserts that `args.proj_cxt_json` is not None, indicating a valid JSON file path must be provided. It then calls the `create_cxt_graph` function with a tuple containing the project context JSON file and the output directory.

For the "batch_proj" task, the function first checks if `args.proj_cxt_folder` is not None, ensuring a valid directory path is provided. It initializes two lists: `proj_js_list` to store paths to each project's context JSON files and `output_dir_list` for corresponding output directories. If the specified `args.output_dir` does not exist, it creates this directory.

The function then iterates over all files in the `args.proj_cxt_folder`. For each file, it constructs the full path (`proj_js`) and appends it to `proj_js_list`. It also determines an appropriate output directory for each project by replacing "_project_context.json" with an empty string in the filename and appending this to `output_dir_list`.

After collecting all necessary paths, the function asserts that both lists have the same length. This ensures a one-to-one correspondence between input JSON files and output directories.

The function uses Python's multiprocessing Pool to parallelize the processing of multiple projects. It maps the `create_cxt_graph` function over zipped pairs of project context JSON file paths and corresponding output directories, utilizing up to `args.nprocs` processes for concurrent execution.

Note: Usage points include ensuring that the correct task type is specified via `args.task`, providing valid file or directory paths for input and output, and optionally specifying the number of parallel processes with `args.nprocs`. Proper setup of these parameters is crucial for successful execution of the function.
