# Fastlane tips

## `match` - Codesiging is easier than ever
Refer: https://docs.fastlane.tools/actions/match/

- Initialize match, select `git` as persistance method. (use an SSH url to rely on SSH as an authentication method)
```
fastlane match init
```

- Setup match for development
```
fastlane match development
```

Then follow the instruction.

### If you don't want to use SSH as the authentication method

Don't just give `match` your raw `personal access token` to use as basic authentication's token.
You have to provide to `match` the base64 encoded `personal aceess token` and your Github username in form of:
```
GITHUB_USERNAME:PERSONAL_ACCESS_TOKEN
```
or simply using this in your `Matchfile`:
```
git_basic_authorization Base64.strict_encode64("GITHUB_USERNAME:PERSONAL_ACCESS_TOKEN")
```

### Sets the `force_for_new_devices` to `true` to ensure that the provisioning always be synced

### It's better to provide to `match` the user name and email which you prefer use in Git commits
```
fastlane run match type:adhoc git_full_name:"YOUR_NAME" git_user_email:"YOUR_EMAIL"
```
Be sure that you already put the `run` in the middle of `fastlane` and `match` :D 
