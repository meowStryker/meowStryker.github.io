## PicoGym Write Up - Reverse Engineering

### 3. crackme-py

![image](https://user-images.githubusercontent.com/36885485/153524871-1dbc883d-1727-49c9-8e3b-4ec720a431e7.png)

We were given a Python source code for a program. 

#### The Flow of Program

This challenge provide us with the source code of a Python program that stored our flag which has been encrypted in a variable named "**bezos_cc_secret**". From the source code itself, we can know that this program uses ROT-47 to decode the encrypted value (Refer to comment block in **decode_secret()** function). The decoding algorithm of the program uses the set of 94 character representation as shown in variable **alphabet** consist mostly printable characters in ASCII format.

There are 2 modules of function exist in the program itself. The following are the description of these modules.
**choose_greatest**: Receive 2 numbers from user input and compare which one has greater value and print out the result.
**decode_secret()**: Convert each character from value passed to the function to a new value using set of alpabets specified with 47 as offset for the algorithm.  



There are several ways of solving this challenge.

#### a. Easy way

![image](https://user-images.githubusercontent.com/36885485/153526589-624b38cb-e612-4512-948c-8f448306376f.png)





#### b. Realistic way
