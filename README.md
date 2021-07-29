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


## Day 2: Cross Site Scriping (XSS) Attack
### HTML, CSS, Javascript, and Node.js Review
- What does HTML do?
- What does CSS do?
- What does Javascript do?
- What is Node.js?

### Build Basic Bootstrap Webpage with Node.js
- Bootstrap is a tool to "quickly design and customize responsive mobile-first sites with Bootstrap, the world’s most popular front-end open source toolkit, featuring Sass variables and mixins, responsive grid system, extensive prebuilt components, and powerful JavaScript plugins."
- Create a new directory for this project and navigate to it
- First initialize the project with node running `npm init`, note when prompted for entry point enter `app.js` NOT `index.js`
- Install the express package via `npm install express`
#### Setting up the app.js file
- Create `app.js` in your project directory with the following code
```javascript
const express = require('express');
const app = express();
const http = require('http');
const server = http.createServer(app);
const port = 8000;

// serve request files
app.get('/', (req, res) => {
	res.sendFile(__dirname + '/public/views/index.html');
});

// set server to listen to specific port
server.listen(port, () => {
	console.log('listening on port: ' + port);
});
```
- Change the port number to one the instructors will give you
#### Creating the HTML page
- Now create the `public` and `public/views` sub-directories
- Create our main HTML file `public/views/index.html`, this will be what you see when you connect to the webpage use this Boostrap based template:
```html
<!DOCTYPE html>
<html>
	<head>
		<title>Basic Webpage</title>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device=width, initial-scale=1, shrink-to-fit=no">
		<link rel="stylesheet" href="https://cdn.tutorialjinni.com/twitter-bootstrap/4.4.1/css/bootstrap.min.css">
		<script src="https://cdn.tutorialjinni.com/jquery/3.4.1/jquery.min.js"></script>
		<script src="https://cdn.tutorialjinni.com/twitter-bootstrap/4.4.1/js/bootstrap.bundle.min.js"></script>
	</head>

	<body>
		<div class="container">
			<div class="jumbotron">
				<h1> This is a webpage! </h1>
			</div>
		</div>
	</body>
</html>
```
- Modify the template to include your name in the `<h1>` tag
#### Running and Connecting to the Server
- Run the server in the background using the command `node app.js &`
- To see your webpage, open up your preferred web browser and navigate to `http://3.12.96.179:<your-port-number-here>`
- Ensure that you see your web page
- Kill the node server using the `kill` command

### Intro to the XSS Attack
- Cross site scripting attack: [video](https://www.youtube.com/watch?v=zv0kZKC6GAM)

### Building a Chat Room Using Node.js and Socket.io
<!-- - Download `public.tar.gz` from  -->
- Run `npm install socket.io` to instal the socket.io plugin
- Modify `index.html` so it contains a login portal and chat area by ading the following code
```html
<!-- Enter username on login -->
<div class="modal fade" id="username_modal" tabindex="-1" role="dialog">
	<div class="modal-dialog modal-lg" role="document">
		<div class="modal-content">
			<div class="modal-header">
				<h4 class="modal-title">User Login</h4>
			</div>
			<div class="modal-body">
				<!-- enter username  -->
				<form onsubmit="return false;">
					<div class="form-group">
						<input type="text" class="form-control" id="username_field" placeholder="Enter username">
					</div>
				</form>
			</div>
			<div class="modal-footer">
				<button id="close_modal" type="button" class="btn btn-primary">Enter Chat</button>
			</div>
		</div>
	</div>
</div>

<!-- chat area -->
<div class="container-fluid">
	<div class="row">
		<div class="col-12">
			<ul id="messages"></ul>
		</div>
	</div>
	<div class="row bottom">
		<div class="col-12">
			<form role="form" onsubmit="return false;">
				<div class="input-group">
					<input id="message_field" class="form-control" type="text" placeholder="Enter message">
					<div class="input-group-btn">
							<button id="message_submit" type="button" class="btn btn-primary">Send</button>
					</div>
				</div>
			</form>
		</div>
	</div>
</div>
```
- Also include our stylesheet and client side javascript (to be created) with the following to lines in the `<head>` tag in addition to including socket.io
```html
<!-- Include socket.io -->
<script src="/socket.io/socket.io.js"></script>

<!-- Include our custom stylesheet and script -->
<link rel="stylesheet" href="/public/styles/style.css">
<script type="text/javascript" src="/public/scripts/client.js"></script>
```
- Add the following stylesheet to `public/styles/style.css`
```css
#messages {
	list-style-type: none;
	padding: 0;
	margin: 0;
	white-space: nowrap;
	font-size: 125%;
}

#messages li {
	font-family: sans-serif;
}

/* Make each entry alternate color for easier reading */
#messages li:nth-child(odd) {
	background: rgb(245, 245, 245);
}

#messages: li:nth-child(even) {
	background: rgb(200, 200, 200);
}

.bottom {
	position: fixed;
	bottom: 0;
	width: 100%;
}
```
- Create the following `public/scripts/client.js` file
```js
$(document).ready(() => {
	const socket = io();
	var username = null;
	var in_modal = true;

	// get login info from modal
	$(window).load(() => {
		$('#username_modal').modal('show');
	});

	// once username is submitted close modal
	$('#close_modal').on('click', () => {
		get_username();
		enter_chat_room();
	});

	function get_username() {
		username = $('#username_field').val() !== '' ? $('#username_field').val() : 'anonymous';
		$('#username_field').val('');
	}

	function enter_chat_room() {
		if (username !== null) {
			$('#username_modal').modal('hide');
			in_modal = false;
			socket.emit('init', username);
		}
	}

	// listen for enter keypress (13)
	$(document).keypress((event) => {
		if (event.which === 13) {
			if (in_modal) {
				get_username();
				enter_chat_room();
			} else {
				send_message();
			}
		}
	});

	// listen for send button press
	$('#message_submit').on('click', () => {
		send_message();
	});

	function send_message() {
		var msg = $('#message_field').val();

		if (msg !== '') {
			$('#message_field').val('');

			socket.emit('msg', {
				username: username,
				message: msg,
				admin: false,
			});
		}
	}

	function add_message(data) {
		var $li = data.admin ? $('<li>').append(data.message) : $('<li>').append(data.username + ': ' + data.message);

		$('#messages').append($li);
	}

	// listen for broadcasts
	socket.on('msg', (data) => {
		add_message(data);
	});

});
```
- Lastly modify app.js so it contains socket.io handling
```js
// setup socket handling
io.on('connection', (socket) => {
	// get connected users data
	socket.on('init', (data) => {
		socket.username = data;
		console.log(data + ' has joined the chat')
	});

	// send users messages
	socket.on('msg', (data) => {
		console.log(data.username + ': ' + data.message);
		io.sockets.emit('msg', data);
	});

	socket.on('disconnect', () => {
		if (socket.users !== undefined) {
			console.log(socket.username + ' has disconnected.');
			io.sockets.emit('msg', {
				username: null,
				message: socket.username + ' has disconnected.',
				admin: true
			});
		}
	});
});
```
- Add routing code to serve up the CSS and JS files you created
```js
app.get('/public/scripts/client.js', (req, res) => {
	res.sendFile(__dirname + '/public/scripts/client.js');
});

app.get('/public/styles/style.css', (req, res) => {
	res.sendFile(__dirname + '/public/styles/style.css');
});
```

### Demo XSS Attack
- Note we can execute arbitrary code on every client's instance of the chatroom

### Mitigating the XSS Attack
- We need to sanitize incoming messages to make sure we don't just execute arbitrary code
- Replace special symbols with escaped ones

| Escape Character | Symbol |
|:------|:----------------|
|\&amp;| & |
|\&lt;| < |
|\&gt;|>|

- To do this in `app.js` create a function called `sanitize_input` like so
```js
function sanitize_input(content) {
	// add code to clean content here
	return content;
}
```
- And in the socket handling section before `io.osockes.emit('msg', data);` add the line
```js
data.message = sanitize_input(data.message);
```
- To santize code use string replace, e.g., `content = content.replace(/stuff-to-replace/g, 'replaced-string');`

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

### Introduction to Prime Numbers
- What is a prime number?
- 


## Day 4: Encryption II (Public Key Cryptography)
- Intro [video](https://www.youtube.com/watch?v=AQDCe585Lnc)

### RSA Motivation and Intro

## Day 5: Ransomware

Work through this as a class [link](https://github.com/jimmy-ly00/Ransomware-PoC)
