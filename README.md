# AlpNode Workspace - Tiny Alpine Pro Environments

![TYD](images/ttyd.png)

Inspired by [Bluxmit's Alnoda Workspaces](https://github.com/bluxmit/alnoda-workspaces). I wanted to build the tiniest possible nodejs cloud workspace. Trimmed the fat by reducing redundant and/or unneeded programs, apps, users and runtimes. However, still with an intuitive and _comfortable_ workflow.

Alnoda (Bluxmit) Ubuntu/base workspaces average ~1-3GB after the container build step _(...still a really **awesome** project)_. **AlpNode** base workspace is ~300MB after build. Dive analysis shows an image efficiency score of 96%.

![Dive](images/dive.png)

## Base Workspace includes:

- Zsh, Oh my Zsh
- Node
- Python3, pip
- curl, wget
- Git: git, git-flow, lazygit
- File browsers: mc, [Filebrowser](https://github.com/filebrowser/filebrowser)
- vim
- htop
- supervisord
- cron
- dmidecode (Must run with `--privileged`)

![FileBrowser](images/filebrowser.png)

All CLI programs accessable via [Web Terminal](https://github.com/tsl0922/ttyd)

![HTOP](images/htop.png)

## Runtime

```
docker run --rm -p 8046:8026 -p 8041:8021 bresnow/alpnode-wrkspce:base -n space
```

## Additional Packages

If you wanted to install additional packages and build your own image based upon this one you'd start your Dockerfile like this:

```
FROM bresnow/alpnode-wrkspce:base

RUN apk -add U -v package_name package_name

...
```

## More/ TODOs

I will be adding more and updating the repo regularly.

- [ ] Traefik reverse proxy with compose/swarm config.
- [ ] IDE workspaces / Codespaces alternative
- [ ] Customization config setup to deploy white-label apps
