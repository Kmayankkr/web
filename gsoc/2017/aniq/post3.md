# Getting rid of redundancy

June 17, 2017

I had completed the task of using movement functions of the Learnbot in python and my PR was merged to the master but due to change of plans, the work had to be restarted. My mentors were of much help to me and replied to all my doubts within hours, which is pretty quick, keeping in mind that our timezones differ.

[This](https://github.com/robocomp/learnbot/tree/master/learnbot-dsl) is the new structure for the **learnbot-dsl**

[LearnBotClient.py](https://github.com/robocomp/learnbot/blob/master/learnbot-dsl/LearnBotClient.py) is the intermediate class which allows for the communication between the python code and the hardware components.
Earlier, we were aiming to takle the DSL problem through the robocompdsl but my mentor, Dr. Pilar pointed out that many unnecessary files were generated in that case and that approach isn’t fast enough.
So, instead of having one file for all the functions, we have a [functions](https://github.com/robocomp/learnbot/tree/master/learnbot-dsl/functions) folder which has all the function files as `<function_name>.py`

Through the intermediate class, we are getting rid of the redundancies and the entire learnbot-dsl structure now occupies lesser memory space.

I have demonstrated the working of the structure through the python script: [code_test.py](https://github.com/robocomp/learnbot/blob/master/learnbot-dsl/code_test.py)

The task at hand is generating the code_test.py type of script from a simple program. This is achieved by the help of translator. The **translator** code is almost ready, I just need to test it more, before opening a PR and then what remains is automating the **build** and **run** of the “simple code”, which won’t even take 10 minutes.

Saturday Greetings! 🙂

* * *
Aniq Ur Rahman