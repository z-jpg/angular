// server.mjs
import { createServer } from 'node:http';

const server = createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello World!\n');
});

// starts a simple http server locally on port 3000
server.listen(3000, '127.0.0.1', () => {
  console.log('Listening on 127.0.0.1:3000');
});

// run with `node server.mjs`
# Docker has specific installation instructions for each operating system.
# Please refer to the official documentation at https://docker.com/get-started/

# Pull the Node.js Docker image:
docker pull node:22-alpine

# Create a Node.js container and start a Shell session:
docker run -it --rm --entrypoint sh node:22-alpine

# Verify the Node.js version:
node -v # Should print "v22.18.0".

# Verify npm version:
npm -v # Should print "10.9.3".
#!/usr/bin/env bash

# This is used by the Node.js installer, which expects the cygwin/mingw
# shell script to already be present in the npm dependency folder.

(set -o igncr) 2>/dev/null && set -o igncr; # cygwin encoding fix

basedir=`dirname "$0"`

case `uname` in
  *CYGWIN*) basedir=`cygpath -w "$basedir"`;;
esac

if [ `uname` = 'Linux' ] && type wslpath &>/dev/null ; then
  IS_WSL="true"
fi

function no_node_dir {
  # if this didn't work, then everything else below will fail
  echo "Could not determine Node.js install directory" >&2
  exit 1
}

NODE_EXE="$basedir/node.exe"
if ! [ -x "$NODE_EXE" ]; then
  NODE_EXE="$basedir/node"
fi
if ! [ -x "$NODE_EXE" ]; then
  NODE_EXE=node
fi

# this path is passed to node.exe, so it needs to match whatever
# kind of paths Node.js thinks it's using, typically win32 paths.
CLI_BASEDIR="$("$NODE_EXE" -p 'require("path").dirname(process.execPath)' 2> /dev/null)"
if [ $? -ne 0 ]; then
  # this fails under WSL 1 so add an additional message. we also suppress stderr above
  # because the actual error raised is not helpful. in WSL 1 node.exe cannot handle
  # output redirection properly. See https://github.com/microsoft/WSL/issues/2370
  if [ "$IS_WSL" == "true" ]; then
    echo "WSL 1 is not supported. Please upgrade to WSL 2 or above." >&2
  fi
  no_node_dir
fi
NPM_PREFIX_JS="$CLI_BASEDIR/node_modules/npm/bin/npm-prefix.js"
NPX_CLI_JS="$CLI_BASEDIR/node_modules/npm/bin/npx-cli.js"
NPM_PREFIX=`"$NODE_EXE" "$NPM_PREFIX_JS"`
if [ $? -ne 0 ]; then
  no_node_dir
fi
NPM_PREFIX_NPX_CLI_JS="$NPM_PREFIX/node_modules/npm/bin/npx-cli.js"

# a path that will fail -f test on any posix bash
NPX_WSL_PATH="/.."

# WSL can run Windows binaries, so we have to give it the win32 path
# however, WSL bash tests against posix paths, so we need to construct that
# to know if npm is installed globally.
if [ "$IS_WSL" == "true" ]; then
  NPX_WSL_PATH=`wslpath "$NPM_PREFIX_NPX_CLI_JS"`
fi
if [ -f "$NPM_PREFIX_NPX_CLI_JS" ] || [ -f "$NPX_WSL_PATH" ]; then
  NPX_CLI_JS="$NPM_PREFIX_NPX_CLI_JS"
fi

"$NODE_EXE" "$NPX_CLI_JS" "$@"
#!/usr/bin/env bash

# This is used by the Node.js installer, which expects the cygwin/mingw
# shell script to already be present in the npm dependency folder.

(set -o igncr) 2>/dev/null && set -o igncr; # cygwin encoding fix

basedir=`dirname "$0"`

case `uname` in
  *CYGWIN*) basedir=`cygpath -w "$basedir"`;;
esac

if [ `uname` = 'Linux' ] && type wslpath &>/dev/null ; then
  IS_WSL="true"
fi

function no_node_dir {
  # if this didn't work, then everything else below will fail
  echo "Could not determine Node.js install directory" >&2
  exit 1
}

NODE_EXE="$basedir/node.exe"
if ! [ -x "$NODE_EXE" ]; then
  NODE_EXE="$basedir/node"
fi
if ! [ -x "$NODE_EXE" ]; then
  NODE_EXE=node
fi

# this path is passed to node.exe, so it needs to match whatever
# kind of paths Node.js thinks it's using, typically win32 paths.
CLI_BASEDIR="$("$NODE_EXE" -p 'require("path").dirname(process.execPath)' 2> /dev/null)"
if [ $? -ne 0 ]; then
  # this fails under WSL 1 so add an additional message. we also suppress stderr above
  # because the actual error raised is not helpful. in WSL 1 node.exe cannot handle
  # output redirection properly. See https://github.com/microsoft/WSL/issues/2370
  if [ "$IS_WSL" == "true" ]; then
    echo "WSL 1 is not supported. Please upgrade to WSL 2 or above." >&2
  fi
  no_node_dir
fi
NPM_PREFIX_JS="$CLI_BASEDIR/node_modules/npm/bin/npm-prefix.js"
NPX_CLI_JS="$CLI_BASEDIR/node_modules/npm/bin/npx-cli.js"
NPM_PREFIX=`"$NODE_EXE" "$NPM_PREFIX_JS"`
if [ $? -ne 0 ]; then
  no_node_dir
fi
NPM_PREFIX_NPX_CLI_JS="$NPM_PREFIX/node_modules/npm/bin/npx-cli.js"

# a path that will fail -f test on any posix bash
NPX_WSL_PATH="/.."

# WSL can run Windows binaries, so we have to give it the win32 path
# however, WSL bash tests against posix paths, so we need to construct that
# to know if npm is installed globally.
if [ "$IS_WSL" == "true" ]; then
  NPX_WSL_PATH=`wslpath "$NPM_PREFIX_NPX_CLI_JS"`
fi
if [ -f "$NPM_PREFIX_NPX_CLI_JS" ] || [ -f "$NPX_WSL_PATH" ]; then
  NPX_CLI_JS="$NPM_PREFIX_NPX_CLI_JS"
fi

"$NODE_EXE" "$NPX_CLI_JS" "$@"


# a path that will fail -f test on any posix bash
NPX_WSL_PATH="/.."

# WSL can run Windows binaries, so we have to give it the win32 path
# however, WSL bash tests against posix paths, so we need to construct that
# to know if npm is installed globally.
if [ "$IS_WSL" == "true" ]; then
  NPX_WSL_PATH=`wslpath "$NPM_PREFIX_NPX_CLI_JS"`
fi
if [ -f "$NPM_PREFIX_NPX_CLI_JS" ] || [ -f "$NPX_WSL_PATH" ]; then
  NPX_CLI_JS="$NPM_PREFIX_NPX_CLI_JS"
fi

"$NODE_EXE" "$NPX_CLI_JS" "$@"


Skip to content
geeksforgeeks
Search...


Git Tutorial
Git Exercises
Git Basic Commands
Git Cheat Sheet
Git Interview Questions
Git Bash
GitHub
Git Branch
Git Merge
Git WorkFlow
Git Hooks
Git LFS
Git Rebase
Git Cherry Pick


Sign In
▲
How to Deploy Angular Application to Firebase using GitHub ?
Last Updated : 23 Jul, 2025
Many of us unable to showcase our small-scale or personal projects over the web. Since hosting these projects are a bit difficult and also costs a few bucks sometimes. In this article, we will show you how to deploy your Angular application at no cost without buying any domain or hosting provider. Also, tired of deploying your application every iteration? Let's also set up automatic builds and deploys using GitHub.

Initialize Git and push the project to the GitHub repository
To configure automated builds and deployments to Firebase, the project must first be pushed to a GitHub repository. 

Open your project in Visual Studio Code.
Open Source control Menu from the sidebar or simply use this shortcut ( press Ctrl+Shift+G) to open.
Click on the Publish to GitHub button as shown below image.

Publish to GitHub
Enter the Repository Name as you desired and select the type of repository public or private.

Wait for VS Code to publish your project to GitHub.
Bundle Angular App for Production
By default, all angular projects are set for development so let's build our project and generate our dist file.

Run the below command in your terminal of the project folder.
ng build --prod

Now after some time, the terminal generates a dist folder that is used for production and deploying.
Download Firebase and setup for deployment
The Firebase Command Line Interface (CLI) Tools can be used to test, manage, and deploy your Firebase project from the command line.

To download and install the Firebase CLI run the following command with the administrator right:
npm install -g firebase-tools
Now you're supposed to login into your firebase with your google account using the following command in your project terminal:
firebase login

Create a Firebase project for deployment
Open the firebase website and go to your console by logging in through the link https://accounts.google.com/v3/signin/identifier?continue=https%3A%2F%2Fconsole.firebase.google.com%2F&followup=https%3A%2F%2Fconsole.firebase.google.com%2F&ifkv=AdBytiN5siwhIM0APP0Z8WGgHkhMahH0ctK5LhEaXB8yq03y0MyZ71wG1TR1uMWDrpfcjG7k2eaSJg&osid=1&passive=1209600&flowName=WebLiteSignIn&flowEntry=ServiceLogin&dsh=S1094424205%3A1753258324288677
After logging in, click on the "Add Project" button, then enter your project name and click on the "Create Project" button to create your Firebase project.
Now it's time for initializing firebase in your project using the following command in your project terminal.
firebase init
Firebase provides various services like Database, Firestore, Functions, Hosting, Storage. Scroll down and select Hosting by pressing the space key to select and then press the Enter button to proceed further.

After pressing enter, now the terminal asks to choose a project. Select the option "Use an existing project" and choose your project name of which you had created earlier pressing the enter key.

Now the terminal asks you to choose your public directory that contains Hosting assets to be uploaded with firebase deploy.
Enter the path of your dist folder which you had generated earlier which contains angular builds. In my case, it is dist/calc-angular

Now select Yes for the option Set up automatic builds and deploys with GitHub? 
Enter your GitHub user name and repository name in the format username/repository for the option "For which GitHub repository would you like to set up a GitHub workflow?"
For all other options just enter Yes or press enter key as you desired.
github1-copy-2


Once Firebase initialization is completed, just enter the following command in your terminal to deploy your project.
firebase deploy
Once the command is executed it provides an output with the link to the firebase project console and URL of the deployed project through which you can access your application through any device.

Now your Angular application is deployed with Firebase successfully.



Comment

More info

Advertise with us
Similar Reads
Git Tutorial
Git is an essential tool for developers, enabling them to manage and track project changes. Whether you're working on a solo project or collaborating with a team, Git keeps everything organized and under control. This Git Tutorial, from beginner to advanced, will give you a complete understanding of
13 min read
Git Introduction
Git Installation and Setup
All Git Commands
Most Used Git Commands
Git Branch
Git Merge
Git Tools and Integration
Git Remote Repositories
Collaborating with Git


geeksforgeeks-footer-logo
Corporate & Communications Address:
A-143, 7th Floor, Sovereign Corporate Tower, Sector- 136, Noida, Uttar Pradesh (201305)
Registered Address:
K 061, Tower K, Gulshan Vivante Apartment, Sector 137, Noida, Gautam Buddh Nagar, Uttar Pradesh, 201305
GFG App on Play Store
GFG App on App Store
Advertise with us
Company
About Us
Legal
Privacy Policy
In Media
Contact Us
Advertise with us
GFG Corporate Solution
Placement Training Program
Languages
Python
Java
C++
PHP
GoLang
SQL
R Language
Android Tutorial
Tutorials Archive
DSA
DSA Tutorial
Basic DSA Problems
DSA Roadmap
Top 100 DSA Interview Problems
DSA Roadmap by Sandeep Jain
All Cheat Sheets
Data Science & ML
Data Science With Python
Data Science For Beginner
Machine Learning
ML Maths
Data Visualisation
Pandas
NumPy
NLP
Deep Learning
Web Technologies
HTML
CSS
JavaScript
TypeScript
ReactJS
NextJS
Bootstrap
Web Design
Python Tutorial
Python Programming Examples
Python Projects
Python Tkinter
Python Web Scraping
OpenCV Tutorial
Python Interview Question
Django
Computer Science
Operating Systems
Computer Network
Database Management System
Software Engineering
Digital Logic Design
Engineering Maths
Software Development
Software Testing
DevOps
Git
Linux
AWS
Docker
Kubernetes
Azure
GCP
DevOps Roadmap
System Design
High Level Design
Low Level Design
UML Diagrams
Interview Guide
Design Patterns
OOAD
System Design Bootcamp
Interview Questions
Inteview Preparation
Competitive Programming
Top DS or Algo for CP
Company-Wise Recruitment Process
Company-Wise Preparation
Aptitude Preparation
Puzzles
School Subjects
Mathematics
Physics
Chemistry
Biology
Social Science
English Grammar
Commerce
GeeksforGeeks Videos
DSA
Python
Java
C++
Web Development
Data Science
CS Subjects
@GeeksforGeeks, Sanchhaya Education Private Limited, All rights reserved
Lightbox

<h1 align="center">Angular - The modern web developer's platform</h1>

<p align="center">
  <img src="adev/src/assets/images/press-kit/angular_icon_gradient.gif" alt="angular-logo" width="120px" height="120px"/>
  <br>
  <em>Angular is a development platform for building mobile and desktop web applications
    <br> using TypeScript/JavaScript and other languages.</em>
  <br>
</p>

<p align="center">
  <a href="https://angular.dev/"><strong>angular.dev</strong></a>
  <br>
</p>

<p align="center">
  <a href="CONTRIBUTING.md">Contributing Guidelines</a>
  ·
  <a href="https://github.com/angular/angular/issues">Submit an Issue</a>
  ·
  <a href="https://blog.angular.dev/">Blog</a>
  <br>
  <br>
</p>

<p align="center">
  <a href="https://www.npmjs.com/@angular/core">
    <img src="https://img.shields.io/npm/v/@angular/core.svg?logo=npm&logoColor=fff&label=NPM+package&color=limegreen" alt="Angular on npm" />
  </a>
</p>

<hr>

## Documentation

Get started with Angular, learn the fundamentals and explore advanced topics on our documentation website.

- [Getting Started][quickstart]
- [Architecture][architecture]
- [Components and Templates][componentstemplates]
- [Forms][forms]
- [API][api]

### Advanced

- [Angular Elements][angularelements]
- [Server Side Rendering][ssr]
- [Schematics][schematics]
- [Lazy Loading][lazyloading]
- [Animations][animations]

### Local Development

To contribute to the Angular Docs, check out the [Angular.dev README](adev/README.md)

## Development Setup

### Prerequisites

- Install [Node.js] which includes [Node Package Manager][npm]

### Setting Up a Project

Install the Angular CLI globally:

```
npm install -g @angular/cli
```

Create workspace:

```
ng new [PROJECT NAME]
```

Run the application:

```
cd [PROJECT NAME]
ng serve
```

Angular is cross-platform, fast, scalable, has incredible tooling, and is loved by millions.

## Quickstart

[Get started in 5 minutes][quickstart].

## Ecosystem

<p>
  <img src="/contributing-docs/images/angular-ecosystem-logos.png" alt="angular ecosystem logos" width="500px" height="auto">
</p>

- [Angular Command Line (CLI)][cli]
- [Angular Material][angularmaterial]

## Changelog

[Learn about the latest improvements][changelog].

## Upgrading

Check out our [upgrade guide](https://angular.dev/update-guide/) to find out the best way to upgrade your project.

## Contributing

### Contributing Guidelines

Read through our [contributing guidelines][contributing] to learn about our submission process, coding rules, and more.

### Want to Help?

Want to report a bug, contribute some code, or improve the documentation? Excellent! Read up on our guidelines for [contributing][contributing] and then check out one of our issues labeled as <kbd>[help wanted](https://github.com/angular/angular/labels/help%20wanted)</kbd> or <kbd>[good first issue](https://github.com/angular/angular/labels/good%20first%20issue)</kbd>.

### Code of Conduct

Help us keep Angular open and inclusive. Please read and follow our [Code of Conduct][codeofconduct].

## Community

Join the conversation and help the community.

- [X (formerly Twitter)][X (formerly Twitter)]
- [Bluesky][bluesky]
- [Discord][discord]
- [YouTube][youtube]
- [StackOverflow][stackoverflow]
- Find a Local [Meetup][meetup]

[![Love Angular badge](https://img.shields.io/badge/angular-love-blue?logo=angular&angular=love)](https://www.github.com/angular/angular)

**Love Angular? Give our repo a star :star: :arrow_up:.**

[contributing]: CONTRIBUTING.md
[quickstart]: https://angular.dev/tutorials/learn-angular
[changelog]: CHANGELOG.md
[ng]: https://angular.dev
[documentation]: https://angular.dev/overview
[angularmaterial]: https://material.angular.dev/
[cli]: https://angular.dev/tools/cli
[architecture]: https://angular.dev/essentials
[componentstemplates]: https://angular.dev/tutorials/learn-angular/1-components-in-angular
[forms]: https://angular.dev/tutorials/learn-angular/15-forms
[api]: https://angular.dev/api
[angularelements]: https://angular.dev/guide/elements
[ssr]: https://angular.dev/guide/ssr
[schematics]: https://angular.dev/tools/cli/schematics
[lazyloading]: https://angular.dev/guide/ngmodules/lazy-loading
[node.js]: https://nodejs.org/
[npm]: https://www.npmjs.com/get-npm
[codeofconduct]: CODE_OF_CONDUCT.md
[X (formerly Twitter)]: https://www.twitter.com/angular
[bluesky]: https://bsky.app/profile/angular.dev
[discord]: https://discord.gg/angular
[stackoverflow]: https://stackoverflow.com/questions/tagged/angular
[youtube]: https://youtube.com/angular
[meetup]: https://www.meetup.com/find/?keywords=angular
[animations]: https://angular.dev/guide/animations
