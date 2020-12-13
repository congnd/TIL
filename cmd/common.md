# List all members of a group
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
```

And then unset the setting when you don't want to direct connections through the proxy anymore.
```
unset http_proxy
```

## Vim-mode on `tmux`

Add the following into your `~/.tmux.conf`
```
set-window-option -g mode-keys vi
bind-key -T copy-mode-vi 'v' send -X begin-selection
bind-key -T copy-mode-vi 'y' send -X copy-selection-and-cancel
```
