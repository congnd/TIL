## Find & Count lines
```shell
find . \( -path ./Pods -o -path ./Carthage -o -path ./.git \) -prune -o -name "*.swift" -print0 | xargs -0 wc -l
```

Brief explanation:
- `find`: `find` command
- `.`: in current directory
- `( -path ./Pods -o -path ./Carthage -o -path ./.git )`: matches all paths that contain `/Pods` or `/Carthage` or `./.git`
  - `-path ./Pods`: matches all paths that contain `/Pods`
  - `-o`: the `or` operator
  - don't forget to put spaces before and after the `\)`/`\(`
  - don't forget to put slash before the `(` and `)` to escape the interpreter
- `-prune`: skip descending into the current path
- `-o`: again, the `or` operator
- `-name "*.swift"`: matches all paths that contain `.swift` in the last path component
- `-print0`: print the pathname followd by a character code `0` instead of new line
- `xargs -0`: turns the output of the prev command into input args of the following command. `-0` expects the character code `0` as the separator instead of new lines or spaces
- `wc -l`: count number of lines in the input paths thoes are passed by `xargs`


## List all members of a group
```bash
dscacheutil -q group -a name admin
```

Learn more about `dscacheutil`
```bash
man dscacheutil
```

## Prevents MacOS from sleeping
Use `caffeinate`

```
caffeinate -d
```

Learn more about `caffeinate`
```
man caffeinate
```

## Setting proxy when using commandline
```
export http_proxy="http://localhost:8888"
export https_proxy="http://localhost:8888"
```

And then unset the setting when you don't want to direct connections through the proxy anymore.
```
unset http_proxy
unset https_proxy
```

## Setting proxy for NPM
```
$ npm config set proxy http://localhost:3128
$ npm config set https-proxy http://localhost:3128
```

And unset
```
$ npm config delete http-proxy
$ npm config delete https-proxy
```

## Vim-mode on `tmux`

Add the following into your `~/.tmux.conf`
```
set-window-option -g mode-keys vi
bind-key -T copy-mode-vi 'v' send -X begin-selection
bind-key -T copy-mode-vi 'y' send -X copy-selection-and-cancel
```

## Find all files larger than a specific size
```
find . -type f -size +50M

find . -type f -size +1G
```

## Get directory/file size
```
du -sh PATH
```
