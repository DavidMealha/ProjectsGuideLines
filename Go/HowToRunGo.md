# How to run Go Services on Linux Ubuntu 101
[Source](https://medium.com/@patdhlk/how-to-install-go-1-9-1-on-ubuntu-16-04-ee64c073cd79)

## Install Go

To install Go on Ubuntu 16.04 it is necessary to download the tar file through curl (install curl if necessary with apt-get).
Do this at $HOME => /home/your_username/

```shell
sudo curl -O https://storage.googleapis.com/golang/go1.9.1.linux-amd64.tar.gz
```

Then, extract the files and move the folder to your prefered location, usually at $HOME (the directory of a go project has to be a child of go path).

```shell
sudo tar -xvf go1.9.1.linux-amd64.tar.gz
sudo mv go $HOME
```

Set **GOPATH** and **GOBIN** paths.
In this language all projects must be in the **same workspace** (folder), which means the GOPATH. 
In this example I set the workspace as $HOME/go-projects and then we have GOROOT with the files of the programming language.

```shell
sudo nano ~/.profile
```

Add the following lines to the end of the file. 
I chose to locate my go (GOROOT) folder in $HOME but could be anywhere, like /usr/local.

```shell
export GOROOT=$HOME/go
export GOPATH=$HOME/go-projects
export GOBIN=$HOME/go/bin
export PATH=$PATH:$HOME/go/bin
```

Save the changes and refresh the profile.

```shell
source ~/.profile
```

Verify that it was installed by checking the version.

```shell
go version
```

## Create your first go program

To ease the integration with Github create a directory.
Also, let's create an example.

```shell
mkdir -p $GOPATH/src/github.com/DavidMealha
mkdir -p $GOPATH/src/github.com/DavidMealha/hello
```

**Time to code :-)**

```shell
nano $GOPATH/src/github.com/DavidMealha/hello/hello.go
```

```go
package main
import "fmt"
func main() {
    fmt.Printf("hello, world\n")
}
```

Compiling is as easy as using go install (must be inside the src folder).
But first we need to set more permissions.
It will generate an executable program in the bin.

```shell
sudo chmod -R 777 ~/go
go install github.com/DavidMealha/hello
```

To run the program just write the following command.

```shell
$GOBIN/bin/hello
```

## Build catalogue service

__Although it is not necessary to compile the go program in your OS, it helps understanding how it works and find some bugs that may occur__
The first step is to have the git repository into golang workspace. 
This command will automatically clone the repository.

```shell
go get -u github.com/DavidMealha/catalogue
```

Then, it is necessary to remove the tracing tools from the code and solve some errors. Check the commits from March 13th and 14th regarding.
After that just follow the instructions in the repository README. The first two will install a vendor tool to download dependencies.

```shell
go get -u github.com/FiloSottile/gvt
gvt restore

cd $GOPATH/src/github.com/microservices-demo/catalogue/cmd/cataloguesvc/
go build -o ./app/catalogue
```

At this time, it is possible to see the binary/application in the same folder.
