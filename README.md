# 331_Notes
## Jan 25

### What is a compiler
  >program that transform source code to target lanuge <br>

High level ------------- Low level <br>
Closer to humans        closer to hardware <br>
C, C++, Python    ------      IA32, IA62<br>

### Compileer
Source Text -> Compiler [front end(analysis] - [semantics] - [back end (synthesis)] -> executable code


### Interpreter
slower than compiler but simpler in that it outputs the execution itself
Source Text -> [front end(analysis] - [semantics] - [execution engine] -> output

 ### Just in time Complier (i.e. JAVA)
 Java create an interpreter that works on non-machine dependent. <br>Use java virtual machine (JVM) 
 
 ### Qualities of a good compiler
 - bug free
 - correct code
 - output fast
 - fast run time
 - time proportional
 - good diagnostics for syntax erros (good ability to address errors) 
 - solid debugger
 - cross language calls
 - Consistent optimization (Can it optimize/reduce your code: looking for repeats of code ecevutions) 

### Phases of compilation
1. scanner (lexical analysis)         <br> highlighting key words (i.e. int, char) <br> ``` a = a + b ```: need a token for each element(5 total tokens)  -> Token Stream
2. parser (sytax analysis)         <br>    -> Parse tree
3. Semantic Analysis and intermediate code generation <br> checks that statement is meanigful and correct <br> ```int a[n];``` : the n must be a constant in the allocation of space<br> ```a[i] = b``` : i must be within the range of the array<br> - >Abstract syntax tree
4. Machine-independent code imporvement (optional) <br> optional optimization -> intermediate form 
5. Target code generation    <br>namely assembley language ->target language

### Example of tokenization
```x = b * b - 4``` === to lexeme tokenization```<ID,'x'> <EQ> <ID, 'b'> <MULT> <ID, 'b'> <MINUS> <INT, 4> ```

fuck 
                  homie
