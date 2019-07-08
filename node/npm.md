# NPM [Dev Guide]

## NPM Commands
### Packages
#### Install/Remove packages
We can run the **install** (or **i** for short) command to install packages.

```powershell
npm install
```
or
```powershell
npm i
```

To install a new or **specific package** we simply specify the name of the package after the install command:

```powershell
npm i axios
```

> In the case above, **axios** is the name of the package to install.

We can also specifiy a precise version of a package to install:

```powershell
npm i axios@0.18.0
```

To get the latest version of a package:
```powershell
npm i axios@latest
```

To install a package globally on your machine:

```powershell
npm i -g axios
```


To remove a package we can do:
```powershell
npm uninstall axios
```


## Package JSON
### NPM Scripts
The following are NPM Scripts which can be added into the *package.json* file of a project.

#### Automatically Update Project Version
In order to automatically update the project version in a *package.json* file, add the following to your **npm scripts**:

```json
"pre-release": "npm version patch && git push && git push --tags",
"release": "npm run pre-release && git push heroku master"
```

> We would then run `npm run release` each time we want to release (to Heroku in this example).
> The *pre-release* script would be automatically ran first, which increases the version number then commits and pushes the *package.json* change to GitHub.
> A **git tag** is also committed, which allows us to easily jump back through the previously released versions of the project.

A placeholder release script (if you do not currently have a platform setup to release on) could be:

```json
"pre-release": "npm version patch && git push && git push --tags",
"release": "npm run pre-release && echo \"\\x1b[97m\\x1b[100mNo platform to release on.\\x1b[0m\""
```


#### Refresh Node Modules
The following script will remove the *node_modules* folder (if it is present in the project).
The node modules will then be re-installed and it will perform an audit on the packages to see if there are any vulnerabilities present.

If there are vulnerabilities present, the script will attept to fix these automatically.

```json
"refresh:node_modules": "rm -fr node_modules && npm i && npm audit && npm audit fix"
```

#### Git Commit and Push to Repo
We can use an NPM script to commit and push to our Git repo.

```json
"git:commit": "git add . && git commit --allow-empty -m",
"git:push": "git push --all",
```

To perform a commit and push we would run the following command:

```powershell
npm run git:commit -- \"This is the message for the commit.\" && npm run git:push
```


## Command Line

### Print Colours
Putting these codes before or after a log will change the colour of the logs in teh terminal.

If you do not **reset** the terminal after changing the colour, then the colour change will remain for all logs make to the terminal, until it is reset or the colour is changed again.

You can chain colour changes together, i.e. have a red background and white foreground.

You may need to escape the first '\' in the colour codes if you get an error when implementing a colour change, i.e. `\x1b[0m` to reset the terminal would actually be `\\x1b[0m`, escaping the first slash.

> **KEY**\
> Fg = Foreground\
> Bg = Background

#### Actions
| Colour        | Code          |
|---------------|---------------|
| Reset         | \x1b[0m       |
| Bright        | \x1b[1m       |
| Dim           | \x1b[2m       |
| Underscore    | \x1b[4m       |
| Blink         | \x1b[5m       |
| Reverse       | \x1b[7m       |
| Hidden        | \x1b[8m       |
| FgBlack       | \x1b[30m      |
| FgRed         | \x1b[31m      |
| FgGreen       | \x1b[32m      |
| FgYellow      | \x1b[33m      |
| FgBlue        | \x1b[34m      |
| FgMagenta     | \x1b[35m      |
| FgCyan        | \x1b[36m      |
| FgWhite       | \x1b[37m      |
| BgBlack       | \x1b[40m      |
| BgRed         | \x1b[41m      |
| BgGreen       | \x1b[42m      |
| BgYellow      | \x1b[43m      |
| BgBlue        | \x1b[44m      |
| BgMagenta     | \x1b[45m      |
| BgCyan        | \x1b[46m      |
| BgWhite       | \x1b[47m      |
