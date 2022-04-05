# Report : F#

The program was designed to be like the Ruby version, as it implemented a tree structure that would work in F# as well. Any expression strings that passed the syntax error checking are transformed into a tree structure using F# types. These types have specific functions and variables, but all inherit type Expr allowing a tree to be created. The tree is then compared to the target string using a global variable to move along the string and local variables for the current expression section.

Because F# is a functional language, it is built on functions rather than objects, unlike Ruby. The program expands the complicated methods used in Ruby into smaller more usable functions, allowing sections to be reused in other functions. This is what functional design is about, breaking large tasks into smaller, easier to understand and use, pieces. Having smaller functions makes the code much more readable, as the Ruby program’s methods became long and contained too many indents, making it hard for the reader to work out which sections were which. However, separating the methods also prevents the reader from being able to read the program like a book. Thankfully, F# uses more expressive language for methods, resulting in a language more like English than machine code. Finally, the largest problem with F# was the inability to run the whole program from the code editor.

However, there were problems with this approach as it made searching for bugs more difficult. F# requires a function to be already declared before it can be used. This meant that recursion, and having a function call its parent, became tricky and confusing. Functions could be stringed together using the `and` keyword, allowing them to call functions not already declared (only if they were connected). The result of this was that the order of the functions became confusing.

To organise the code more efficiently, it was separated into modules. This is a widely used technique to keep related code together and avoid name conflicts in the program. Modules can be referenced by name, avoiding name conflicts, or just opened in the new module, which allows all functions and variables to be used freely.

A problem was discovered when the program was first run on more than one input. Unlike Ruby, where a new version of the class was called for each new expression and target, F# used the functions and variables as it had left them. This meant that the global variables weren’t reset to 0. In the Create module, the global variable `indexStringExpr` kept track of where in the expression string the program was up to. Not resetting it to 0 resulted in following expression strings returning empty trees. Also, in the Check module, the global variable `indexOfString`, which kept track of where in the target string the program was, started part way into the string (or past its length), resulting in the program ignoring those first characters and returning false results. This was resolved by resetting the global variables manually.

Because F# is a functional language which uses functional design, it provides an easier approach to extending programs than Ruby which is object-orientated and uses larger methods. This is because new functions can be slotted into the code without too much editing. However, because this program is not neatly organised, it requires the editor to understand the entirety of the code to extend it. There are also varieties of functions which would need editing if extensions were required. An example is the `checkStar` function. The target of the asterisk, in the current program, can only be a dot, a literal, or another tree contained inside parentheses. Thus, the match function in checkStar expects the default match to be a literal. If the program was extended to include another expression which could the target of an asterisk, the program would break when it tries to assign it to a Literal type.

A different approach could be to separate the expression and target strings into tuples. From the expression `a*a`, the section `a*` could be paired with ‘aaaaa’ making it easier to later remove a character to pair with the section ‘a’.

In conclusion, this program parsed expression strings into tree structures to compare with a target string. It was written in a functional language which increased code reuse and reduced long methods into smaller functions. While functions allowed the code to be more separated than the Ruby version, there is still tightly connected functions.