# heroku-buildpack-headless-chrome

To have headless chrome work correctly on Heroku, particularly
for use with Heroku CI, you need to have
both Chromium & Chromedriver installed, as well as Xvfb or you
will see errors every time you try to send keys.  In order to
have a fully functional headless chrome install ready to
use with for example Selenium, you just need to add
heroku-buildpack-apt and this buildpack to your
environment.test.buildpacks config.


This buildpack:

1. patches Xvfb to work correctly given the location of the
heroku-buildpack-apt installed files (/app/.apt/usr/bin).
2. creates a shim for chrome which adds the `--headless --no-sandbox --disable-gpu` flags by default.
3. installs the latest version of [Chromedriver](https://sites.google.com/a/chromium.org/chromedriver/)


## Installation
1. [heroku-buildpack-apt](https://github.com/heroku/heroku-buildpack-apt) should be added before this buildpack
2. [heroku-buildpack-google-chrome](https://github.com/heroku/heroku-buildpack-google-chrome)
3. An `Aptfile` in your root dir with the following contents

        Xvfb

## Usage

### Aptfile

Create or amend a file called Aptfile in your project root with the following content:

    xvfb libxss1 xfonts-100dpi xfonts-75dpi xfonts-cyrillic xorg dbus-x11
    https://dl.google.com/linux/direct/google-chrome-beta_current_amd64.deb

### app.json

Your app.json should list the buildpacks in this order:

    {
        "environments": {
            "test": {
                "buildpacks": [
                    {"url": "https://github.com/heroku/heroku-buildpack-apt"},
                    {"url": "https://github.com/cordery/heroku-buildpack-headless-chrome"},
                    ... # rest of your buildpacks here
                }
            }
        }
    }

## Troubleshooting
#### I keep getting the following error: `an X display is required for keycode conversions, consider using Xvf`
Xvfb is not properly installed.  If this buildpack is correctly installed
you will see "Patching Xvfb" during the build process.

If you see that "Patching Xvfb" and are still getting the
above error then most likely another buildpack is
reinstalling apt-get debs into the ./.apt/usr/bin dir
and overwriting the changes made by this buildpack to Xvfb.

For example, including heroku-buildpack-google-chrome alongside
heroku-buildpack-apt will cause this.

If you need both buildpacks and
cannot resolve this behavior by eliminating the non-heroku-buildpack-apt pack, then just make sure
this buildpack (heroku-buildpack-headless-chrome) comes after every other buildpack.

## License

MIT