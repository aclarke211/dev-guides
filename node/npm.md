# NPM [Dev Guide]

## Package JSON
### Version
#### Automatically Update Project Version
In order to automatically update the project version in a *package.json* file, add the following to your **npm scripts**:

```json
"pre-release": "npm version patch && git push && git push --tags",
"release": "npm run pre-release && git push heroku master"
```

> We would then run `npm run release` each time we want to release (to Heroku in this example).\
> The *pre-release* script would be automatically ran first, which increases the version number then commits and pushes the *package.json* change to GitHub.\
> A **git tag** is also committed, which allows us to easily jump back through the previously released versions of the project.

