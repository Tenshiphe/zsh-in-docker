# Oh My Zsh Installer for Docker

This is a script to automate [Oh My Zsh](https://ohmyz.sh/) installation in development containers.
Works with any images based on Alpine, Ubuntu, Debian, CentOS or Amazon Linux.

The original goal was to simplify setting up `zsh` and Oh My Zsh in a Docker image but it can be used
in any case you need a simple way to install Oh My Zsh and its plugins in a Docker image.

## Usage

One line installation: add the following line in your `Dockerfile`:

```Dockerfile
# Default agnoster theme, no plugins installed
RUN sh -c "$(curl https://raw.githubusercontent.com/tenshiphe/zsh-in-docker/master/zsh-in-docker.sh)"
```

#### Optional arguments:

- `-t <theme>` - Selects the theme to be used. Options are available
  [here](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes). By default the script installs
  and uses agnoster.
- `-p <plugin>` - Specifies a plugin to be configured in the generated `.zshrc`. List of bundled
  Oh My Zsh plugins are available [here](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins).
  If `<plugin>` is a url, the script will try to install the plugin using `git clone`.

#### Examples:

```Dockerfile
# Uses "robbyrussell" theme (original Oh My Zsh theme), with no plugins
RUN sh -c "$(curl https://raw.githubusercontent.com/tenshiphe/zsh-in-docker/master/zsh-in-docker.sh)" -- \
    -t robbyrussell
```

```Dockerfile
# Uses "git" and "ssh-agent" bundled plugins
RUN sh -c "$(curl https://raw.githubusercontent.com/tenshiphe/zsh-in-docker/master/zsh-in-docker.sh)" -- \
    -p git -p ssh-agent
```

```Dockerfile
# Uses "ys" theme, uses some bundled plugins and install some more from github
RUN sh -c "$(curl https://raw.githubusercontent.com/tenshiphe/zsh-in-docker/master/zsh-in-docker.sh)" -- \
    -t ys \
    -p git \
    -p ssh-agent \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions
```

## Notes

- The examples above use `curl`, but if you prefer `wget`, just replace `curl` with `wget -O-`
- This scripts requires `git` and `curl` to work properly. If your `Dockerfile` uses `root` as the
  main user, it should be fine, as the script will install them automatically. If you are using a
  non-root user, make sure to install the `sudo` package _OR_ to install `git` and `curl` packages
  _before_ calling this script
- By default this script install the `powerlevel10k` theme. If you want the default Oh My Zsh theme, uses the option
  `-t robbyrussell`

## Source
This repository is a fork of https://github.com/deluan/zsh-in-docker
