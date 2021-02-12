## Git LFS

### Installation
```sh
$ brew install git-lfs
$ git lfs install
```

### Track files
```sh
# Don't forget the double quotes
$ git lfs track "*.psd" 
```
After that you can use `git push` as normal push push changes to remote.

### Skip download LFS files on clone, pull, checkout
```
export GIT_LFS_SKIP_SMUDGE=true
```
Then when you want to pull LFS files, you can use `git lfs pull` to pull all current lfs files for current state.

If you prefer to pull invidual files, use `git lfs pull` with options `-I` or `--include` 
with the provided list of files is written in comma-separeated format.

```sh
git lfs pull -I "path/to/file1,path/to/file2"
```

### Mirroring a git repo 
```
$ git clone --mirror git@example.com/upstream-repository.git
$ cd upstream-repository.git
$ git push --mirror git@example.com/new-location.git
```
