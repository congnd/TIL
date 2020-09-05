## rbenv - Multiple Ruby's versions manager

Source: https://stackoverflow.com/a/54977206/3867033

### Installing `rbenv`

```
$ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
```

### Add `rbenv` into your `PATH` and init rbenv when you login
```
// Add these lines into your bash config file
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
```

Then don't forget to reload your configs 
```
$ source path/to/your/config/file
```

### Veriry installation

```
$ rbenv
rbenv 1.1.1-39-g59785f6
Usage: rbenv <command> [<args>]

Some useful rbenv commands are:
   commands    List all available rbenv commands
   local       Set or show the local application-specific Ruby version
   global      Set or show the global Ruby version
   shell       Set or show the shell-specific Ruby version
   rehash      Rehash rbenv shims (run this after installing executables)
   version     Show the current Ruby version and its origin
   versions    List all Ruby versions available to rbenv
   which       Display the full path to an executable
   whence      List all Ruby versions that contain the given executable

See `rbenv help <command>' for information on a specific command.
For full documentation, see: https://github.com/rbenv/rbenv#readme
```

### Installing `ruby-build`
```
$ mkdir -p "$(rbenv root)"/plugins
$ git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
```

### Installing `Ruby`
```
rbenv install -f -v 1.2.3 // Your desired Ruby version
```
### Veriry installed versions
```

$ rbenv versions
  system
* 2.5.3 (set by /home/ubuntu/.rbenv/version)
```

### Choose your default version
```
echo "1.2.3" > .ruby-version
```
