[Tasks]
Таски можно вызывать ctrl+shift+P. Выбираешь какую таску выполнить.
Таски пишуться в task.json доп блоком кода({ }).
Пример:
```
{
"tasks":[
{
"label": "Какое-то имя"
"type": "Тип"
}
]
}
```
[Atribute for launch.json]
#docs
The following attributes are mandatory for every launch configuration:

- `type` - the type of debugger to use for this launch configuration. Every installed debug extension introduces a type: `node` for the built-in Node debugger, for example, or `php` and `go` for the PHP and Go extensions.
- `request` - the request type of this launch configuration. Currently,[](vscode://settings/launch)

- [launch](vscode://settings/launch) and `attach` are supported.
- `name` - the reader-friendly name to appear in the Debug launch configuration dropdown.

Here are some optional attributes available to all launch configurations:

- **``presentation``** - using the `order`, `group`, and `hidden` attributes in the `presentation` object, you can sort, group, and hide configurations and compounds in the Debug configuration dropdown and in the Debug quick pick.
- **``preLaunchTask``** - to launch a task before the start of a debug session, set this attribute to the label of a task specified in [tasks.json](https://code.visualstudio.com/docs/debugtest/tasks) (in the workspace's `.vscode` folder). Or, this can be set to `${defaultBuildTask}` to use your default build task.
- **``postDebugTask``** - to launch a task at the very end of a debug session, set this attribute to the name of a task specified in [tasks.json](https://code.visualstudio.com/docs/debugtest/tasks) (in the workspace's `.vscode` folder).
- **``internalConsoleOptions``** -  controls the visibility of the Debug console panel during a debugging session.
- **``debugServer``** - **for debug extension authors only**: this attribute allows you to connect to a specified port instead of launching the debug adapter.
- **``serverReadyAction``** - if you want to open a URL in a web browser whenever the program under debugging outputs a specific message to the debug console or integrated terminal. For details see section [Automatically open a URI when debugging a server program](https://code.visualstudio.com/docs/debugtest/debugging-configuration#_automatically-open-a-uri-when-debugging-a-server-program) below.

Many debuggers support some of the following attributes:

- `program` - executable or file to run when launching the debugger
- `args` - arguments passed to the program to debug
- `env` - environment variables (the value `null` can be used to "undefine" a variable)
- `envFile` - path to dotenv file with environment variables
- `cwd` - current working directory for finding dependencies and other files
- `port` - port when attaching to a running process
- `stopOnEntry` - break immediately(немедленно) when the program launches
- `console` - what kind of console to use, for example, `internalConsole`, `integratedTerminal`, or `externalTerminal` 
**Простой способ запуска  программ VS code**
Есть два способа:
1) С помощью расширения Code Runner:[WayForLaunch]
```
```"code-runner.clearPreviousOutput": true,

"code-runner.showExecutionMessage": false,

"code-runner.executorMap": {

"javascript": "node",

"java": "cd $dir && javac $fileName && java $fileNameWithoutExt",

"c": "cd $dir && gcc $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",

"zig": "zig run",

"cpp": "cd $dir && g++ -std=c++17 $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",

"objective-c": "cd $dir && gcc -framework Cocoa $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",

"php": "php",

"python": "python -u",

"perl": "perl",

"perl6": "perl6",

"ruby": "ruby",

"go": "go run",

"lua": "lua",

"groovy": "groovy",

"powershell": "powershell -ExecutionPolicy ByPass -File",

"bat": "cmd /c",

"shellscript": "bash",

"fsharp": "fsi",

"csharp": "scriptcs",

"vbscript": "cscript //Nologo",

"typescript": "ts-node",

"coffeescript": "coffee",

"scala": "scala",

"swift": "swift",

"julia": "julia",

"crystal": "crystal",

"ocaml": "ocaml",

"r": "Rscript",

"applescript": "osascript",

"clojure": "lein exec",

"haxe": "haxe --cwd $dirWithoutTrailingSlash --run $fileNameWithoutExt",

"rust": "cd $dir && rustc $fileName && $dir$fileNameWithoutExt",

"racket": "racket",

"scheme": "csi -script",

"ahk": "autohotkey",

"autoit": "autoit3",

"dart": "dart",

"pascal": "cd $dir && fpc $fileName && $dir$fileNameWithoutExt",

"d": "cd $dir && dmd $fileName && $dir$fileNameWithoutExt",

"haskell": "runghc",

"nim": "nim compile --verbosity:0 --hints:off --run",

"lisp": "sbcl --script",

"kit": "kitc --run",

"v": "v run",

"sass": "sass --style expanded",

"scss": "scss --style expanded",

"less": "cd $dir && lessc $fileName $fileNameWithoutExt.css",

"FortranFreeForm": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",

"fortran-modern": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",

"fortran_fixed-form": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",

"fortran": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",

"sml": "cd $dir && sml $fileName",

"mojo": "mojo run",

"erlang": "escript",

"spwn": "spwn build",

"pkl": "cd $dir && pkl eval -f yaml $fileName -o $fileNameWithoutExt.yaml",

"gleam": "gleam run -m $fileNameWithoutExt"

},

"C_Cpp_Runner.msvcBatchPath": ""

}```
```
2) Способ через таски:[WayForLaunch]
```{

"version": "2.0.0",

"tasks": [

{

"label": "Build and Run",

"type": "shell",

"command": "g++",

"args": [

"-g",

"${file}",

"-o",

"${fileDirname}/${fileBasenameNoExtension}"

],

"group": {

"kind": "build",

"isDefault": true

},

"problemMatcher": [

"$gcc"

],

"presentation": {

"reveal": "always",

"panel": "new"

},

"options": {

"cwd": "${workspaceFolder}"

},

"runOptions": {

"runOn": "default"

}

},

{

"label": "Run in New Terminal",

"type": "shell",

"command": "${fileDirname}/${fileBasenameNoExtension}",

"problemMatcher": [],

"presentation": {

"reveal": "always",

"panel": "new",

"focus": true,

"clear": true

},

"dependsOn": [

"Build and Run"

],

"options": {

"cwd": "${workspaceFolder}"

}

}

]

}
```