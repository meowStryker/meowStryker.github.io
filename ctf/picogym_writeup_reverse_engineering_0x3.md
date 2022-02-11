## PicoGym Write Up - Reverse Engineering
### 0x3 crackme-py

![image](https://user-images.githubusercontent.com/36885485/153524871-1dbc883d-1727-49c9-8e3b-4ec720a431e7.png)

We were given a Python source code for a program. 

### Details of Program

This challenge provide us with the source code of a Python program that stored our flag which has been encrypted in a variable named "**bezos_cc_secret**". From the source code itself, we can know that this program uses ROT-47 to decode the encrypted value (Refer to comment block in **decode_secret()** function). The decoding algorithm of the program uses the set of 94 character representation as shown in variable **alphabet** consist mostly printable characters in ASCII format.  

There are 2 modules of function exist in the program itself:  
**choose_greatest()**: Receive 2 numbers from user input and compare which one has greater value and print out the result.  
**decode_secret()**: Convert each character from value passed to the function to a new value using set of alpabets specified with 47 as offset for the algorithm.  


![image](https://user-images.githubusercontent.com/36885485/153526589-624b38cb-e612-4512-948c-8f448306376f.png)

---

### The Flow of Program

The program simply will run **choose_greatest()** function when executed

![image](https://user-images.githubusercontent.com/36885485/153527935-c5395518-0c44-4926-8102-20f792b9b8ab.png)

---

### Solution
There are several ways of solving this challenge.


#### a. Easy way
Since we know that **bezos_cc_secret** variable stored the encrypted value of the flag that we want, we can just decrypt it using online ROT-47 decode tools to get the flag.
![image](https://user-images.githubusercontent.com/36885485/153528363-5801fc48-ad2f-4cf5-9ce4-0502c6e6f8ed.png)


**OR**

We can make changes to the source code of the python code and run it to get the flag.
![image](https://user-images.githubusercontent.com/36885485/153528483-f0b36f22-ee87-43de-b1ec-c7127961da14.png)



#### b. Realistic way
There's quote saying "_If you are not making mistakes, then you're not doing anything_". 
The reality is not as easy as what we thought including this challenge. Sometimes, easy example and challenges was provided to encourage beginners to learn reverse engineering which is clearly a very complex field in cybersecurity.

So what it looks like if the challenge was set to realistic mode?  
Consider at least these 2 condition in realistic reverse engineering.

##### 1. Reversing without source code!
In reverse engineering, it is normal for any reverser not having any access to the program source code when doing reverse engineering. Typically the sample or program that is accessible to them was simply a binary or non-source code file which looks different from the original source code. This situation similar to a **Black-Box** approach in penetration testing (sound similar right?) where the tester did not have any prior knowledge towards any of the code of the targeted system.

##### 2. Obfuscation or complex algorithm
In rare cases, the reverser might be provided with the source code of the program (except for open-source software). However, challenges that are often faced by the reverser is that the source code or sample program might comes with obfuscation or complex algorithm that are not feasible for finding quick solution.  

For example, the hardcoded encrypted password stored in **bezos_cc_secret** may undergone several anti-analysis technique to harden manual analysis from the reverser such as breaking the data into several parts, converting & encrypting each part, rearranging it, repeating and changing the order of those procedure for several many times. This definitely frustrated the reverser who analysed the sample!

##### Finding Vulnerability
Python 2.x known to have vulnerability in its **input()** function. It will received the data as what you have entered and passed it to the program without modifying it. Means the input we type in ASCII will not be necessarily being treated as string.[1] Based on the program, this vulnerability allow us to change the flow of the program execution and help us get the flag easily in a realistic way.


##### Exploiting input()
This vulnerability only exist in Python 2.x but not in the latest Python 3 version. You can run it directly or compile it into python binary but make sure to use version 2.x instead of version 3.

Run the following command to check your Python version
> python -V

If you want to compile the source code
> python -m compileall crackme.py


We can exploit the vulnerability by simple providing the program with **decode_secret(bezos_cc_secret)** to any of the input. Since the **decode_secret()** itself will print out the result of decoded text, it doesn't matter about the result produced by the caller module (**choose_greatest()**) module as long as we successfully invoked **decode_secret()** to serve it purposes.  
![image](https://user-images.githubusercontent.com/36885485/153538254-b81e56d0-a04b-4b78-9e19-99fad2152065.png)

![image](https://user-images.githubusercontent.com/36885485/153538181-0d3a89f5-419b-4a0b-bac3-106ac2914bc3.png)

![image](https://user-images.githubusercontent.com/36885485/153537698-df3a116b-3702-4042-b273-f4aef946ed6d.png)

The result is...  
![image](https://user-images.githubusercontent.com/36885485/153538322-ba865648-b96e-44b6-b4a2-e998a7c23b6a.png)  
You also can repeat it by doing the same to the second input.  
![image](https://user-images.githubusercontent.com/36885485/153538424-ecfde9a6-fa30-4fe4-b21e-3af551fbada9.png)

  ---
### Reference
[[1] - Vulnerability in input() function – Python 2.x](https://www.geeksforgeeks.org/vulnerability-input-function-python-2-x/)
