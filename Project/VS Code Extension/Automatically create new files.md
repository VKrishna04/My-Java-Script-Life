Creating a Visual Studio Code extension involves several steps and files. Here's a simplified example of how you could create an extension that automatically creates a new file when a link to a non-existing file is clicked in a Markdown document.

First, you'll need to set up a new extension. You can do this by following the [VS Code extension tutorial](https://code.visualstudio.com/api/get-started/your-first-extension). This will create a new directory with the basic files needed for an extension.

Next, replace the code in `extension.js` (or `extension.ts` if you're using TypeScript) with the following:

```javascript
const vscode = require('vscode');
const fs = require('fs');
const path = require('path');

function activate(context) {
    let disposable = vscode.commands.registerCommand('extension.createFile', function (uri) {
        let filePath = uri.fsPath;
        if (!fs.existsSync(filePath)) {
            fs.writeFileSync(filePath, '');
        }
        vscode.window.showTextDocument(vscode.Uri.file(filePath));
    });

    context.subscriptions.push(disposable);
}
exports.activate = activate;
```

This code registers a command named `extension.createFile` that creates a new file at the specified path if the file does not exist, then opens the new file in a text editor.

Finally, you'll need to override the default link click behavior in Markdown documents to call your command. Unfortunately, VS Code does not currently provide an API for overriding the default link click behavior. This means that it's not possible to automatically create a new file when a link to a non-existing file is clicked in a Markdown document.

However, you can use your command to create a new file from the Command Palette or from a keybinding. To do this, add the following to your `package.json` file:

```json
"contributes": {
    "commands": [
        {
            "command": "extension.createFile",
            "title": "Create File"
        }
    ]
}
```

This makes your command available in the Command Palette (`Ctrl+Shift+P`), where you can run it by typing `Create File`. You can also assign a keybinding to your command by adding the following to your `keybindings.json` file:

```json
{
    "key": "ctrl+alt+n",
    "command": "extension.createFile"
}
```

This assigns the `Ctrl+Alt+N` keybinding to your command. When you press `Ctrl+Alt+N`, VS Code will prompt you for a file path, then create a new file at that path if it does not exist.