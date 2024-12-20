## ClassDef Token
**Token**: Wrapper class around tree-sitter Node to facilitate easier manipulation and access of token data.

attributes:
· node: An instance of TSNode representing a syntax tree node.
· code_bytes: A bytes object containing the source code from which the node is derived.

Code Description: The Token class encapsulates a tree-sitter Node, providing additional functionality through properties and methods. It initializes with a TSNode and its corresponding source code in bytes. The extent property extracts the substring of the source code that corresponds to the token's position, decoding it from bytes to a UTF-8 string. The line_start and line_end properties provide the starting and ending column numbers of the token within its line, respectively. The __str__ method returns a string representation of the token, showing its extent.

Note: This class is used in parsing source code into tokens, which are then manipulated or analyzed further. It is utilized by methods such as _parse in BaseCodeContext to create Token instances from syntax tree nodes and by is_multiline_token to determine if a token spans multiple lines.

Output Example: If the token represents the keyword "def" at the start of a function definition on line 5, column 0, the extent would be 'def', line_start would be 0, and line_end would also be 0. The string representation of this Token instance would be "Token('def')".
### FunctionDef __init__(self, node, code_bytes)
**__init__**: Initializes a new instance of the Token class with a given tree-sitter node and corresponding code bytes.

parameters:
· node: Represents a node from a parsed syntax tree, specifically of type TSNode. This node encapsulates information about a particular element within the source code.
· code_bytes: A byte string that contains the actual code associated with the node. It represents the raw bytes of the code snippet that the node corresponds to.

Code Description: The __init__ method is a constructor for the Token class, responsible for setting up new instances of this class. When an instance of Token is created, it takes two parameters: 'node' and 'code_bytes'. The 'node' parameter is expected to be an object of type TSNode, which typically comes from parsing source code using a tree-sitter parser. This node provides structural information about the parsed code element. The 'code_bytes' parameter is a byte string that holds the exact bytes of the code snippet that the node represents. By storing these two pieces of information together in a Token object, it becomes easier to associate structural data (from the node) with the actual source code (from code_bytes).

Note: Usage points include initializing tokens when parsing and processing source code. This is particularly useful in tools like code analyzers or transformers where understanding both the structure and content of code elements is crucial. Developers should ensure that the 'node' parameter is indeed a TSNode object from a tree-sitter parser, and 'code_bytes' accurately reflects the bytes corresponding to that node's position in the source code.
***
### FunctionDef extent(self)
**extent**: This function retrieves the substring of source code corresponding to a token by using byte indices stored in the node attribute.
parameters:
· None: The extent method does not take any parameters; it operates on the instance's attributes.

Code Description: The extent method is designed to extract and return the exact portion of source code that a particular token represents. It achieves this by slicing the `code_bytes` array from the start byte index (`self.node.start_byte`) up to, but not including, the end byte index (`self.node.end_byte`). After extracting the relevant bytes, it decodes them into a UTF-8 string using the `.decode("utf8")` method. This decoded string is then returned as the output of the function.

Note: The extent method is crucial for accurately representing tokens in their original form within the source code. It is used by other methods such as `__str__` to provide a human-readable representation of the token and by `token_partial_pos` to calculate positions relative to the start of the token's extent.

Output Example: If the `code_bytes` contains the bytes for the string "def my_function():", and the node's `start_byte` is 4 and `end_byte` is 13, then calling `extent()` would return the substring "my_funct".
***
### FunctionDef line_start(self)
**line_start**: This function retrieves the starting column index of a token within its line.
parameters:
· No parameters: The function does not accept any input arguments.

Code Description: The `line_start` method is designed to return the column number where a specific token begins on its respective line in the source code. It accesses the `start_point` attribute of an object, presumably representing a node or token in a parsed syntax tree (AST). The `start_point` attribute is expected to be a tuple containing two elements: the first element represents the line number and the second element represents the column index. By returning `self.node.start_point[1]`, the function extracts and returns only the column index, which indicates where the token starts on its line.

Note: This method is particularly useful in code analysis tools or editors that need to highlight specific tokens or provide detailed error messages pointing to exact locations within source files.

Output Example: If a token begins at the 5th character of a line, calling `line_start` would return `4`, as column indices are typically zero-based.
***
### FunctionDef line_end(self)
**line_end**: This function retrieves the column position at which a token ends within its line.
parameters:
· None: The function does not accept any parameters.

Code Description: The `line_end` method is designed to return the column index where a specific token's representation concludes on its respective line. It accesses the `end_point` attribute of the `node` object, which contains a tuple representing the end position of the node in terms of (row, column). By indexing this tuple with `[1]`, the function extracts and returns only the column number.

Note: This method is particularly useful when parsing or analyzing source code to understand the exact positioning of tokens within lines. It can be used for syntax highlighting, error reporting, or any feature that requires precise location information about elements in a parsed document.

Output Example: If the token ends at the 15th column on its line, `line_end` would return `15`.
***
### FunctionDef __str__(self)
**__str__**: This function provides a human-readable string representation of a Token object.
parameters:
· None: The __str__ method does not take any parameters; it operates on the instance's attributes.

Code Description: The __str__ method is designed to return a string that represents the Token object in a way that is easy for humans to understand. It achieves this by using an f-string to format the output, which includes the substring of source code corresponding to the token. This substring is obtained by calling the `extent` method on the Token instance.

The `extent` method extracts and returns the exact portion of source code that a particular token represents. It does so by slicing the `code_bytes` array from the start byte index (`self.node.start_byte`) up to, but not including, the end byte index (`self.node.end_byte`). After extracting the relevant bytes, it decodes them into a UTF-8 string using the `.decode("utf8")` method. This decoded string is then used by the __str__ method to provide a clear and concise representation of the token.

Note: The __str__ method is particularly useful for debugging and logging purposes, as it allows developers to easily see what each Token object represents in terms of the original source code. It leverages the `extent` method to ensure that the string representation accurately reflects the content of the token within the source code.

Output Example: If a Token object corresponds to the substring "my_function" in the source code, calling str(token) would return the string "Token('my_function')". This output clearly indicates that the token represents the "my_function" portion of the original source code.
***
## ClassDef SplitPoint
**SplitPoint**: Represents a specific point within a token where text can be split or analyzed further. This class is designed to encapsulate both the token itself and an offset indicating a position within that token.

attributes:
· token: An instance of the Token class, representing the lexical unit in the source code.
· offset: An integer value indicating the position within the token's extent where the split point is located. Defaults to 0 if not specified.

Code Description: The SplitPoint class is initialized with a token and an optional offset. The token parameter must be an instance of the Token class, which presumably contains information about a lexical unit in the source code, such as its type, value, and position within the file. The offset parameter specifies a particular position within this token's extent, allowing for more granular operations on the token.

The class provides three properties:
- token_line_start: Returns the starting column index of the token within its line.
- token_line_end: Returns the ending column index of the token within its line.
- token_partial_pos: Calculates and returns a position that is the sum of the token's starting column index and the minimum value between the offset and the length of the token's extent. This property helps in determining a precise location within the token, which can be useful for operations like inserting or splitting text.

Note: The SplitPoint class is particularly useful when performing detailed analysis or transformations on source code at a granular level, such as refactoring tools or syntax highlighters that need to manipulate specific parts of tokens.

Output Example: Assuming we have a Token instance representing the word "example" starting at column 5 and ending at column 11 in its line, creating a SplitPoint with an offset of 3 would result in:
- token_line_start = 5
- token_line_end = 11
- token_partial_pos = 8 (since 5 + min(3, len("example")) = 5 + 3 = 8)
### FunctionDef __init__(self, token, offset)
**__init__**: Initializes a new instance of the SplitPoint class, which represents a point within a token where a split may occur.

parameters:
· token: An instance of the Token class, representing the token around which the split point is defined.
· offset: An integer indicating the position within the token's extent where the split should take place. Defaults to 0 if not specified.

Code Description: The __init__ method sets up a new SplitPoint object by storing the provided token and offset as instance variables. The token parameter is expected to be an instance of the Token class, which encapsulates a tree-sitter Node and provides additional functionality for handling source code tokens. The offset parameter specifies where within the token's extent (the substring of the source code that corresponds to the token) the split should occur. By default, this value is set to 0, meaning the split point would be at the beginning of the token unless otherwise specified.

Note: This class is likely used in scenarios where tokens need to be divided or analyzed at specific points within their extent. For example, it could be useful for parsing or transforming code where certain operations need to occur at particular positions within a token. The offset parameter allows for precise control over where these operations take place.
***
### FunctionDef token_line_start(self)
**token_line_start**: This function retrieves the column index of the starting point of a token within its line.
parameters:
· None: The function does not accept any parameters.

Code Description: The function `token_line_start` is designed to return the column number where a specific token begins on its respective line. It accesses this information through the `start_point` attribute of the `node` object, which is an attribute of the `self.token`. The `start_point` is typically a tuple containing two integers: the first integer represents the row index (line number) and the second integer represents the column index within that line. By accessing `[1]`, the function specifically extracts the column index.

Note: This function assumes that `self.token` and `self.token.node.start_point` are properly defined and accessible, meaning they should have been initialized before calling this method. The returned value is zero-based, which means the first character of a line corresponds to column 0.

Output Example: If the token starts at the beginning of a line, the function will return 0. For instance, if a token "example" begins at the start of line 5 in the source code, `token_line_start` would return 0. Conversely, if "example" starts after five spaces on the same line, it would return 5, indicating that the token begins at column 5.
***
### FunctionDef token_line_end(self)
**token_line_end**: This function retrieves the column position at which a token ends within its line of code.
parameters:
· None: The function does not accept any parameters.

Code Description: The function `token_line_end` is designed to return the ending column index of a token's location in the source code. It accesses the `end_point` attribute from the `node` object, which is presumably part of a larger data structure representing parsed tokens or syntax elements. The `end_point` is expected to be a tuple where the first element represents the line number and the second element represents the column number. By returning `self.token.node.end_point[1]`, the function specifically extracts and returns the column index at which the token ends.

Note: This function assumes that the `token` object has a `node` attribute, which in turn contains an `end_point` tuple with at least two elements. Developers should ensure that these attributes are properly initialized before calling this method to avoid potential errors such as AttributeError or IndexError.

Output Example: If the token ends at column 25 on its respective line, the function will return `25`.
***
### FunctionDef token_partial_pos(self)
**token_partial_pos**: This function calculates a specific position within the extent of a token by adding an offset to the starting column index of the token's node.
parameters:
· None: The `token_partial_pos` method does not take any parameters; it operates on the instance's attributes.

Code Description: The `token_partial_pos` method is designed to determine a particular position within the substring of source code that a token represents. This position is calculated by adding an offset value to the starting column index (`start_point[1]`) of the token's node attribute. The offset is constrained by the length of the token's extent, ensuring it does not exceed the bounds of the token's substring.

The method first retrieves the starting column index of the token from `self.token.node.start_point[1]`. It then calculates the minimum value between the provided `offset` and the length of the token's extent (`len(self.token.extent)`). This ensures that the calculated position does not go beyond the end of the token. Finally, it returns the sum of the starting column index and this minimum offset value.

Note: The `token_partial_pos` method is useful for determining specific positions within a token's substring, which can be important for tasks such as syntax highlighting or error reporting in code editors and IDEs.

Output Example: If the token represents the substring "function" (starting at column 4) and the offset is 3, then calling `token_partial_pos` would return 7, indicating the position of the character 'c' within the token's extent.
***
