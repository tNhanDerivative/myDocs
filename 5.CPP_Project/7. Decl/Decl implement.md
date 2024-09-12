# Class
`scanner.cpp`contains `token` and `scanner` class
`decl.cpp` contains the `parser` class and main function

1. Each `token` object represents a single input token
2.  A `scanner` object transform a stream of character into a stream of tokens
3.  A `parser` object analyzes sequences of tokens:
- Identifies each sequence of tokens that forms a valid declaration
- Translates each declaration into English


## Tokens
`Text`: character text that made up the symbol
`Category`: enumeration type indicate what kind of token.
`Category::no_more` mark the end of the input sequence of character

# Analyzing

## Dependency
- In general, class with fewer dependency are easier to test.
- `parser` depends on scanner and token.
	- `scanner` public function return tokens.
	- `token` constructors are only accessible to scanner.
==> So we will choose to test the `scanner` and `token` to be test first.

## first test
if there's no input at all the first token should be a no_more token
- set up an empty input stream
-  create a scanner with that input stream
- read token from the scanner
-  verify that the token is category::no_more

```cpp
scanner s{/* empty input */}
token t{s.get()}
if(t.kind() == token::no_more){
	/* report success */
} else{
	/* report failure*/
}
```
Problem is decl takes its input from cin.
But lucky my `scanner` constructor take any istream 
`istringstream`

```cpp
istringstream input{"123"};
scanner s{input}
```