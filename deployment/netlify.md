# Netlify [Dev Guide]

## About
Netlify is an online service where we can deploy and host static websites.
Netlify is free of charge, however there are limits on the amount of sites you can have live at any given time.

## Installing Netlify CLI
In order to deploy projects to Netlify via the command line, we first need to install netlify globally on our machine.

```bash
npm i netlify-cli -g
```

## Logging Into the CLI
In order to use the Netlify features, we first need to login via the command line.

```bash
netlify login
```
> This command can be entered from any directory.

A browser window will now open, where we can **Authorize** the Netlify CLI.
![Netlify Browser Authenication](https://i.ibb.co/6bb3XHg/Screenshot-2020-01-03-at-14-28-08.png)

![Netlify Authorisation Granted](https://i.ibb.co/rm3TDTm/Screenshot-2020-01-03-at-14-30-09.png)

**[NOTE]:** You may need to login into your account in the browser (if you are not already logged in).

## Linking a Repo
Once we have navigated to the local repo directory via a terminal, we can setup a connection between the repo and Netlify.

For repos which are stored on GitHub you can use the `netlify init` command to automatically setup the connection.

For other Git providers (such as BitBucket) we need to use the following command:
```bash
netlify init --manual
```

Follow the on-screen prompts and add the SSL Key to the repo in the Git Provider (i.e. BitBucket)  when it is shown.
