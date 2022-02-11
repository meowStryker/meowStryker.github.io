## PicoGym Write Up - Reverse Engineering

### 3. crackme-py

![image](https://user-images.githubusercontent.com/36885485/153524871-1dbc883d-1727-49c9-8e3b-4ec720a431e7.png)

We were given a Python source code for a program. 

#### Details of Program

This challenge provide us with the source code of a Python program that stored our flag which has been encrypted in a variable named "**bezos_cc_secret**". From the source code itself, we can know that this program uses ROT-47 to decode the encrypted value (Refer to comment block in **decode_secret()** function). The decoding algorithm of the program uses the set of 94 character representation as shown in variable **alphabet** consist mostly printable characters in ASCII format.

There are 2 modules of function exist in the program itself. The following are the description of these modules.


**choose_greatest**: Receive 2 numbers from user input and compare which one has greater value and print out the result.

**decode_secret()**: Convert each character from value passed to the function to a new value using set of alpabets specified with 47 as offset for the algorithm.  


![image](https://user-images.githubusercontent.com/36885485/153526589-624b38cb-e612-4512-948c-8f448306376f.png)

#### The Flow of Program

The program simply will run **choose_greatest()** function when executed

![image](https://user-images.githubusercontent.com/36885485/153527935-c5395518-0c44-4926-8102-20f792b9b8ab.png)

#### Solution
There are several ways of solving this challenge.


##### a. Easy way
Since we know that **bezos_cc_secret** variable stored the encrypted value of the flag that we want, we can just decrypt it using online ROT-47 decode tools to get the flag.
![image](https://user-images.githubusercontent.com/36885485/153528363-5801fc48-ad2f-4cf5-9ce4-0502c6e6f8ed.png)


**OR**

We can make changes to the source code of the python code and run it to get the flag.
![image](https://user-images.githubusercontent.com/36885485/153528483-f0b36f22-ee87-43de-b1ec-c7127961da14.png)



##### b. Realistic way
There's quote saying "_If you are not making mistakes, then you're not doing anything_". 
The reality is not as easy as what we thought including this challenge. Sometimes, easy example and challenges was provided to encourage beginners to learn reverse engineering which is clearly a very complex field in cybersecurity.

So what it looks like if the challenge was set to realistic mode?

1. Reversing without source code!
2. Source code with Obfuscation

