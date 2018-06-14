# Changes in Learnbot-DSL

July 19, 2017


The new changes in the  Learnbot-DSL are:

    While loop
    Temporal Loop
    Turn 90 degree functions
    Exitloop command

## While loop
```
while <condition>
statement1
statement 2
...
end
```

The set of statements run as long as the given condition is true.

## Temporal Loop
```
repeat <n> seconds
statement 1
statement 2
...
end
```
The set of statements run continuously for n seconds.

##Turn 90 functionalities

Added two new functions for turning right and left by 90 degrees, the earlier turn_right and turn_left functions are modified to a generic turn function for better performance and adaptability with the newly written visual functions like line follower.

## Exitloop command
`exitloop`
This command works similar to a break command, but because we want to keep the commands simple, it has been named exitloop, so that it is understood to used for exiting loops if required.

* * *
Aniq Ur Rahman