# Setting up a Dev Container for Go

* Primary author: [Julia Guzzo](https://github.com/jkguzzo)
* Reviewer: [Ryan Neill](https://github.com/raneill26)

Welcome! In this tutorial, you will learn how to set up your own project from scratch using the Go programming language. If you are unfamiliar with Go, that is okay; the point of this tutorial is to walk you through how to create a new project using today’s development tools, practice Git, and learn how to use technical documentation when working with an unfamiliar tool. Before starting, ensure all of the prerequisites are fulfilled.  

## Prerequisites

1. A GitHub Account [Create a GitHub account](https://github.com/signup)
2. Git installed on your machine [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
3. Visual Studio Code (VSCode) [Install VSCode](https://code.visualstudio.com/)
4. Docker installed [Install Docker](https://www.docker.com/products/docker-desktop/)
5. A basic understanding of the command-line

## Part 1: Project Setup

### Step 1: Creating a Local Repository

The first step in making a new project after ensuring all of the necessary conditions are satisfied is creating a new **directory**. A **directory** is like a folder that lives on your machine that organizes and manages certain files. 

Think of a name you’d like to give your directory. This name should be descriptive of the content that you will be putting in the directory. In this instance, a name like “go-tutorial” would be sufficient, as it contains the name of the programming language you are using and indicates that you are following a tutorial. 

When you have decided on a name, open your machine’s terminal and type in the following commands: 

```
mkdir <directory name>
```

```
cd <directory name>
```

Now that you have created a directory (**mkdir**) and changed into that directory (**cd**), you need to initialize a new, empty Git repository. To do so, run the following command:

```
git init
```

The last step in creating a repository is creating a README file. A README file is the first point of documentation in a project that contains an overview about the project, including its purpose, how to use it, and any other key details someone unfamiliar with the project would need to know. 

To do this, type in the following commands:

```
echo "# Go Tutorial" > README.md 
git add README.md
git commit -m “Initial commit with README”
```

Since there is no README.md file yet in this directory, the first line will create one and will write "Go Tutorial" to this file. The next 2 lines **stage** and then **commit** this file to our local repository. 

At this point, we only have a local repository created. This means that all work done in this directory will only exist on your machine. This can be problematic, so we should create a remote repository, too.

A remote repository is similar to your local repository, except instead of existing solely on your machine, your project is hosted on the web. 

### Step 2: Creating a Remote Repository on GitHub

Log in to GitHub.

Find the [Create a New Repository](https://github.com/new) page and fill in the following details:

- Repository Name: ```comp423-go-tutorial```
- Description: "Followed a Go tutorial"
- Visibility: Public

Do not initialize the repository with a README, .gitignore, or license.

When you have checked to make sure these details are correct, click **Create Repository**.

Now, we have a local repository and a remote repository, but they are not linked.

### Step 3: Linking your Local Repository to GitHub

To link your local repository to the remote repository, enter the following command in the terminal

```
git remote add origin https://github.com/<your-username>/comp423-course-notes.git
```
replacing `<your-username>` with your GitHub username.

The default branch name should be ```main```. To check, use the subcommand ```git branch```. If the default branch isn't named 'main', rename it using the following command:

```
git branch -M main  
```

Next, push your local commits to the GitHub repository:

```
git push --set-upstream origin main
```

Now, your local repository should be linked to your GitHub repository. To confirm this worked, navigate to your GitHub repository, refresh, and you should see the same commit you made locally. You can also enter ```git log``` into your terminal to see the commit ID, which should match the ID of the most recent commit on GitHub. 

## Part 2: Dev Container Configuration

A dev container is a preconfigured environment that includes everything you need to work on a specific project, including the programming language, tools, libraries, and dependencies. Essentially, a dev container is like its own computer that gets rid of inconsistencies across machines. This way, anyone with the dev container can work in an identical environment without having to worry about having the same things installed and with the same versions. 

### Step 1: Add Development Container Configuration

First, open your go-tutorial directory in VSCode. To do this, open VSCode, go to File > Open Folder and find the correct directory.

Next, install the **Dev Containers** extension for VSCode.

Then, create a ```.devcontainer``` directory in the root of your project with the following file inside of this "hidden" configuration directory:

```
.devcontainer/devcontainer.json
```

This file defines the configuration for your development environment. We are going to specify the following:

- ```name```: a descriptive name for your dev container
- ```image ```: the Docker image to use 
- ```features```: additional components we want to use
- ```customizations```: this is how we add useful configurations to VSCode, such as programming language extensions
- ```postCreateCommand```: commands we want to be run everytime the dev container is built

```
{
  "name": "COMP423 DevContainer",
  "image": "mcr.microsoft.com/devcontainers/python:latest",
  "features": {
    "ghcr.io/devcontainers/features/go:1": {
      "version": "latest"
    }
  },
  "customizations": {
    "vscode": {
      "settings": {},
      "extensions": [
        "ms-python.python",
        "golang.go"
      ]
    }
  },
  "postCreateCommand": "pip install -r requirements.txt && touch main.go && go mod init hello_423"
}
```

- ```"image": "mcr.microsoft.com/devcontainers/python:latest"```: This uses the latest official Python base image from Microsoft. 
- ```"features": { "ghcr.io/devcontainers/features/go:1": { "version": "latest" } }```: This feature adds the latest version of Go to the dev container, so you can use it alongside Python.
- ```"postCreateCommand"```: "pip install -r requirements.txt && touch main.go && go mod init hello_423": After the container is created, this command installs any Python dependencies listed in requirements.txt, creates a main.go file for Go code, and initializes a Go module for the project.

If you don't need your project to support Python, your devcontainer.json file might look like this:

```
{
  "name": "COMP 423 Go Tutorial",
  "image": "mcr.microsoft.com/vscode/devcontainers/go",
  "customizations": {
    "vscode": {
      "settings": {},
      "extensions": [
        "golang.go"
      ]
    }
  },
  "postCreateCommand": "touch main.go && go mod init hello_423"
}
```

After adding the necessary configurations, rebuild your dev container and you should see a file called ```main.go``` appear in your project's directory. This file is where you will write your Go code. 

### Step 2: Creating a Go Function

Copy and paste the following code into ```main.go```:

```
package main

import "fmt"

func main() {
	fmt.Println("Hello COMP423")
}
```

Save and rebuild the dev container yet again. 

### Step 3: Running the Program

Now that you have your dev container set up and your main function written, you should be able to run your program. To do this, run:

```
go run main.go
```

!!! note
    The ```run``` subcommand **compiles** and **runs** the Go program in one step. Since it doesn't create a binary file, it is most useful for on-the-fly testing. You should see ```Hello COMP423``` printed directly to your terminal from this one line of code.

You can also use the ```build``` subcommand to compile your program into a binary file. This step is similar to how you would use the ```gcc``` command in a C project, where ```gcc``` compiles your source code into an executable. For Go, use: 

```
go build -o hello
```

!!! note
    The ```build``` command compiles your Go code into a binary executable named hello. Unlike ```go run```, the ```build``` subcommand doesn't execute your program; it only compiles it. You won't see any output from ```go build``` unless there's an error in your code.

The key difference from ```go run``` is that ```go build``` creates an independent executable that you can run multiple times without needing to recompile every time. This is the same as compiling a program with gcc in a C project. ```gcc``` generates an executable that can be run independently of the source code.

Once the binary is built, you can run it directly:

```
./hello
```

This will execute the compiled binary, and you'll see the output ```Hello COMP423``` in your terminal, just like with the ```go run``` command. However, now that you have the binary, you don't need to rerun ```go build``` unless you make changes to your source code. Simply executing ```./hello``` will run your program.