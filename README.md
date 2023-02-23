# 331_Notes
## Jan 25

### What is a compiler
  >program that transform source code to target lanuge <br>
<pre>
High level ------------- Low level <br>
Closer to humans  ------      closer to hardware <br>
C, C++, Python    ------      IA32, IA62<br>
</pre>

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
for IDs the check is letter(letter|digit)^*^

### Expansion of compentns in a tree
Must take into consideration of order of presesedence 
>             =
>            /  \
>          x      -
>               /    \
>              *     4
>            /   \
>          b      b  

<br>
at each phase the data type is examined to ensure semantic analysis. <br> Ensure that data types are comparable with their expected operators

automatic type castings vs explicit typecasting vs implicit conversion<br>
At this state the compiler generates temporary variables to store the data in compliation of the equation

### Interpetation of expression
temporary values are stored during calculation<br>
R2 = b * b<br>
R1 = R2 - 4<br>
R2 = x * R1<br>

### Assembly Code Actions 
Moving the values within the regestires<br>
* Mov Rs, (sp+8)
* SAL R2, 2
* Mov R1, (sp+16)

### Error Checking
- Lexicial analysis 
- Sysntax analysis
- Semantica allanys


### Assignment 
Same research as 321 

# 20 FEB

- dealing with back-tracking

#### Deterministic solutions
- left recersion
  - easy to write
  - Top down parsers cannot handle/ can lead to a non-termination state (any recuresion must be right recursion
  - A -> A@ | B
  - Rewrite 
  - A -> BA' (where A' is new non terminal) 
  - A' -> @A' | E (empty symbol) ..... (slide 24)-25 
  
  example <br> 
  Expression E, Term T, Factor F, empty <br> 
  E -> E + T | E - T | T .....(T is the B in this scenario)<br>
  T -> T * F | T / F | F ......(F is the B in this scenario) <br> 
  
  moved to right recursion 
  E -> TE' ......make it BA'<br> 
  E' -> +TE' | - TE' | empty........make it A'@ for all @s<br>
  T -> FT' ...... make it BA' <br>
  T' -> *FT' | /FT' | empty........make it A'@ for all @s <br> <br><br> 

  keep an eye out for not immediate grammers <br> 
  S->Aa | B<br>
  A -> Sc | D<br><br>
  S->Aa -> Sca   or    A -> Sc -> Aac 
  
  
  #### another example
  - S->Aa | b
  - A ->Ac | Sd | f
  - S -> Aa -> Sda
  <br> 
  replace A -> Sd with A->Aad | bd
  
  ### Un class exercise
  #### quetion 1 eliminate left recursion 
  - T ->T*U | U 
    - T-> UT' 
    - T' -> *UT' | empty<br>
    

    
    
  - A -> A - (A) | int | int + A | int - A
    - A ->intA' | int + AA' | int - AA'
    - A' -> -(A) A' | empty<br>
    
    
    
  - S-> Aa | b
  - A -> Ac | Sd | c
    - replace A -> Sd with Aad | Ab
    - so 
    - A -> Ac | Aad | Ab | c
    - A -> cA'
    - A' ->  adA' | bA' | empty<br><br> 


- left factoring 

  # 22 Feb 
  
  ### Recursive Descent Pattern
  > - a top down parsing method
  > - use a set of mutually recursive procedure
  > - using predictive parsing with lockahead sybmols to decide uses
  
  ### Stack Based Table-Driven Pasring
  
<img width="667" alt="Screen Shot 2023-02-22 at 2 34 50 PM" src="https://user-images.githubusercontent.com/70354960/220787677-65266392-2519-4d7f-8554-8378baea750c.png">

  > use 3 tables
  > Stack                   Lookahead     Parse-Table Lookup
  > $S                        IF            M(S, IF( S->if E then S else S
  > $S, else, s, then, e, if  IF           (push to stack in reverse order) 
   > $S, else, s, then, e,    NUM          (pop last element if they last and get
   
  ###Example
  ~~~
  >     INT         *       +       (         )         $
  >  E  TX                          TX
  >  X                      +E                 empty     empty
  >  T  int Y                       (E)
  >  Y              *T     empty              empty     empty
  ~~~
  
  <br> int + (int) + (int)$<br>
  ~~~
  >   Stack         Lookahead                   Rule
  >   $E             INT                        E ->TX         
  >     XT             INT                    T -> int Y
  >     X, Y INT      INT                     (pop)
  >     X, Y          +                 Y -> empty
  >     X             +                  X ->  + E
  >     E +           +                     (pop)
  >     E             (                   E-> TX
  >     X, T          (                   T-> (E)
  >     X, ), E, (    (                   (pop)
  >     X, ), E       int                 E-> TX
  >     X, ), X, T    int                 T-> int Y
  >     X, ), X, Y, int int             (pop)
  >     X, ), x, y     )                y -> empty
  >     x, ), x       )                 x -> empty
  >     x, )          )                   (pop)
  >     x             +                 x-> + E
  >    E, +          +                   (pop)
  >     E             (                 E -> TX 
  >     X, T          (                   T-> (E)
  >     X, ), E, (    (                   (pop)
  >     X, ), E       int                 E-> TX 
  >     X, ), X, T    int                 T-> int Y 
  >     X, ), X, Y, int int             (pop)v
  >     X, ), x, y     )                y -> empty 
  >     x, ), x       )                 x -> empty 
  >     x, )          )                   (pop) 
  >     X             $                   x-> empty 
  >     $             $                 
 ~~~
<br>
### How to build parse trees
#### First Sets 
looking for terminal symbols, lol looking good 
<img width="667" alt="Screen Shot 2023-02-22 at 3 29 31 PM" src="https://user-images.githubusercontent.com/70354960/220795017-3d330a69-e472-457a-adc0-72f904eca2fa.png">

<br>
Here's and example stupid
~~~
S->aABb
S-> a
A-> c | E
B -> d|e

~~~
#### Following Sets
