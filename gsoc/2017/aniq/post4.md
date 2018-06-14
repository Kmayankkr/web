# Designing the Syntax for DSL

June 23, 2017

As notified in the earlier posts, I had started working on the DSL Translator.
I discussed this with my mentor and came up with the [syntax for DSL](https://gist.github.com/Aniq55/002a92f1a3c85b2078b4606c082cf71b).

The DSL bears close resemblance with python, except for the fact that indentation isn’t mandatory here. Instead I have used the end keyword, to demarcate the scope of a code block. This is because we intend to keep the code simple and easy to write. writing end explicitly, rules out the possibility of errors because of wrong indentation of a few statements

The syntax is designed for:

    print statement
    variable declaration
    conditional statements
    mathematical operations
    loops
    input
    learnbot-specific statements

The DSL is currently, only being developed for the event-based programs.
I will expand the DSL for the state-based approach after this is merged.

After the DSL is finalized for event and state-based approach, I’ll begin coding the Error-reporter, which reports the appropriate syntactical error and suggests the possible debugging solution.

Thanks for reading and have a blessed Friday! 🙂

* * *
Aniq Ur Rahman