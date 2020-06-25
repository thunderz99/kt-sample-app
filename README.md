# Debugging Kotlin program using vscode

## TL;DR

Install fwcd.kotlin extension.

## Summary

Many sites demostrate how to code and run Kotlin programs, without showing how to debug it.

I want to share what I tried and got worked, if someone else face the same situation. (I tried on Mac, but the concept is common for Windows PC.)


## Prerequisites

* vscode
* Java(OpenJDK)

The instructions are skipped to install the above prerequisites.


## Procedures

### 1. Install kotlin and gradle


```shell
% brew install kotlin
% brew install gradle
```

### 2. create a kotlin sample app

```shell
% mkdir kt-sample-app
% cd kt-sample-app
% gradle init --type=kotlin-application
# Select all default choices is OK
 
# Have a try is everything is ok
% ./gradlew build

BUILD SUCCESSFUL in 3s
8 actionable tasks: 8 executed
```

### 3. Install VSCode extensions

```
% mkdir -p .vscode
% vi .vscode/extensions.json

----- Input contents as below -----
{
    "recommendations": [
        "fwcd.kotlin",
        "vscjava.vscode-java-pack"
    ]
}

# Launch vscode
% code .

```

Select "Install All" to the dialog showed, which will install the 2 extensions wrote above.

Open App.kt file, and you will see "syntax highlight", "auto compleletion" working.

### 4. Settings to build and debug

```
% vi .vscode/tasks.json

----- Input the content below -----

{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "./gradlew build -x test",
            "problemMatcher": [],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        },
        {
            "label": "run",
            "type": "shell",
            "command": "./gradlew run",
            "problemMatcher": []
        },
        {
            "label": "test",
            "type": "shell",
            "command": "./gradlew test",
            "problemMatcher": []

        }
    ]
}

```

In vscode menu,  Terminal -> Run Task... 
You can execute  gradlew's build / run / test tasks.

Finally the debug settings:

```
% vi .vscode/launch.json

----- Input content below -----

{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "kotlin",
            "request": "launch",
            "name": "Kotlin Launch",
            "projectRoot": "${workspaceFolder}",
            "mainClass": "kt/sample/app/AppKt",
            "preLaunchTask": "build"
        }
    ]
}

```

* Rewrite the mainClass to fit to your project.

Set some breakpoint,  debug the program by vscode's Debug tab  -> Run button. 


