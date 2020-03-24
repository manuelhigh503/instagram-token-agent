# Instagram Token Agent

## A service to keep your Instagram Basic Display API token fresh.

⚠️**This is a work in progress and not ready for use yet!**

This agent is designed to run as a small, self-contained app on [Heroku](https://heroku.com) (though there are other ways to run it if you prefer). By default, it runs using free services and will keep your token up to date once set up correctly.

This agent is designed to work with libraries like [instafeed.js](https://github.com/stevenschobert/instafeed.js) which need a valid token to operate. These tokens need to be periodically refreshed - **Instagram Token Agent** takes care of that for you.

## You will need

To begin, you'll need the following:

 - A Facebook Developer account
 - An Instagram account
 - A Heroku account

## Usage

**1.** [Follow the instructions here](https://developers.facebook.com/docs/instagram-basic-display-api/getting-started) to create an application on Facebook to connect to Instagram and generate a Long-lived Basic Display API token for your user. Copy this token and keep it handy for the next step.

**2.** Click this handy 'Deploy' button:

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

**3.** On the page that follows, fill in the following values:

![heroku-deploy-config](https://user-images.githubusercontent.com/53896/77387150-83d18300-6ddc-11ea-9095-e846a2d206c4.png)

 - **App name**: Make up a name for your app - it needs to be unique, like a username. Something like `your-name-token-agent` will do.

_Under 'Config vars'_:

- **App name**: Copy+paste your app name here as well.
- **Starting Token**: Paste your initial token from Facebook here.

Everything else can be left as default.

**4.** Click _Deploy App_.

The deployment process can take a minute or so. Once complete, you'll see a _View_ button to visit your new application.

## Configuration options

Instagram Token Agent is designed to be configured using Heroku's web UI or CLI. It understands the following environment variables:

| Key  | Description  | Default  |
|---|---|---|
| `HIDE_HELP_PAGES` | Set to `true` to turn off the setup pages that aren't needed in production.  | none (Help pages display by default) |
| `ALLOWED_DOMAINS`  | White-list of the domains that can request the token via JS snippet or JSON object | none (any domain is allowed)  |
| `REFRESH_MODE`  | How should the refresh schedule work? Currently, only 'cron' is allowed, which refreshes on a set schedule | `cron`  |
| `REFRESH_FREQUENCY`  | How often should we refresh the token? Currently 'daily', 'weekly', 'monthly' are supported.  | `weekly`  |
| `JS_CONSTANT_NAME` | Set the name of the constant provided by the JS snippet  | `InstagramToken` |

## What are all the moving parts?

This app is designed to run using free plans and add-ons at Heroku, and be configurable via Heroku's interface, so you don't need to use the [CLI](https://devcenter.heroku.com/articles/heroku-cli) if you don't want to.

Here are the main parts and what they do:

 - **Heroku free dyno:** This serves the requests for tokens. Free dynos are limited in the amount of work they can do per month, but this should be ample for most sites just serving tokens.
 - **Temporize Scheduler:** This service schedules the app to refresh the token with Instagram to keep it working. Currently this happens once a week.
 - **Heroku Postgres:** The database that stores the token value
 - **MemCachier:** This caches the token payloads the agent sends out to keep things fast and take load off of the free dynos.

## Privacy considerations

Each installation of the agent is independent and self-contained. There's no 'phoning-home' or any other sending of data from your app to [Companion Studio]() or anyone else.

## To do

This is a first cut of this application, and there are still a lot of things to be done:

 - Add tests (!)
 - Improve documentation and setup instructions
 - Investigate a simpler scheduling/refresh solution
 - Work out a nice way to trigger the refresh tasks within the app or in heroku
 - ~~Add a switch to turn the 'hello world' pages off in production~~
 - ~~Make the domain whitelist actually do something~~
 - Add some mechanism whereby updating the starting token restarts the process


## License

MIT License

Copyright (c) 2020 Companion Studio

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
