Ngày 28/2/2026
# Chapter 1: Introducing C
1. History of C

2. Strengths
- Efficiency: C is highly efficient and renowned for its speed. It was designed for systems where memory and resources were very limited.
- Portability: A C program written on one machine can easily be moved and compiled on different computers or operating systems with minimal to no changes.
- Power and Flexibility: It is a versatile language that can be used to write everything from low-level operating systems to complex, large-scale applications.
- Standard Library: C provides a robust collection of standard library functions to handle common tasks like input/output, string manipulation, and mathematics.

3. Weaknesses
- Error-prone: Because of its flexibility, C allows programmers to write code that can easily introduce bugs. The compiler does not catch many mistakes that stricter languages would flag.
- Difficult to understand: C syntax includes many shortcuts and complex combinations that can make programs hard to read and decipher for humans.
- Difficult to modify: If a large program is not well-structured and properly documented, maintaining and modifying C code can be extremely challenging.

4. Effective Use of C
- Avoid common bugs: Programmers must proactively learn to recognize and avoid the standard pitfalls and traps of the language.
- Utilize the standard library: Instead of writing code from scratch, it is highly recommended to use functions provided by the C Standard Library.
- Adopt a consistent style: Maintaining a clear and consistent coding style is critical to making C programs readable and maintainable over time.

# Chapter 2: C Fundamentals
2.1 Writing a Simple Program
- C programs require very little "boilerplate" setup code.
- Example program (pun.c):
    #include <stdio.h>
    int main(void)
    {
        printf("To C, or not to C: that is the question.\n");
        return 0;
    }
- Converting a C program into an executable involves three steps:
    + Preprocessing: The preprocessor obeys commands starting with # (directives) to modify the program.
    + Compiling: The compiler translates the code into machine instructions, also known as object code.
    + Linking: The linker combines your object code with library functions (like printf) to create the final executable.
- To compile and link a program using the GCC compiler, you can use the following command:
        % gcc -o pun pun.c

2.2 The General Form of a Simple Program
Simple C programs generally follow this exact template:
        directives
        int main(void)
        {
            statements
        }
- C relies heavily on abbreviations and special symbols, making the code concise.
- Programs rely on three key features: directives, functions, and statements.

Directives:
- These are commands intended for the preprocessor.
- Directives always begin with the # character.
- Example: #include <stdio.h> includes information about C's standard I/O library.
- Directives are normally one line long and do not end with a semicolon.

Functions:
- Functions are the basic building blocks of a C program.
- The main function is mandatory because program execution automatically begins there.
- The word int indicates that the main function returns an integer value.
- The word void in parentheses indicates that main has no arguments.
- The return 0; statement ends the program and returns a status code of 0 to the operating system.

Statements & Printing Strings:
- Statements are the actual commands executed when the program runs.
- Every statement in C must end with a semicolon (;).
- The printf function is used to display string literals (characters enclosed in double quotes).
- printf does not automatically move to the next output line when it finishes printing.
- You must include the new-line character (\n) inside the string to move the cursor to the next line.

2.3 Comments
- Used to add explanatory remarks to the code.
- Completely ignored by the computer.
- Single-line comments: Begin with //.
- Multi-line comments: Begin with /* and end with */.
- Can span across multiple lines easily.
- Warning: Forgetting to close with */ will cause the compiler to accidentally ignore part of your program.

2.4 Variables and Assignment
- Variables: Temporary storage locations for data during program execution.
- Types: Every variable must have a type that specifies what kind of data it will hold.
- int (integer): Stores whole numbers like 0, 1, or -2553.
- float (floating-point): Stores numbers with decimal points like 379.125.
- Declarations: You must declare a variable before it can be used or assigned a value.
- It is best to append the letter f to floating-point constants to avoid compiler warnings.
- Initialization: Assigning a starting value at the time the variable is declared.
- Printing Variables: Use printf with specific placeholders to display values.
- Use %d for int variables.
- Use %f for float variables.
- You can control decimal places (e.g., %.2f prints exactly two digits after the decimal point).
- Printing Expressions: printf can display the value of a calculated numeric expression directly, saving you from creating extra variables.
