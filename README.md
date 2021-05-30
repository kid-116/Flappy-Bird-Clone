### Flappy-Bird-Clone
https://editor.p5js.org/mehultodi116/present/925FR8Xn5
---
 layout: post
 title: "FlappyBirdClone"
 description: "A clone of the famous game of Flappy Bird using p5.js."
---

### Mentors
- Aditya Chirania 
- Niranjan S Yadiyala
- Praveen Reddy

### Members
- Chaithanya Shyam
- Janmansh Agarwal

# Introduction
CodePal is a Visual Studio Code Extension to help **Codeforces** Users ***Code with Convenience.*** This extension is specially for people who want to save time in a live Codeforces contest and up-solve problems comfortably. This extension responds quickly to users. It can swiftly filter through the problem list by specifying tags and ratings, create folders for contests and problems containing sample tests of each problem in them and compile and run tests automatically. For added convenience we've created buttons to directly open problem statements and submission pages on the default browser. 

# Brief Overview of Technologies Used

### NodeJS

- Node.js is an open-source, cross-platform, back-end, [JavaScript](https://en.wikipedia.org/wiki/JavaScript) runtime environment that runs on the [V8 engine](https://en.wikipedia.org/wiki/V8_(JavaScript_engine)) and executes JavaScript code outside a [web browser](https://en.wikipedia.org/wiki/Web_browser). Node.js lets us use JavaScript to write command line tools and for [server-side scripting](https://en.wikipedia.org/wiki/Server-side_scripting)—running scripts server-side to produce [dynamic web page](https://en.wikipedia.org/wiki/Dynamic_web_page) content before the page is sent to the user's web browser. In our case to the VSCode editor.
- It is only with NodeJS we can perform API calls to Codeforces to get all the features of our extensions. We also use NodeJS to web scrape data from the internet.

### Codeforces API

- The [Codeforces API](https://codeforces.com/apiHelp) is extremely useful and the bulk of our features perform API calls to Codeforces to get useful information. 
- But it does not provide everything. Our extension uses a small amount of web scraping of the Codeforces website to get the data the API does not provide for example test cases for a problem. 
- The documentation of the Codeforces API is small but clear.

### TypeScript

- [TypeScript](https://www.typescriptlang.org) is a typed superset of JavaScript that compiles to plain JavaScript. It offers classes, modules, and interfaces to help you build robust  components. 
- TypeScript code is transformed into JavaScript code via the TypeScript compiler or [Babel](https://babeljs.io/). This JavaScript is clean, simple code which runs anywhere JavaScript runs: In a browser, on Node.JS or in  apps. In our case the final JavaScript file will run on Node.JS.
- Since  there is compilation, TypeScript makes it much easier to debug your code compared to JavaScript. 

### VS Code API

- This is the main reason why VS Code is popular. The [VS Code API](https://code.visualstudio.com/api) has a plethora of useful features that not only give us a lot of control over the environment but also make it quite easy for us to understand and code.
- The organised tree-view, context menus, key-bindings, input boxes etc. are some of the API features that have been used in this extension.
- It cannot be stressed enough how important and useful VS Code API TypeScript documentation is. It is concise, clean and extremely informative. It was heavily referred in this project.


# Demo 
### Enter Handle in settings and view problems, contests and profile, with submission status on each problem
![Promo1](https://github.com/IEEE-NITK/CodePal/raw/master/res/GIFS/Demo0.gif)


### Create Problem/Contest Folders inclusive of all sample tests, compile and run automatically, get comprehensive verdicts and view problem statement and submission webpages. 
![Promo2](https://github.com/IEEE-NITK/CodePal/raw/master/res/GIFS/Demo1.gif)


### Filter Through Problems
![Promo2](https://github.com/IEEE-NITK/CodePal/raw/master/res/GIFS/Demo2.gif)


### Stress Testing your Code
![Promo2](https://github.com/IEEE-NITK/CodePal/raw/master/res/GIFS/Demo3.gif)


# Features of the Extension and Their Implementation Details

### <u>View Complete Problem Set List along with their associated tags and ratings.</u> 

The list of problems along with their details are available through the Codeforces API. A problem class is maintained which is initialised by fetching details of all problems from this [API end point](https://codeforces.com/apiHelp/methods#problemset.problems). The fetched list of problems is displayed in a tree view in the activity bar on the left hand side of the window.

### <u>Swiftly Filter through the ProblemSet by specifying Ratings, Tags and Submission Status.</u> 

The list of problems that are rendered in the tree view are filtered internally based on the problem tags, problem rating range and submission status and re-rendered on the screen. The input for the filters is taken through the VS Code API. 

### <u>View Currently Running, Upcoming and all Past Contests and also view the precise start date, time and duration of upcoming contests.</u> 

The list of contests along with their details are available through the Codeforces API. A contest class is maintained by us which is initialised by fetching details of all contests from this [API end point](https://codeforces.com/apiHelp/methods#contest.list). The list of contests is also diplayed in a tree view on the activity bar on the left hand side of the window.

### <u>Fast Folder Creation on a single click for Contests from the Codeforces contest list and Problems from the Codeforces problem list.</u> 

When a user wishes to create a folder for a problem on Codeforces, a folder with the name of the problem is created and test cases of the particular problem are web scraped from the problem's web page and are included in the same folder.  

When a user wishes to create a contest folder, a folder with the name of the contest is created, and a folder for each problem in the contest is iteratively created. 


### <u>Folder for a problem consists of all its sample test cases and a program file loaded with a template whose path may be specified in the settings.</u>

What is also included in a problem folder is a file named as "Problem_name.{extension}" which loads any starter template that you like if you specify the template path in the settings of the extension. 

### <u>Add additional tests to any problem.</u>

2 Additional files called "input_{test_number}.txt" and "output_{test_number}.txt" are created where the user may enter the input he wants and the expected output respectively.

### <u>Compile and run any program file against the testcases and get comprehensive results.</u>

The *child_process* library is used to run compilation and running commands on the host's system. We make sure to terminate a running program process in case it exceeds 6 seconds of execution. We measure the time by using races of promises in JavaScript. We set a timeout of 6 seconds and also run the program and whichever promise accepts or rejects first ends the wait and then the running program is terminated. This is very crucial to indicate "Time Limit Exceeded" to users.

The code is run on each of the sample test cases that are already present in the problem folder, and for each test a file is created having the output produced by the code, which is compared with the expected output file to give the verdict. A file called "result.txt" is created, having all details of each of the tests, including the input, expected output, obtained output and standard error, along with the final verdict on the test. In case of a compilation error or a run-time error, the error is logged into a file called "error.txt".

### <u>Open problem statement or submission page on your default browser, with a single click in VS Code.</u>

A file is saved called ".problem.json" in the same directory as the problem file. This json file contains the problem index and the ID of the contest it was part of, details sufficient to construct the problem URL. So when a user clicks the "open problem statement" button, we construct the URL with the help of the json file and open it your default browser. Similarly, the problem submission page can also be opened. 

### <u>Compiler may be selected and compilation flags can be set through the codepal settings.</u>

Commands are run to compile and run the programs written by the user on his/her own system. They are compiled and run based on the language that has been selected from the CodePal settings. We also allow for additional compilation flags that a user may want to add. We add these flags to the command for compiling.

Example: "-std=c++14"

### <u>Perform Stress Testing to find a counter test case for your code.</u>

We create 2 more files for the user
- A generator  file: This file is supposed to be filled by the user to generate random test cases in the input format specified by the problem.
- A program file: A user may write the correct solution or a brute force solution that he may want to compare his own solution with. 

Now, we run the generator file and create test cases, and iteratively run both, the users code and the program file that he entered, against the generated test cases as many times as the user wants. He may specify the number of times he wants this to continue in the settings. This shall either stop as soon output given by the users code and the program file differ. 

### <u>Get a personalized experience of viewing details of your Codeforces profile and the status of problem submissions made, by entering your Codeforces handle in the codepal settings.</u>

Given the handle of a user, the [codeforces API](https://codeforces.com/apiHelp/methods#user.info) can return basic information about the user. We present that in the user profile section. The [codeforces API](https://codeforces.com/apiHelp/methods#user.status) also returns the set of all submissions made by a given user. We sort submissions as per problem id, and then run a pointer each across the user's submissions and problems (which are already sorted as per problem id) and determine the submission status of a problem (Passed, Failed, Unattempted).

# Languages Supported

- C++ (compiler : g++)
- C (compiler : gcc)
- Java (compiler : javac)
- Python 
- Note : You may add additional compilation flags through the codepal settings (example : -std=c++14) and also choose between python, python2 or python3 depending upon the python command you use on your system to run.

# Operating Systems Supported

- Windows 
- Linux
- MAC

# Usage Guide

<a id="labels"></a>
Below are the various clickable icons used in our extension. 

<img src="https://github.com/IEEE-NITK/CodePal/raw/master/res/svg/IconLabels.png"/>

## Key to Icons:
1. Reload Contest List
2. Future Contest Registration on Codeforces
3. Contest Folder Creation
4. Reload Problem List  
5. Problem Filter Based on Rating and Tags
6. Problem Folder Creation
7. Add Manual Test Cases
8. Create Stress Testing Files
9. Open Problem Statement on Codeforces
10. Run Test Cases on Solution File
11. Perform Stress Testing 
12. Open Problem Submission Page on Codeforces

## Keyboard Shortcuts
- *Ctrl + Alt + A* (*Cmd + Alt + A* for Mac) : Add Manual Test Cases
- *Ctrl + Alt + R* (*Cmd + Alt + R* for Mac): Run Test Cases on Solution File
- *Ctrl + Alt + O* (*Cmd + Alt + O* for Mac) : Open Problem Statement on Codeforces
- *Ctrl + Alt + S* (*Cmd + Alt + S* for Mac) : Open Problem Submission Page on Codeforces
- *Ctrl + Alt + Z* (*Cmd + Alt + Z* for Mac) : Start Stress Testing 
- *Ctrl + Shift + Z* (*Cmd + Shift + Z* for Mac) : Force Stop Stress Testing 

## Usage Guide to Various Features

### 1. Filtering Problems
- Click on icon **5** (shown [here](#labels)) to filter the problem set.
- Add the lower bound for problem’s rating. (Default lower bound is 0)
- Add the upper bound for problem’s rating. (Default upper bound is 4000)
- Tick the tags you want for the problems. (If no tags are selected, no tag-based filtering is done)
- Tick the submission status of the problems you want to view. 

### 2. Creating Problem Folder
- Click on icon **6** (shown [here](#labels)) beside the problem name to create and open a problem folder containing the solution file and test cases. (Make sure you have opened a folder on VS Code where you want the problem folder)

### 3. Creating Contest Folder
- Click on the type of contest you want to participate in. (Past, Running or Future)
- Precise timings and durations of upcoming contests are also shown.
- Click on icon **3** (shown [here](#labels)) beside the contest name to create and open a contest folder containing the problems folders of each problem of the contest. (Make sure you have opened a folder on VS Code where you want the contest folder)

### 4. Adding Your Own Code Template
- Press *Ctrl + comma(,)* or go to settings of VS Code (icon on bottom left).
- Go to extensions and select codepal, and add your template file path to the given space

### 5. Changing Compilation Language
- Press *Ctrl + comma(,)* or go to settings of VS Code (icon on bottom left).
- Go to extensions and select codepal, and choose the compiler you want from the drop down list.

### 6. Adding Test Cases
- Click on icon **7** (shown [here](#labels)) on the top right side of the editor window, or press *Ctrl + Alt + A*, to add manual test cases for the problem.

### 7. Running Test Cases
- Click on icon **10** (shown [here](#labels)) on the top right side of the editor window, or press *Ctrl + Alt + R*, to run the code for all sample and manual test cases.

### 8. Viewing Problem
- Click on icon **9** (shown [here](#labels)) on the top right side of the editor window, or press *Ctrl + Alt + O*, to open the problem statement on Codeforces in your default browser.

### 9. Submitting Problem
- Click on icon **12** (shown [here](#labels)) on the top right side of the editor window, or press *Ctrl + Alt + S*, to open the submission page of the problem on Codeforces in your default browser.

### 10. Stress Testing
- Click on icon **8** (shown [here](#labels)) on the top right side of the editor window to create the stress testing files called "brute" and "gen". Here "brute" is the code that is a bruteforce solution or any code that gives the correct output and "gen" is the generator file that makes testcases. You need to code both of them. 
- Command line arguments of integers (1,2,3...) are passed to the generator file so as to keep a fixed random seed each time you stress test. A template is initially provided that takes care of this.
- Once all 3 files compile properly (solution, "brute" and "gen" files) click on icon **11** (shown [here](#labels)), or press *Ctrl + Alt + Z*, to stress test. It will run the "gen" file to create input and compare it against your solution and "brute". If they differ then it is reported else it moves to the next case. By defualt it will run for a number of 100 test cases but this can be changed in the settings.
- Stress testing can be forced to stop on pressing *Ctrl + Shift + Z*.

### 11. User Profile
- Press *Ctrl + comma(,)* or go to settings of VS Code (icon on bottom left).
- Go to extensions and select codepal, and add your Codeforces handle to get a personalized experience of the extension.
- On entering your handle, you will be able to view your profile details in the *User Profile* section of the side bar. 
- This will also enable you to view the status of problem submissions made on Codeforces, through ticks or crosses against the problem names, denoting whether the submission was accepted or failed.   


# Conclusion

Building a VS Code extension is a great way to understand how such a popular code editor like VS Code works. 

- You can install our extension [here](https://marketplace.visualstudio.com/items?itemName=IEEE-NITK.codepal)
- Please do try it and give your feedback. If you face issues or bugs, please file an issue [here](https://github.com/IEEE-NITK/CodePal/issues)
- To know more about CodePal and its code base, you may visit its public repository [here](https://github.com/IEEE-NITK/CodePal)
