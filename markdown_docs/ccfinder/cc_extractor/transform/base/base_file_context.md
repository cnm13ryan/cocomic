## ClassDef CodeBlock
**CodeBlock**: Wrapper class designed to encapsulate a block of text along with its corresponding line numbers within a file.

attributes:
· text: A string representing the content of the code block.
· start_lineno: An integer indicating the starting line number of the code block in the source file.
· end_lineno: An integer indicating the ending line number of the code block in the source file.

Code Description: The CodeBlock class is a simple yet effective structure used to store and manage information about specific segments of code. It holds the actual text of the code block as well as its position within the file, identified by start and end line numbers. This class facilitates easy manipulation and retrieval of code snippets during processing or analysis tasks.

The constructor (__init__) initializes a new instance of CodeBlock with three parameters: the text content of the block, the starting line number, and the ending line number. These attributes are stored as instance variables for later use.

The to_dict method provides a way to convert the CodeBlock object into a dictionary format. This is particularly useful when serializing the code block data or preparing it for output in JSON-like structures. The dictionary includes the text of the code block and an array representing the range of line numbers, where the end line number is incremented by one to reflect an inclusive range.

Note: Instances of CodeBlock are typically created through methods like get_code_block in other classes, which extract relevant information from a source file or parsed syntax tree nodes. This class serves as a fundamental building block for more complex operations involving code analysis and transformation.

Output Example: A possible appearance of the code's return value when calling to_dict on an instance of CodeBlock could be:
{"text": "def example_function():\n    print('Hello, world!')", "line_no": [10, 12]}
### FunctionDef __init__(self, text, start_lineno, end_lineno)
**__init__**: Initializes a new instance of the CodeBlock class, representing a block of code with its text content and line number range.

parameters:
· text: A string containing the actual source code that this CodeBlock represents.
· start_lineno: An integer indicating the starting line number in the original source file where this code block begins.
· end_lineno: An integer indicating the ending line number in the original source file where this code block ends.

Code Description: The __init__ method is a constructor for the CodeBlock class. It takes three parameters to set up a new instance of the class. The 'text' parameter holds the string representation of the code block, which could be a function definition, a loop, or any other segment of source code. The 'start_lineno' and 'end_lineno' parameters specify the exact range of lines in the original file that this code block occupies. These attributes are crucial for tools like code analyzers and refactoring utilities to understand where specific blocks of code exist within larger files.

Note: When creating an instance of CodeBlock, ensure that the 'text' accurately reflects the content between 'start_lineno' and 'end_lineno'. This method does not perform any validation on these inputs, so it is the responsibility of the caller to provide correct values. This initialization allows for easy tracking and manipulation of code segments in larger source files.
***
### FunctionDef to_dict(self)
**to_dict**: This function converts a CodeBlock object into a dictionary representation, capturing essential attributes of the block.
parameters:
· No explicit parameters: The method does not accept any external parameters; it operates on the instance's attributes.

Code Description: Detailed analysis and description.
The `to_dict` method is designed to serialize a CodeBlock object into a Python dictionary. This serialization process involves extracting two key pieces of information from the CodeBlock instance:
- "text": This key holds the actual text content of the code block, which is stored in the `self.text` attribute of the CodeBlock instance.
- "line_no": This key contains a list representing the line numbers that the code block occupies within its source file. The start and end line numbers are derived from `self.start_lineno` and `self.end_lineno + 1`, respectively. Note that `end_lineno` is incremented by one to ensure the range is inclusive of the last line.

This method facilitates easy conversion of CodeBlock objects into a format that can be easily manipulated, stored, or transmitted, such as for debugging purposes or further processing in data pipelines.

Note: Usage points.
The `to_dict` function is particularly useful when you need to represent code blocks in a structured and easily accessible form. This could be necessary for logging, analysis, or integration with other systems that require data in dictionary format. Developers can use this method to quickly convert complex objects into simpler structures without losing critical information.

Output Example: Mock up a possible appearance of the code's return value.
{
    "text": "def example_function():\n    print('Hello, world!')",
    "line_no": [5, 7]
}
In this example output, the dictionary represents a CodeBlock that contains a simple function definition. The text includes two lines: the function declaration and its body. The line numbers indicate that this block starts at line 5 and ends at line 6 (inclusive), with `end_lineno + 1` ensuring the end is correctly represented as 7 in the output list.
***
## ClassDef BaseFileContext
**BaseFileContext**: Base class designed to extract file-level focal context from source files, including imports, function definitions, and class definitions.

**attributes**:
· context: An instance of BaseCodeContext that provides the coding context necessary for extracting information from the file.

**Code Description**: The `BaseFileContext` class serves as a foundational structure for parsing and analyzing source code files. It includes methods to retrieve specific components of the file such as imports, function definitions, and class definitions. However, these methods are not implemented in the base class and must be overridden by subclasses to provide concrete functionality.

The `_dfs` method is a helper function that performs a depth-first search on the Abstract Syntax Tree (AST) of the source code. It traverses the tree, checking each node against a list of specified types (`node_types`). When a match is found, it calls a provided callback function with the matching node as an argument.

The `collect_nodes` method uses `_dfs` to gather all nodes in the AST that match the specified types and returns them as a list. This method simplifies the process of extracting specific elements from the AST by abstracting away the traversal logic.

The `get_code_block` method converts a given node into a `CodeBlock` object, which encapsulates the text of the code block along with its starting and ending line numbers. This is useful for isolating and manipulating specific parts of the source code.

Finally, the `parse` method consolidates all the extracted information (imports, function definitions, class definitions) into a single dictionary and returns it. This method provides a unified interface for accessing various components of the file context.

**Note**: The `BaseFileContext` class is intended to be extended by language-specific subclasses that provide concrete implementations for extracting imports, functions, and classes from source files in different programming languages.

**Output Example**: A possible appearance of the code's return value when calling `parse()` on a subclass instance could be:
```
{
    "imports": ["import os", "from math import sqrt"],
    "functions": [
        {
            "func_signature": "def calculate_area(radius):",
            "func_name": "calculate_area",
            "func_parameters": [{"name": "radius", "type": null}],
            "func_return_type": null,
            "func_decorator": null,
            "func_docstring": "",
            "func_body": "    return 3.14 * radius * radius\n",
            "return_statements": ["return 3.14 * radius * radius"],
            "func_orig_str": "def calculate_area(radius):\n    return 3.14 * radius * radius\n",
            "byte_span": [20, 65],
            "start_point": [1, 0],
            "end_point": [2, 37],
            "use_dependencies": {}
        }
    ],
    "classes": [
        {
            "class_signature": "class Circle:\n",
            "class_name": "Circle",
            "class_baseclass_list": [],
            "class_docstring": "",
            "class_variables": [],
            "instance_variables": ["radius"],
            "functions": [
                {
                    "func_signature": "    def __init__(self, radius):",
                    "func_name": "__init__",
                    "func_parameters": [{"name": "self", "type": null}, {"name": "radius", "type": null}],
                    "func_return_type": null,
                    "func_decorator": null,
                    "func_docstring": "",
                    "func_body": "        self.radius = radius\n",
                    "return_statements": [],
                    "func_orig_str": "    def __init__(self, radius):\n        self.radius = radius\n",
                    "byte_span": [70, 124],
                    "start_point": [3, 4],
                    "end_point": [4, 35],
                    "use_dependencies": {"radius": ["radius"]}
                }
            ],
            "class_orig_str": "class Circle:\n    def __init__(self, radius):\n        self.radius = radius\n",
            "byte_span": [67, 124],
            "start_point": [2, 0],
            "end_point": [4, 35]
        }
    ]
}
```
### FunctionDef __init__(self, context)
**__init__**: Initializes an instance of BaseFileContext by setting up its coding context.
parameters:
· context: An object of type BaseCodeContext, which holds various parsed code structures such as lines of code, tokens stream, and more.

Code Description: The __init__ method is a constructor for the BaseFileContext class. It takes one parameter, `context`, which must be an instance of the BaseCodeContext class. This method assigns the provided `context` to the instance variable `self.context`. The purpose of this setup is to associate the file context with its corresponding parsed code structures encapsulated within a BaseCodeContext object.

The BaseFileContext class is designed to work in conjunction with BaseCodeContext, which provides a structured representation of parsed source code. By initializing BaseFileContext with a BaseCodeContext instance, developers can leverage the parsed data and methods provided by BaseCodeContext for further analysis or transformation tasks.

Note: Usage points include creating an instance of BaseFileContext with a pre-existing BaseCodeContext object that has already been initialized with parsed AST, source code, and language information. This setup allows for easy access to the structured code data within the context of file-level operations or transformations.
***
### FunctionDef imports(self)
**imports**: Get all the imports from the file.
parameters:
· None: This function does not take any parameters.

Code Description: The `imports` method is designed to retrieve all import statements present within a file. It is intended to be part of a larger class, likely responsible for parsing and analyzing file contents in some form of code extraction or transformation process. Currently, the method raises a `NotImplementedError`, indicating that its functionality has not yet been implemented. This suggests that developers using this class should expect an error if they attempt to call `imports` directly until it is properly defined.

Note: The `parse` method within the same class relies on `imports` to consolidate file context information, which includes imports along with function and class definitions. Since `imports` raises a `NotImplementedError`, calling `parse` will also result in an error unless `imports` is implemented to return a dictionary of import statements or some other appropriate data structure. This highlights the importance of implementing this method for the overall functionality of the class.
***
### FunctionDef function_defs(self)
**function_defs**: Get all function level definitions from the file.
**parameters**:
· No parameters: This method does not accept any input parameters.

**Code Description**: The `function_defs` method is designed to retrieve all function-level definitions present within a file. Currently, this method raises a `NotImplementedError`, indicating that its functionality has not yet been implemented in the codebase. Its purpose is to serve as a placeholder for future development where it will be responsible for parsing and returning a structured representation of functions defined in the file.

The method is intended to be part of a larger context extraction process, as evidenced by its usage within the `parse` method of the same class (`BaseFileContext`). In this context, `function_defs` is called alongside other methods like `imports()` and `class_defs()`, which suggests that it should return a dictionary or similar structure containing function definitions. This data would then be combined with import statements and class definitions to form a comprehensive file context.

**Note**: Usage points: The method is currently not functional due to the raised `NotImplementedError`. Developers are advised to implement this method if they need to extract function definitions from files as part of their project's requirements. Once implemented, it should be integrated into the existing parsing workflow to ensure that all relevant file components (imports, functions, and classes) are correctly captured and returned in a structured format.
***
### FunctionDef class_defs(self)
**class_defs**: Get all class level definitions from the file.
parameters:
· None: This function does not take any parameters.

Code Description: The `class_defs` method is designed to retrieve all class-level definitions present within a file. It is part of the `BaseFileContext` class, which appears to be responsible for parsing and extracting various components of a source code file. The method currently raises a `NotImplementedError`, indicating that its functionality has not yet been implemented in this version of the codebase.

The purpose of this function is to provide a structured representation of all classes defined within the file it processes, which could include class names, methods, attributes, and possibly other metadata relevant to each class. This information would be useful for various static analysis tasks, such as code documentation generation, refactoring tools, or dependency analysis.

Note: Usage points. The `class_defs` method is called by the `parse` method of the same class (`BaseFileContext`). The `parse` method consolidates different aspects of a file's context, including imports, function definitions, and class definitions (via `class_defs`). It returns a dictionary containing all these components, which suggests that once implemented, `class_defs` will play a crucial role in providing comprehensive information about the classes within a file. Developers working with this codebase should implement the `class_defs` method to ensure that it accurately extracts and returns class-level definitions from files as expected by other parts of the system.
***
### FunctionDef _dfs(self, node, node_types, callback)
**_dfs**: Helper function to traverse a parsed Abstract Syntax Tree (AST) using Depth-First Search (DFS).
**parameters**:
· node: The current node being visited in the AST, of type TSNode.
· node_types: A list of strings representing the types of nodes that are of interest for processing.
· callback: A callable function to be executed when a node of one of the specified types is encountered.

**Code Description**: This function implements a depth-first search algorithm to traverse an Abstract Syntax Tree (AST). It starts from a given node and recursively visits each child node. If the type of the current node matches any type listed in `node_types`, it calls the provided callback function with the current node as its argument. The traversal continues until all nodes have been visited, ensuring that every branch of the tree is explored to its deepest level before backtracking.

The `_dfs` function is a recursive helper method designed for internal use within the class. It does not return any value directly but instead relies on side effects through the callback mechanism. This allows flexibility in what actions can be taken when nodes of interest are found, as different callbacks can be passed depending on the requirements of the caller.

**Note**: Usage points include scenarios where specific types of nodes need to be identified and processed within an AST. For example, in the `collect_nodes` method, `_dfs` is used to gather all nodes that match certain types into a list. This demonstrates how `_dfs` can be leveraged to perform tasks such as collecting or analyzing specific elements of a parsed code structure efficiently.
***
### FunctionDef collect_nodes(self, node_types)
**collect_nodes**: This function gathers all nodes from an Abstract Syntax Tree (AST) that match specified node types.

**parameters**:
· node_types: A list of strings where each string represents a type of node to be collected from the AST.

**Code Description**: The `collect_nodes` method is designed to traverse an AST and collect nodes that belong to specific types. It initializes an empty list named `result` to store the matching nodes. Inside this function, a nested callback function `_cb` is defined, which simply appends any node it receives to the `result` list.

The core of the collection process is handled by calling the `_dfs` (Depth-First Search) method with three arguments: the root node of the AST (`self.context.tree.root_node`), the list of node types to collect (`node_types`), and the callback function `_cb`. The `_dfs` method performs a depth-first traversal of the tree, invoking the callback for each node that matches one of the specified types. This ensures that all nodes of interest are identified and added to the `result` list.

After the traversal is complete, the `collect_nodes` method returns the `result` list containing all collected nodes.

**Note**: The `collect_nodes` function is versatile and can be used in various scenarios where specific node types need to be extracted from an AST. For example, it is utilized in methods like `imports` and `classes` within the `PythonASTFileContext` class to gather import statements and class definitions, respectively.

**Output Example**: If the AST contains nodes of type "import_statement" and "class_definition", calling `collect_nodes(["import_statement", "class_definition"])` might return a list similar to:
[
    TSNode(type="import_statement", ...),
    TSNode(type="class_definition", ...)
]
Each `TSNode` object in the list represents a node from the AST that matches one of the specified types, encapsulating details such as the node type and its position within the source code.
#### FunctionDef _cb(n)
**_cb**: This function serves as a callback within the context of collecting nodes, appending each node to a predefined list named `result`.

parameters:
· n: Represents an individual node that is to be added to the result list.

Code Description: The function `_cb` takes one parameter, `n`, which is expected to be a node object or data structure. Inside the function body, there is a single line of code where the `append` method is called on the `result` list with `n` as its argument. This action adds the node `n` to the end of the `result` list. The purpose of this callback function is typically to gather nodes during some form of traversal or processing, such as in a tree or graph structure, and store them for further use.

Note: Usage points include scenarios where `_cb` is used as part of a larger algorithm that requires collecting nodes from a data structure. It is important that the `result` list is defined in the scope where `_cb` is called to avoid NameError exceptions. This function is likely a helper within a more complex system, such as a file context processor or transformer, where nodes are systematically collected and processed.
***
***
### FunctionDef get_code_block(self, node)
**get_code_block**: This function converts a given syntax tree node into a CodeBlock object, encapsulating the block of text and its corresponding line numbers within a file.

parameters:
· node: An instance of TSNode representing the syntax tree node that needs to be converted into a code block.

Code Description: The get_code_block method is designed to extract a specific segment of code from a source file based on a provided syntax tree node. It begins by calling the `get_node_window` method, which retrieves the lines of text corresponding to the given node. This method returns a list of line objects that include both the line number and the actual text content.

The get_code_block function then determines the starting (`st_lineno`) and ending (`ed_lineno`) line numbers from this list of lines. It constructs the text content of the code block by joining the text attributes of each line object in the list with newline characters. This concatenated string represents the complete text of the code block.

Finally, the method returns a new instance of the CodeBlock class, initialized with the constructed text and the identified start and end line numbers. The returned CodeBlock object can be used for further processing or analysis tasks that require information about specific segments of code within a file.

Note: This function is typically used in conjunction with other methods that parse source files into syntax trees and extract relevant nodes for analysis. It serves as a fundamental step in transforming abstract syntax tree nodes into more manageable and informative data structures, such as CodeBlock objects.

Output Example: If the node corresponds to lines 5 through 7 of the source file, containing the following text:
```
def example_function():
    print("Hello, world!")
```
The function would return a CodeBlock object with the following attributes:
- text: "def example_function():\n    print(\"Hello, world!\")"
- start_lineno: 5
- end_lineno: 7

When calling the `to_dict` method on this instance of CodeBlock, the output would be:
{"text": "def example_function():\n    print(\"Hello, world!\")", "line_no": [5, 8]} 
Note that the line number range is inclusive of the start and exclusive of the end in the dictionary representation.
***
### FunctionDef parse(self)
**parse**: Return a consolidated file context including imports, function definitions, and class definitions.
parameters:
· None: This function does not take any parameters.

Code Description: The `parse` method is designed to gather and consolidate various components of a source code file into a single dictionary. It achieves this by calling three other methods within the same class: `imports()`, `function_defs()`, and `class_defs()`. Each of these methods is expected to return a dictionary containing specific parts of the file's context.

The method initializes an empty dictionary named `file_context` and then updates it with the results from each of the three methods. The `update()` function merges the dictionaries returned by `imports()`, `function_defs()`, and `class_defs()` into `file_context`. This consolidation allows for a unified view of the file's structure, including all its imports, functions, and classes.

Note: Usage points. The `parse` method is intended to be called when a developer needs a comprehensive overview of a file's context. It relies on the successful implementation of `imports()`, `function_defs()`, and `class_defs()` methods. Currently, these methods raise a `NotImplementedError`, which means calling `parse` will result in an error unless these methods are properly implemented to return dictionaries containing the respective components of the file.

Output Example: A possible appearance of the code's return value could be:
{
    'imports': {'numpy': 'np', 'pandas': None},
    'function_defs': {
        'calculate_sum': {'parameters': ['a', 'b'], 'body': 'return a + b'},
        'greet_user': {'parameters': ['name'], 'body': "print(f'Hello, {name}!')"}
    },
    'class_defs': {
        'Calculator': {
            'methods': {
                '__init__': {'parameters': ['self', 'value'], 'body': 'self.value = value'},
                'add': {'parameters': ['self', 'num'], 'body': 'self.value += num'}
            }
        },
        'User': {
            'attributes': ['name', 'age'],
            'methods': {
                '__init__': {'parameters': ['self', 'name', 'age'], 'body': 'self.name = name\nself.age = age'},
                'display_info': {'parameters': ['self'], 'body': "print(f'Name: {self.name}, Age: {self.age}')"}
            }
        }
    }
}
***
