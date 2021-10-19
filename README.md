# Veeam Best Practice Guide

## Jekyll Plugins
The following Jekyll plug-ins are currently enabled and can be used

* [jekyll-redirect-from](https://github.com/jekyll/jekyll-redirect-from) for redirects to other pages (e.g. 301 for moved pages)

## Local Preview Build

The BP is built upon the [Jekyll](https://jekyllrb.com/) and the template
[just-the-docs](https://github.com/pmarsceill/just-the-docs) for
[Github Pages](https://pages.github.com/).

You can use your local ruby installation to run a local development server which renders the page
as it is rendered on Github Pages.

To do this

1.  Install [Ruby for Windows](https://rubyinstaller.org/) (v2.6 - currently some libs are not
    compatible with 2.7)

2.  Install the Ruby packaging tool _bundler_

    `gem install bundler`

3.  Install the necessary gems for this project (go to the project's root folder of your working
    copy):

    `bundler install`

4.  Now you can run the local development server on http://localhost:4000 with

    `bundler exec jekyll serve`

    Changing any file with trigger an automatic rebuild of the complete site which takes ~ 40s.
    You can improve this process by ordering incremental builds, which will just rebuild the changed
    files

    `bundler exec jekyll serve --incremental`

    which takes only ~3 seconds.

## Local Preview - Docker container running on Windows

Docker Jekyll container can be used to render the pages locally without installing Ruby on Windows.

To deploy container use this Powershell command:

`cd "to the book repository root"`

`docker run --rm -e "JEKYLL_ENV=docker" -v ${PWD}:/srv/jekyll -p 4000:4000 jekyll/jekyll jekyll serve --config _config.yml,_config.docker.yml`

Pages will be available at http://localhost:4000

### Docker command break down

| Switch                                               | Note                                            |
|------------------------------------------------------|-------------------------------------------------|
| --rm                                                 | remove any other Jekyll container               |
| -e                                                   | pass the environment variables to the container |
| -v                                                   | map the repository into /srv/jekyll             |
| jekyll/jekyll                                        | the container name                              |
| jekyll serve --config _config.yml,_config.docker.yml | the command to issue at the startup             |

### Stopping the container

Take note of the container ID running:

`docker ps -a`


And stop it with:

`docker rm ID -f`
