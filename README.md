# godot-ios-upload
Github Action to export a Godot 3.x and 4.x game to iOS and upload it on App Store.  
If you are facing problems with the action or this README feels incomplete, pull requests are welcome or open an issue.

## Table of contents
- [godot-ios-upload](#godot-ios-upload)
  - [Table of contents](#table-of-contents)
  - [Godot 4.x](#godot-4x)
    - [Requirements](#requirements)
    - [Parameters](#parameters)
    - [How to use](#how-to-use)
  - [Godot 3.x](#godot-3x)
    - [Parameters](#parameters-1)
    - [Working examples](#working-examples)
    - [Parameters](#parameters-2)
    - [Working examples](#working-examples-1)
  - [App-specific password](#app-specific-password)
  - [Certificate Request without a Mac](#certificate-request-without-a-mac)
  - [Github Actions pricing](#github-actions-pricing)
  - [License](#license)

## Godot 4.x
Godot 4.x is the current active stable version and will be actively maintained.
You can find the code in the `main` branch.

### Requirements
 - Apple Developer Account
 - App-specific password
 - Godot Project with a iOS Export
 - Certificate and Provision Profile

### Parameters
| key | required | default | description |
| ----|----------|---------|-------------|
| project-name | true |  | Name used in xcodebuild |
| apple-id-username | true |   | Apple ID email |
| apple-id-password | true |   | App specific password |
| godot-version | true | . | Godot Engine version. Supported are 4.x versions. Check versions [here](https://github.com/godotengine/godot-builds/releases) |
| godot-channel | false | stable | Godot Engine release channel (stable, beta, rc1, rc2, rc3...). Defaults to 'stable' Check release channels [here](https://github.com/godotengine/godot-builds/releases) |
| working-directory | false | . | Path to .project file |


### How to use
Use the 4.x tag
```
- name: Export and upload to Testflight
  uses: dulvui/godot-ios-upload@v4
  with:
    project-name: FutsalManager
    godot-version: 4.2.2
    provision-profile-path: ~/Library/MobileDevice/Provisioning\ Profiles/build_pp.mobileprovision
    apple-id-username: ${{ secrets.IOS_APPLE_ID_USERNAME }}
    apple-id-password: ${{ secrets.IOS_APPLE_ID_PASSWORD }}
```

## Godot 3.x
Godot 3.x is the current LTS version and will be less actively maintained.
You can find the code in the `godot-3` branch.

### Parameters
| key | required | default | description |
| ----|----------|---------|-------------|
| project-name | true |  | Name used in xcodebuild |
| apple-id-username | true |   | Apple ID email |
| apple-id-password | true |   | App specific password |
| working-directory | false | . | Path to .project file |
| godot-version | false | 3.5.3 | Check versions [here](https://github.com/dulvui/godot-headless-mac/releases) |

### Working examples
You an find a working examples here:  
https://github.com/dulvui/pocket-broomball/blob/main/.github/workflows/upload-ios.yml  
https://github.com/dulvui/ball2box/blob/main/.github/workflows/upload-ios.yml


### Parameters
| key | required | default | description |
| ----|----------|---------|-------------|
| project-name | true |  | Name used in xcodebuild |
| apple-id-username | true |   | Apple ID email |
| apple-id-password | true |   | App specific password |
| working-directory | false | . | Path to .project file |
| godot-version | false | 4.2.1 | Check versions [here](https://downloads.tuxfamily.org/godotengine/) |

### Working examples
You an find a working examples here:  
https://github.com/dulvui/futsal-manager/blob/main/.github/workflows/upload-ios.yml


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

## Certificate Request without a Mac
To upload a game to the App Store, you need to create a Developer Certificate.
The official Apple guide shows you how to do it easily with a Mac device in its official [documentation](https://developer.apple.com/help/account/create-certificates/create-a-certificate-signing-request).

But it can be done also without a Mac, using a Linux System, following [this guide](https://gist.github.com/jcward/d08b33fc3e6c5f90c18437956e5ccc35) of a Github user.

Here you can find the needed steps, but since this might change over time, please always check the comments of the guide above.

1. Generate a private key and certificate signing request:
Change "info@simondalvai.org" and "Simon Dalvai" with your values.
```sh
openssl genrsa -out distribution.key 2048
openssl req -new -key distribution.key -out distribution.csr -subj '/emailAddress=info@simondalvai.org, CN=Simon Dalvai, C=IT'
```
2. Upload CSR to apple at: https://developer.apple.com/account/ios/certificate/create
Choose Production -> App Store and Ad Hoc
3. Download the resulting distribution.cer, and convert it to .pem format:
```sh
openssl x509 -inform der -in distribution.cer -out distribution.pem
```
4. Download Apple's Worldwide developer cert from portal and convert it to pem:  
   https://www.apple.com/certificateauthority/ - Worldwide Developer Relations - G4 (Expiring 12/10/2030 00:00:00 UTC

```sh
openssl x509 -in AppleWWDRCAG4.cer -inform DER -out AppleWWDRCAG4.pem -outform PEM
```
5. Convert your cert plus Apple's cert to p12 format (choose a password for the .p12):

Note: use -legacy if using opensssl v3.x https://stackoverflow.com/questions/70431528/mac-verification-failed-during-pkcs12-import-wrong-password-azure-devops
```sh
openssl pkcs12 -export -legacy -out distribution.p12 -inkey distribution.key -in distribution.pem -certfile AppleWWDRCAG4.pem 
```
Finally, update any provisioning profiles with the new cert, and download from dev portal.
6. Create base64 of distribution.p12 for github actions
Now you can prepare it for the Github Action
```sh
base64 distribution.p12 -w 0 > distribution.base64
```
7. Add distribution.base64 and the previous created p12 password to the Github Action secrets in your repository settings

## Github Actions pricing
Github Actions are always free only for Open Source repositories.
On private you get 2000 to 3000 run minutes per month, depending on the type of your account.
Additionally, MacOS actions consume 10x times the minutes of a Linux machine, so you actually get 200 to 300 minutes per month.

Here the detailed documentation about pricing https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions

## License
This software is licensed under the [MIT license](LICENSE).