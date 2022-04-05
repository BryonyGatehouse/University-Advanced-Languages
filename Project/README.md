# Programming Project

Create a program which can parse a regex expression and perform comparisons. No external libraries or pre-programmed regex parsers can be used, so the parser and comparator should be all the student's code.

The program should be able to parse the following regex types:
- **character** : The character is matched literally (case sensitive).
- `.` : The dot character matches any character except new line.
- `*` : The asterisk character means the preceding character is optional and can be optionally duplicated.
- `|` : The pipe character means either the preceding character or succeding character will be matched.
- `()` : The round brackets create a group which is considered a single character for the above special characters.

Input will be given in the form of two files:
- The regex expressions
- The corresponding string to compare with
- Example:
  |File 1|File 2|
  |-|-|
  |`a`|`a`|
  |`.`|`a`|
  |`..`|`a`|

The output will be printed to the console:
- YES: `a` with `a`
- YES: `.` with `a`
- NO: `..` with `a`

Some regex expressions are not structured properly, example:
- `(`
- `(a(b)))`

and will have a SYSTEM ERROR message rather than YES or NO.
