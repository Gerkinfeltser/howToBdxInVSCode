# [HOWTO] Setup VSCode as an IDE for BDX

**NOTICE: VSCode Java addons requires JDK 1.8.x or greater. BDX with Android projects require no greater then 1.7.x.
For this reason, Android BDX projects cannot be developed on VS Code at this time (though I could be very wrong about this).**


### Start a new BDX project In Blender (**skip for pre-existing projects**): ###
1) Setup your BDX project as usual (again, sorry but leave the Android SDK slot blank).
2) After the project is setup, run the project once via the Render tab button labeled *Export and Run BDX*.

### In VS Code: ###
1) Install the [Microsoft Java Extention Pack](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack).
2) Open your BDX project's root Folder (*File > Open Folder... [Ctrl+K Ctrl+O]*)
3) Open **settings.gradle** and remove `'android', ` so the file appears like this:
`include 'core', 'desktop', 'ios', 'html'`
4) Open **build.gradle** and remove (or comment out) lines 60 through 77 (the whole `project(":android") {... }` bit.
5) Restart VS Code. After restarting, some Java api stuff show up in the status bar at the bottom as VS Code scans your project for classes and functions.

If all went well, when its done you should be good to start coding with intellisenseTM/code completion! Congratulations! :tada:

## Setting up the debugger in VS Code ##
1) Click the Debug tab in the left bar in Code (it's the one with a crossed out bug icon).
2) Hit F5 get code to generate a Java launch command **launch.json** file in the project root's **.vscode** folder (which also gets created at this time if it hasn't been already).
3) Clear the newly created **launch.json** file, paste the following into it, and save.
```
{
"version": "0.2.0",
"configurations": [
        {
            "type": "java",
            "name": "Debug (Attach)",
            "request": "attach",
            "hostName": "localhost",
            "port": 5005
        }
    ]
}
```
4) Open the integrated terminal (*View > Integrated Termingal* or *Ctrl+\`*) and assuming the terminal starts in the project root (it should), run `./gradlew.bat desktop:run --debug-jvm`
5) The game should build then stop short of creating a window. At this point hit **F5** & Debug away! Breakpoints also function should you care to use them! :tada:

## Setting up a build command ##
1) Select *Tasks > Configure Tasks...*
2) This should create a **tasks.json** in the before mentioned **.vscode** directory and open said file (which you could also just create yourself). Delete the generated files contents, paste in the following, and save.
```
{
"version": "2.0.0",
"tasks": [
        {
            "label": "Start game",
            "type": "shell",
            "command": "./gradlew.bat desktop:run",
            "group": {
                "kind": "build",
                "isDefault": true
            }
        },
        {
            "label": "Debug with JVM",
            "type": "shell",
            "command": "./gradlew.bat desktop:run --debug-jvm"
        }
    ]
}
```
3) Hit *Ctrl+Shift+B* to run the default **Start game** build task or select *Tasks > Run Tasks...* and select the build command you'd like. Remember to hit *F5* if you select the **Debug with JVM** after its done with its initial cooking! Also note, you'll probably have to hit *Continue without scanning the task output* (or the *Never scan the task ouput* which places *"problemMatcher": []* in the task in tasks.json) option in the command bar to actually get the build to start.

## More [clarity] to come later! :tada: ##
