# Development ⚡

[Return to Table of Contents](../README.md).

## TERMINAL

> Upgrade your command line to be more productive

1. Install brew `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`;

2. Install iterm2 `install --cask iterm2`;

3. Install zsh `sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`;

4. Install zsh plugins for highlights and autocomplete  
`git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh custom/plugins/zsh-autosuggestions`  
`git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting`  

5. Open iterm2 and paster this command `nano ~/.zshrc` or `vi ~/.zshrc`;

6. In document find line of text with `plugin=(<your plugins>)` and replace it on

```javascript
plugins=(
 git
 sudo
 catimg
 npm
 zsh-syntax-highlighting
 zsh-autosuggestions
)
```

7. Now you are have customized terminal, you can choose theme in iterm2 preferences.

## VSCODE

> Here is description for our devs to be pruductive with thair IDE.

### Make your vscode available from terminal

1. Press `Cmd+Shift+P` in your VSCODE.  

2. Search for `shell command`.  

3. Press `Shell Command: Install 'code' command in PATH`.

4. Close your VSCODE and try to launch it via console `code ./path-to-your-project-folder`
### Usefull shortcuts

> For Linux/Windows use `Ctrl` instead of `⌘`.

`⌘ + P` - open file search.  
`⌘ + B` - close sidebar.  
`⌘ + /` - toggle comments.  
`⌘ + I` - open autocomplete.  
`⌘ + J` - open terminal.  
`⌘  + F` - search in opened file.  
`⌘ + shift + F` - search in all files.  
`shift + option + F` - format code with your Prettier config.  
`⌘ + option + down` - put cursor on line down.  
`⌘ + option + up` - put cursor on line up.  
`option + down` - move line of code down.  
`option + up` - move line of code up.  

### Useful extensions

> To increase your codding speed you can use those extensions.  

`Bracket Pair Colorizer` - colors your brackets for better navigation in project.  
`Prettier - Code formatter` - it will help to format your scripts automatic.  
`Eslint` - it helps you to keep your code the same style as your team members.  
`Code Spell Checker` - spellcheck your words in the app.  
`Import Cost` - give you ability to analyze size of your imports.  
`GitLens` - helps you tracks pr/changes authors in the app.  
`vscode-styled-components` - highlights syntax in styled components.  

### Tips and Tricks

> For better developer experience you can use our `vscode` config file.  
> Link: [VSCODE settings file](../features-components/settings/settings.json)

Its Gives you ability to:  

- Run eslint on save.

- Colors your editor sidebar into our corporate colors (if your project uses separate repo for backend and frontend you can set different color for `editorSuggestWidget.selectedBackground` param, to be sure that you are in correct repo)

Installation:

1. Run `mkdir .vscode` in your project folder.  

2. Run `touch .vscode/settings.json`.

3. Run `nano .vscode/settings.json`.

4. Paste content of [VSCODE settings file](../features-components/settings/settings.json) to created `settings.json` file.  
