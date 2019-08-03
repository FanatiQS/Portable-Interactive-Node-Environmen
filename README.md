# Portable-Interactive-Node-Environment (PINE)

## This is a script that runs a node-script-file as well as downloads and manages portable versions of Node.js to run the node-script-file with. Nothing is installed on the local computer, everything is contained inside a "lib" folder created in the same directory next to the script.

How the portable Node.js part works:
The script downloads the binaries for the Node.js version specified or for the latest stable version if not specified, based on your OS and architecture, then runs the node-script-file with the executable from that downloaded binary.

## Manually downloading Node.js binaries:
Go to "https://nodejs.org/en/download/" and download the "binary" version for your computer (not an installer). Then unpack it into the "lib/binary" folder and keep the name intact. If the folder path doesn't exist, you can just create it. The script should now be able to find that Node.js binary and use it. If it doesn't, check what OS and architecture the script detects with the command line option "-i" and if something is not detected properly, please get in contact with me and we'll figure it out.

## Adding the node-script-file:
Simply move/copy or download a Node.js package/"index.js" file to "lib/node". If the folder path doesn't exist, you can just create it manually or run the script once to automatically create it.

## Settings file:
Settings can be loaded from an optional "settings.txt" file. The options available are: version, context, binary and package. They are all configured like "option=value".
	version: The "version" option defines what version of node to use, like "version=10.16.0". More info under the "Version" chapter.
	context: This is used to specify the context, like "context=module.exports". More info under the "Interaction" chapter.
	binary: Changes the directory where the script stores its Node.js binaries, like "binary=lib/binary". Default: "lib/binary"
	package: Changes the directory where the script gets the node-script-file to run, like "package=lib/node". Default; "lib/node"

## Command line options:
The command line options are used to specify how the script will run.
	-v or -version: Specifies the version to use with an argument, like "PINE-executable -v 10.16.0". More information in the "Version" chapter.
	-i or -info: This option displays what OS and architecture is detected as well as what versions are downloaded matching that OS and architecture. It will not run the node-script-file, even if a version is specified. This command line options should be used by itself and is not meant to be used in combination with other options.
	-d or -download: This is used to download new Node.js binaries without running the node-script-file. It takes an argument specifying the version to download, like the "-v" or "-version" option.
	-h or -help: Right now, there is no help to be had.

## Starting:
The script will only run the node-script-file from "lib/node" if there are no command line options or if the "-v" or "-version" is included in the options. For more information about how a version is selected without a command line option, go to the "Version" chapter. Before the script runs, it will display info about the OS, architecture, version and more. This info is however scrolled of screen to only display the Node.js interactive console. To see this information, just scroll up past the start of the node-script-file.

## Version:
You can specify what version of Node.js to use. Multiple versions can be downloaded to switched between if needed. Using the "-v" or "-version" command line option is top priority when the script selects what version to use. More info in the "Command line options" chapter. If that option is not used, the script looks for a version in the "settings.txt" file, if it exists. More info in the "Settings file" chapter. When a version is specified with one of the above methods and does not exist in the "lib/binary" folder, it will automatically be downloaded and used. If none of these are specified, the latest downloaded version will be chosen. However, if there is no version downloaded, the latest LTS version of Node.js will automatically be downloaded and used.

## Interaction:
When the node-script-file runs, it can still be interacted with from the command line. This is what I call the "interactive console". The interactive console has access to all local and global variables by default since they exist in the same scope. You can however change the scope for the interactive console by defining what the scope should be in the "settings.txt" file. For example, "context=module.exports" will give the interactive console access to all the properties in the "module.exports" object. Manually defining an object is also possible. Something like "context={foo,bar}" would only give it access to the variables "foo" and "bar". It is important to remember that you can't have spaces or you need to put you value in quotes. So "context={foo, bar}" will not work, but "context="{foo, bar}"" and "context={foo,bar}" will work. If the context value is not an object, it will just silently be ignored and not change the scope.There are only two properties that are always added to the context object. That is the "_" and "_error" properties. They come from the last input and last error and are added by the "repl" module used for interaction. If you want to see all the properties available on the context object, run "Object.getOwnPropertyNames(this)" to see a list of all the properties available. (Note that variables will not show up in the context object and if specifying a context object without a "console" property, the console will not work). The way all this works is by executing the node-script-file in the global scope instead of in a module scope so the interactive console can access it. If the context is changed, the interactive console is separated into a its own context and and has its own global variable that we define with the "context" property. The only issue with this method is that you can not have return statements at the top level of the node-script-file.

## Bugs:
* This is not really a bug, but all the built-in modules are automatically added to the global object. This is done by Node.js since the script is evaluating code instead of using separate files.
