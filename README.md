# godot-ios-upload
Github Action to export a Godot game to iOS and upload it on App Store.  
It uses https://github.com/huskeee/godot-headless-mac to do the export.  
If you are facing problems with the action or this README feels uncomplete, pull requests are welcome or open an issue.

## Table of contents
- [godot-ios-upload](#godot-ios-upload)
  - [Table of contents](#table-of-contents)
  - [Requirements](#requirements)
  - [App-specific password](#app-specific-password)
  - [Parameters](#parameters)
  - [Working examples](#working-examples)
  - [License](#license)


## Requirements
 - Apple Developer Account
 - App-specific password
 - Godot Project with a iOS Export
 - Certificate and Provision Profile


## App-specific password
To upload the app you can use your account login credentials.  
But for security reasons it is not recommended to do that, so prefer to use an app-specific password, by following this steps:
1. Sign in to appleid.apple.com.
2. In the Sign-In and Security section, select App-Specific Passwords.
3. Select Generate an app-specific password or select the Add button, then follow the steps on your screen.
4. Enter or paste the app-specific to the Action secrets.

Find here the official instructions on how to create an app-specific password:
[support.apple.com/en-us/HT204397](https://support.apple.com/en-us/HT204397)

Note: An API key generated from [appstoreconnect.apple.com/access/api](https://appstoreconnect.apple.com/access/api) site would also work, but is not implemented yet. Open an issue, if you need to use an API key and I'll implement it asap.


## Parameters
| key | required | default | description |
| ----|----------|---------|-------------|
| project-name | true |  | Name used in xcodebuild |
| apple-id-username | true |   | Apple ID email |
| apple-id-password | true |   | App specific password |
| working-directory | false | . | Path to .project file |
| godot-version | false | 3.5.2 | Check versions [here](https://github.com/huskeee/godot-headless-mac/releases) |
| cache-version | false | 1 | Headless godot gets cached, on problems increase version |

## Working examples
You an find a working examples here:  
https://github.com/dulvui/pocket-broomball/blob/main/.github/workflows/upload-ios.yml

## License
This software is licensed under the [MIT license](LICENSE).