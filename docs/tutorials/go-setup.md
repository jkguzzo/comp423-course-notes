# Setting up a Dev Container for Go

* Primary author: [Julia Guzzo](https://github.com/jkguzzo)
* Reviewer: [Ryan Neill](https://github.com/raneill26)

Welcome! In this tutorial, you will learn how to set up your own project from scratch using the Go programming language. If you are unfamiliar with Go, that is okay; the point of this tutorial is to walk you through how to create a new project using today’s development tools and practice Git. Before starting, ensure all of the prerequisites are fulfilled.  

## Prerequisites

1. A GitHub Account [Create a GitHub Account](https://github.com/signup)
2. Git installed on your machine [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
3. Visual Studio Code (VSCode) [Install VSCode](https://code.visualstudio.com/)
4. Docker installed [Install Docker](https://www.docker.com/products/docker-desktop/)
5. A basic understanding of the command-line

!!! note
    Feel free to run ```git --version``` and ```docker --version``` to ensure these conditions are satisfied.

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

The last step in creating a repository is creating a README file. A README file is the first point of documentation in a project that contains an overview about the project and other key details someone unfamiliar with the project would need to know. Feel free to edit this file as you like. A good practice is to add the author (that's you!), purpose, and usage for your project.

To create a README, type in the following commands:

```
echo "# Go Tutorial" > README.md 
git add README.md
git commit -m “Initial commit with README”
```

Since there is no README.md file yet in this directory, the first line will create one and will write "Go Tutorial" to this file. The next 2 lines **stage** and **commit** this file to our local repository. 

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
git remote add origin https://github.com/<your-username>/comp423-go-tutorial.git
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

A dev container is a preconfigured environment that includes everything you need to work on a specific project, including the programming language, tools, libraries, and dependencies. Essentially, a dev container is like its own computer that gets rid of inconsistencies across machines. This way, anyone with the dev container can work in an identical environment without having to worry about having the same software and/or versions installed. 

### Step 1: Add Development Container Configuration

First, open your go-tutorial directory in VSCode. To do this, open VSCode, go to File > Open Folder and find the correct directory.

Next, install the **Dev Containers** extension for VSCode.

Now, we have to create a devcontainer directory that holds our devcontainer configuration. To do this, create a ```.devcontainer``` directory in the root of your project. The '.' before ```devcontainer``` makes this directory a **hidden** directory. A **hidden** directory is one that is not visible by default. Inside this directory, create a file called ```devcontainer.json```.

This file defines the configuration for your development environment. We are going to specify the following:

- ```name```: a descriptive name for your dev container
- ```image ```: the Docker image to use 
- ```customizations```: this is how we add useful configurations to VSCode, such as programming language extensions
- ```postCreateCommand```: commands we want to be run everytime the dev container is built

Paste the following into your ```devcontainer.json``` file.
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

- ```"image": "mcr.microsoft.com/vscode/devcontainers/go"```: This uses the official Go base image from Microsoft.
- ```"customizations": {"vscode": {"settings": {},"extensions": ["golang.go"]}}```: Downloads the VSCode extension for Go styling.
- ```"postCreateCommand": "touch main.go && go mod init hello_423"```: After the container is created, this command installs any Python dependencies listed in requirements.txt, creates a main.go file for Go code, and initializes a Go module for the project.

After adding the necessary configurations, rebuild your dev container using ```Click Ctrl + Shift + P``` for Windows,  ```Cmd + Shift + P``` for Mac, or go to ```View``` -> ```Command Pallette```, and type "Dev Containers: Reopen in Container."  You should see a file called ```main.go``` appear in your project's directory. This file is where you will write your Go code. 

To ensure the correct version of Go is in this dev container, run the following command:

```
go version
```

This should return the latest released version of Go if everything was configured correctly.

### Step 2: Creating a Go Function

Copy and paste the following code into ```main.go```:

```
package main

import "fmt"

func main() {
	fmt.Println("Hello COMP423")
}
```

!!! note
    It is not uncommon to be asked to use a programming language that you are unfamiliar with. When this happens, it is important you know how to consult documentation. You can find the documentation for Go [here](https://go.dev/doc/). ```package main``` defines that the file belongs to the ```main``` package. In Go, the ```main package``` is special because it indicates that the file has an executable program. ```import "fmt"``` imports the ```fmt``` package, which contains functions for formatting text and printing to the consult. Finally, the ```func main() { fmt.Println("Hello COMP423) }``` defines the ```main``` function that prints our message.  

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

All that's left is to stage, commit, and push your code! To do so, run the following code:

```
git add .
git commit -m "Finished the Go tutorial"
git push -u origin main
```

Congratulations! You just started a project from scratch using a programming language you were unfamiliar with! Feel free to play around with this program by changing the message that gets printed. Thanks for following along!