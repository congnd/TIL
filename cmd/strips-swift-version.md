A simple script that strips swift version from the result of the `swift --version` command.

```sh
#!/bin/bash

swift_version_desc=$(swift --version)
swift_version_stripped=$(echo $swift_version_desc | egrep -o ' (\d+\.)?(\d+\.)?(\*|\d+) ')
swift_version=${swift_version_stripped//' '/''}

echo $swift_version
```

Examples: 
- `5.3` for the Swift version 5.3
- `5.3.1` fror the Swift version 5.3.1
