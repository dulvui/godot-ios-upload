# godot-ios-upload
Github Action to export a Godot game to iOS and upload it on App Store.  
It uses https://github.com/huskeee/godot-headless-mac to do the export.  
If you are facing problems with the action or this README feels uncomplete, pull requests are welcome or open an issue.

## Table of contents
- [godot-ios-upload](#godot-ios-upload)
  - [Table of contents](#table-of-contents)
  - [Requirements](#requirements)
  - [Parameters](#parameters)
  - [Working examples](#working-examples)
  - [License](#license)


## Requirements
 - Apple Developer Account
 - Godot Project with a iOS Export
 - Certificate and Provision Profile

Note: Its recommended to create a Apple Developer service account to use a separate user/password to upload.

## Parameters
| key | required | default | description |
| ----|----------|---------|-------------|
| project-name | true |  | Name used in xcodebuild |
| apple-id-username | true |   | Service account username |
| apple-id-password | true |   | Service account password |
| working-directory | false | . | Path to .project file |
| godot-version | false | 3.5 | Check versions [here](https://github.com/huskeee/godot-headless-mac/releases) |
| cache-version | false | 1 | Headless godot gets cached, on problems increase version |

## Working examples
You an find a working examples here:  
https://github.com/dulvui/pocket-broomball/blob/main/.github/workflows/upload-ios.yml

## License
This software is licensed under the [MIT license](LICENSE).