# Report : Ruby

## 1. Program Design
 
### 1.1 The design of the program, and why I built it that way
 
The program’s design is centred around the five classes which represent the five patterns:
 
The `*` pattern means that the string can include the previous element zero or more times. This pattern is represented by removing the previous element from the main expression and passing it into a Star object.  The previous element can be retrieved later but will be considered as a Star inner object and won’t return a false negative as it would if left in the main expression. There is also a support method that returns true if the inner object is of Dot class, reducing repeated code.
 
The `.` pattern matches any single character in the string. This pattern is represented by the Dot class. It doesn’t require any special methods.
 
The `|` pattern means that the string can include either what is on the left or the right. This is represented by removing the entire main expression and saving it into a Pipe object, then saving the second half when the rest of the expression has been read (only done if the first element of the main expression is a Pipe object). To represent this in the class, the Pipe object has an array of arrays of objects, with the top array representing the different options, and the inner array representing the expression. More options can be added using a side method.
 
The `()` pattern groups together multiple elements. This is represented by the Parent class, which holds an array of objects representing the expression inside the parentheses. There is also a method to return true if the last object is .* which means that the next pattern might start a while back.
 
Finally, any literal character not given a specific meaning above is represented by the Literal class. The literal character is saved inside the class, and the check methods returns true if the given input matches the inner character. Another method is provided to check if the inner character is empty, used only for expressions like `|a` where the string could be “” or “a”.
 
The Read class accepts two strings: the expression and the string. The expression is immediately checked for wrong syntax then, if it’s correct, separated into the classes mentioned above and saved into an array. E.g. `(a)` is represented by `[<Parent @inner=[<Literal @inner=”a”>]>]`. The expression array is then compared to the string and the response saved. This uses a class variable to iterate through the string and a local variable for the expression due to the need to check through inner expressions (like with Parent objects).  The response method can then be called which either returns SYNTAX ERROR or returns YES or NO depending on the result of comparing the expression and string.
 
`ruby Read expression.txt target.txt` runs the Read file which opens the given files, iterates through each line, passes the two strings (expression and target) to a Read object, and prints the response. It is in its own file so the parsing and comparing abilities of the Read object can be used elsewhere without removing code.
 
### 1.2 How the designs used are typically found in common application areas for the language and how it relates to this program
 
This is similar to the Composite Design Pattern which builds a hierarchy of tree objects. Three main classes make up the Composite Design Pattern:
 
>The component is the interface that all the nodes and leaves are based off.
>The composite is the node, which is a component and holds components.
>Finally, the leaf is the leaf node, which is a component.
 
In this program, the Object class is the interface as the only method all need to share is the `.to_s` method. The composites are represented by the Parent, Star and Pipe classes which hold inner components. Finally, the leaves are the Dot and Literal classes. The character inside the Literal class is not a component as it has to be a character, not any Object.
 
This program also follows a simple version of the Interpreter Design Pattern which parses a sentence structure from given data then interprets whether the given string matches. More commonly, the Interpreter Design Pattern extends regular expressions to interpret a set of strings.
 
### 1.3 Alternate approaches to the program
 
An alternative method of designing this program could be to use the inbuild regular expression tester and sanitise the regular expression before parsing it. This would result in a program less likely to produce false-positive and negatives but might break if patterns are forgotten or new patterns are added which the program doesn’t sanitise against.
## 2. Language Aspects
 
### 2.1 The aspects of the language used in the program
 
Ruby is an object-oriented program language. However, unlike Java, it is able to run code outside of methods and classes. This removes the use of a main method and makes testing applications and abilities of methods easier. The arguments from the command line can be retrieved using ARGV. This method is used in the Read file to access the two filenames which the user provides.
 
### 2.2 The aspects of the language not used in the program, and why they weren’t used
 
The main data structure of Ruby is the hash (dictionary) which keeps data stored in order based on its hash value. This is a very efficient Collection structure. However, I wasn’t able to use this in my program as the Collections needed to be ordered based on order they were added. An example is the main expression which represents the expression using objects. This has to be ordered to match the original string expression, else it is breaking the expected behaviour of the program.
 
Dynamic Typing doesn’t require explicit declaration of the variables before use. This if useful during coding but can lead to problems going unnoticed, including trivial mistakes which would normally be caught. An example is the need to convert an object to string before adding to the end of a string. Each of the pattern classes have a `.to_s` method so printing the expression matches the original string. However, add the inner object to the end of the string wouldn’t work.
`s += @inner` doesn’t work because you’re trying to add an Object to a String.
`s += @inner.to_s` does work because you’re adding a String to a String
 
Ruby allows iteration through arrays using `.map` and `.each`. This provides an easy way to access all the entries without the need for a loop variable. The `.map` provides an iterator to go through all the contents, edit each, and collect all the new contents into a new Collection. I used this to map the contents of an array of Objects to their string versions, proving very useful for debugging. The `.each` method iterates through each entry. This method proved useful for going through each possible option for the Pipe pattern. An extension to this method is the `.each_with_index` method which allows access to the current index of the Iterator. Using this, I was able to run through each possibility, check if it matches, and save the longest correct response. This solved the problem of whether `.` or `..` matched ab.
 
Separating the pattern types into classes allowed for more separated behaviour and unique methods. However, it resulted in arrays of Objects. Ruby has a method which confirms the class of an Object, which is widely used in this program. A better method of finding out the pattern type could have been using an interface with a method to return pattern type which all pattern classes implemented.

## 3. Likes and Dislikes of the Language
 
### 3.1 What I liked/didn’t like, or found helpful/unhelpful in using the language
 
Ruby is human-centric and was designed to emphasise productivity and ease of use:
 
It offers many ways to complete a task, compared to Python’s “there should only be one obvious way to do it” mentality. For example, size and length can both be used for the same functionality for an array or string, allowing the user to use their personal preferences.
 
It uses a clean language that appears more similar to human language, compared to Java which is based on machine code. For example, the question-mark (`?`) appended to the end of a predicate-like method makes the line read more like a question and reminds the user of the ability of the method.
 
It has similar language patterns to Java, allowing a quick transfer between languages, though converting back to Java does result in lots of forgotten semi-colons. Due to being widely used, many tutorials or answered questions are already available on the internet, allowing any difficulties to be solved easily.
 
### 3.2 Aspects of the program that the language made particularly simple to express
 
Writing the method for checking whether a string matches a Pipe pattern appeared to be originally easy:
- Save the current index of the target string
- Ruby offers a method to iterate through each entry in an array. Use this to iterate through the Pipe’s options.
- Run the check method on each option, resetting the string’s index to the saved index, until one returns true.
- Break the loop and all’s good!

However, this only works if the first option found is the best. A pattern like `.|..` could lead to problems. Thankfully, Ruby also had a method to iterate through each entry and provide the index!
- Save the option which moved the string index the furthest
- If the longest length is still zero, and the option doesn’t move the index (i.e., the first option of `|b`.

Thus, Ruby was able to make a problem so much simpler by providing a large variety of methods with similar, but not identical, abilities.
 
### 3.3 Aspects of the language and the program design that together enable, or inhibit, future extensions
 
The current program is designed so the parser and comparer are in a separate file from the file reader. This means that it can be included in other programs with a few lines of code. However, due to the decision to not separate expression parsing and checks into separate pattern classes, adding an extension might prove difficult.
 
Keeping the checks and parsing in one class allowed the use of class variables which, in turn, allowed recursion within the program. If, for example, a new pattern type was to be added, this would require editing the main methods. The largest problem is the repeated code due to the Star pattern and the Parent pattern. The Star pattern uses almost identical code when checking a Literal or a Parent as the logic behind the Star pattern requires slightly different handling. The Parent pattern repeats the code which converts the expression string into Objects as if requires a different logic to stop the while loop. An extension would need to edit this code, making sure that all the repeated code is kept up to date.
