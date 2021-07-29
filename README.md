# Cyber Camp 2.0 2021
The Cyber Camp 2.0 provides students with a thorough introduction to different types of malware and attacks.
Students will learn about different attack vectors and implement there own attack vectors along with how to mitigate them.
The target demographic are students in years 9-12 of schooling who have taken either a first year programming course or introductory Cyber Security Camp offered by the instructors.

## Goals
- Understanding of different types of attacks **list them out once the course is finished**
- Being able to think critically about software development from a security focus

## Day 1: Introduction
### Introduction
- Instructos: Dr. Chen Liu and Mr. Zander Blasingame
- Students:
  - Name
  - Year
  - School
  - Why did you choose this course
  - What is your anticipated field of study

### Linux Overview


### Additional Setup for Windows Users
- Install the free home edition of [MobaXterm](https://mobaxterm.mobatek.net/download.html)
- Create an SSH session

### Connect to the Server
- IP address is `3.12.96.179`
- Account usernames are given by `<fistletter><lastname>`
- The default password is `password`
- Use the ssh command to connect to the server
- Use the command `passwd` to change your password upon login

### Review of Essential Linux Commands
- What do the following commands do?
  - `cd`
  - `ls`
  - `pwd`
  - `mkdir`
  - `chmod`

### Review of VIM
- Vim (Vi IMproved) is a text editor for Linux and the preferred text editor for this course
- Two primary modes:
  - Escape: the mode where you type commands, which you enter via `<esc>` 
  - Insert: the mode where you type text, which you enter from escape mode via `i`
- List of very helpful commands. **All commands can only be executed from escape mode!**
  - `:wq` save and quit the file
  - `h`, `j`, `k`, and `l` navigate around the file: left, up, down, right
  - `o` insert line below
  - `O` insert line above
  - `dd` delete current line
  - `yy` to yank current line to vim buffer, i.e., copy
  - `p` to paste content in vim buffer
  - `^` navigate to beginning of line
  - `$` navigate to end of line
- For further information see this [tutorial](https://www.openvim.com/)

### Review Linux File Permissions
- Three different types of classes of users:
  - The file owner
  - Group members
  - Everyone else
- Each class has three permissions
  - The read permission (100 = 4)
  - The write permission (010 = 2)
  - The execute permission (001 = 1)
- What does `chmod 744 my_file` do to the permssions?


### Intro to Python Scripting
- Python is very powerful scripting language used for machine learning, scientific computer, web development, software engineering, data science, and many more fields
- Python has a simple syntax that is designed to be readable and expressive

#### First Python Script with the Fibonacci sequence
- Fibonacci sequence is a famous recurrence relationship that occurs often in nature
- The first two elements are `f[0] = 1` and `f[1] = 1`
- Then for any `n > 1` we have `f[n] = f[n-1] + f[n-2]`
- We can write a recurisive python function for this
```python

n = 10

# Fibonacci function
def fib(n):
    # conditionals in python, == checks for equality, = is for assignment
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)


print(fib(n))
```
- Save the script as `fib.py`
- To run the script enter `python3 fib.py`
- Currently, you have to manually edit the script to change values for `n`
- What if you could pass command line arguments?
- Use `argpase` see the example code below
```python
# use the import keyword to import additional libraries!
import argparse

parser = argparse.ArgumentParser()

# add argument and doc string to appear if -h or --help is entered
parser.add_argument(
    'n',
    type=int,
    help='n-th element of Fibonacci sequence to compute'
)

# grab arguments
args = parser.parse_args()


# Fibonacci function
def fib(n):
    # conditionals in python, == checks for equality, = is for assignment
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)


print(fib(args.n))
```


## Day 2: XSS
- Cross site scripting attack: [video](https://www.youtube.com/watch?v=zv0kZKC6GAM)
- Create a new directory for this project and navigate to it
- Download `public.tar.gz` from this github repo
- Use the `tar` command to extract the conents and use the `man` command to figure out what flags to use
- The contents of the `public.tar.gz` file contain all the HTML, CSS, and client-side JS for you website
- The main html file is located at `public/index.html`
- Install the express package with `npm install express`
- Create `app.js` in your project directory
- Add the following code to your `app.js` file
```js
// Import express package
var express = require('express');
var app = express();
// specify port number
var port = 8000;

// Handle request to serve main file
app.get('/', function(req, res) {
        // __dirname is the working directory of the project
        res.sendFile(__dirname + '/public/index.html');
});

// allow the server to serve content from public directory
app.use(express.static('public'));

// Set server to listen to `port`
app.listen(port, function() {
        console.log('Server on port: ' + port);
});
```
- Change the port number to one the instructors will give you
- Run the server in the background using the command `node app.js &`
- To see your webpage, open up your preferred web browser and navigate to `http://3.12.96.179:<your-port-number-here>`
- Ensure that you see your web page
- Kill the node server using `kill`



## Day 3: Encryption I (Introduction)
- Goal is to take plaintext, i.e. content, and obsfucate in a way (encryption) that only some one with the key can recover the plaintext (decryption)
- Watch this intro [video](https://www.youtube.com/watch?v=fNC3jCCGJ0o)
- 
### Caesar Cipher
- Idea is to shift each character `k` places, this is your key
- E.g., if `k = 3` and we have input string `the` then by shift each letter by 3 we have `wkh`
- To decrypt the shift is performed the other way
- Modulo arthimetic is used to wrap characters around, e.g., `x -> a` as (23 + 3) mod 26 = 0 (remember in computer science the first index is 0!)
- Here is an example of a Caesar Cipher
```
this message needs to be encrypted
```
And once encrypted we have
```
sghr ldrrzfd mddcr sn ad dmbqxosdc
```
- Here how to write this program
```python
import argparse
import string

# Parse input arguments
parser = argparse.ArgumentParser()
parser.add_argument(
    'file',
    type=str,
    help='File to encrypt or decrypt'
)
parser.add_argument(
    '-e', '--encrypt',
    action='store_true',
    help='Encrypt the file'
)
parser.add_argument(
    '-d', '--decrypt',
    action='store_true',
    help='Decrypt the file'
)

args = parser.parse_args()

# check if valid input
assert not (args.encrypt and args.decrypt), "Cannot both encrypt and decrypt!"
assert args.encrypt or args.decrypt, "No flags set!"

# determines the offeset for caesar cipher
key = 13

# All ascii printable characters
characters = string.printable
n_chars = len(characters)


def encrypt(in_text):
    out_text = ''
    for character in in_text:
        if character in characters:
            idx = characters.index(character)
            # Modulus addition, i.e., if we go past n_chars wrap back to 0
            new_idx = (idx + key) % n_chars
            new_character = characters[new_idx]
            out_text += new_character
        else:
            out_text += character

    return out_text


def decrypt(in_text):
    out_text = ''
    for character in in_text:
        if character in characters:
            idx = characters.index(character)
            # Modulus addition, i.e., if we go past n_chars wrap back to 0
            new_idx = (idx - key) % n_chars
            new_character = characters[new_idx]
            out_text += new_character
        else:
            out_text += character

    return out_text


# load input file
with open(args.file, 'r') as f:
    in_text = f.read()

# rewrite file
with open(args.file, 'w') as f:
    if args.encrypt:
        out_text = encrypt(in_text)
    if args.decrypt:
        out_text = decrypt(in_text)

    f.write(out_text)
```

### Frequency Analysis
- Letters occur with certain frequencies in English
- We can use these letter statistics to decode the encrypted file
- An example is found [here](https://www.youtube.com/watch?v=nikWSEjFCWg)


## Day 4: Encryption II (Public Key Cryptography)
- Intro [video](https://www.youtube.com/watch?v=AQDCe585Lnc)

### RSA Motivation and Intro

## Day 5: Ransomware

Work through this as a class [link](https://github.com/jimmy-ly00/Ransomware-PoC)
