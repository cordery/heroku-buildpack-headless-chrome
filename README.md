# heroku-buildpack-headless-chrome

To have headless chrome work correctly on Heroku, particularly for use with Heroku CI, you need to have
both Chromium & Chromedriver installed, as well as Xvfb or you
will see errors every time you try to send keys.

This buildpack patches Xvfb to work correctly given the location of the
heroku-buildpack-apt installed files (/app/.apt/usr/bin) and installs
the latest version of [Chromedriver](https://sites.google.com/a/chromium.org/chromedriver/)


## Requires
1. [heroku-buildpack-apt](https://github.com/heroku/heroku-buildpack-apt) should be added before this buildpack
2. [heroku-buildpack-google-chrome](https://github.com/heroku/heroku-buildpack-google-chrome)
3. An `Aptfile` in your root dir with the following contents

        Xvfb

## Usage

Add this buildpack after heroku-buildpack-apt

Your app.json should list the buildpacks in this order

    {
        "environments": {
            "test": {
                "buildpacks": [
                    {
                      "url": "https://github.com/heroku/heroku-buildpack-apt"
                    },
                    {
                      "url": "https://github.com/captain401/heroku-buildpack-xvfb"
                    },
                    {
                      "url": "https://github.com/heroku/heroku-buildpack-google-chrome"
                    }
                    ...
                }
            }
        }
    }


## License

MIT