
# NOTE: This fork is likely outdated by now. Please refer to the original nativefier repo.

# Nativefier Fork with working Windows 10 Notifications

## Introduction

Enables notifications for Windows. It has only been tested on Windows 10.
This is my first GitHub fork, so please let me know if there's anything I can improve.
Everyone is welcome to improve this fork, specially regarding the TODO section below.

## Fork differences

- Adds the AppUserModelId option required by Windows to display notifications. This parameter is passed using the --app-user-model-id flag
- Adds an event listener to the notification, so when it's clicked, it shows/focus the app window
- Adds an exe app (/bin/ShortcutCreator.exe) that creates a shortcut in the user's Start Menu directory. Launching the app from the shortcut created by this app is necessary for notifications to work.

## Set Up
- Clone the project:
```bash
git clone https://github.com/rdcvnh/nativefier.git
cd nativefier
```
- Build the app:
```bash
npm run dev-up-win
```
- Link the app:
```bash
npm link
```

## Usage
1. The usage is identical to the original nativiefier project, however, to get notifications to work you need to pass the **--app-user-model-id** parameter when nativefying an app. This can be anything you like, such as *Gmail.Work*, *Nativefier.Gmail*, etc...

    The parameter value should contain less than 128 characters, be in the camelCase style and contain no special characters or spaces. You may separate words with a period

    Keep in mind you need to use the same AppUserModelId parameter when creating a shortcut in the step below. Also, if more than one app has the same parameter value, they will be grouped together in the Windows taskbar (this could be useful for multiple Gmail accounts)

    For example:
    ```bash
    nativefier --name 'Notification' http://www.bennish.net/web-notifications.html --app-user-model-id 'Nativefier.Notification'
    ```
2. After nativefying an app, you need to create a shortcut in your Start Menu directory with the AppUserModelId parameter value chosen above. Since Windows doesn't allow you to directly set this property, we need to use the ShortcutCreator command line app located in the /bin folder:
    - Go to the directory where you cloned the project and then enter:
      ```bash
      cd bin
      ```
    - Call the ShortcutCreator.exe from the command line passing the parameters required to the create the shortcut:
  
      **ShortcutCreator.exe 'shortcut file name' 'target file absolute path' 'AppUserModelID'**
    
      Example:
      ```bash
      ShortcutCreator.exe 'Notification' 'C:\apps\Notification-win32-x64\Notification.exe' 'Nativefier.Notification'
      ```

    The shortcut created can be found at: *%appdata%\Microsoft\Windows\Start Menu\Programs*
    
    It should also show up in your Windows Start Menu

3. Launch the app via the shortcut created

For more information on how to use Nativefier, please refer to the original project documentation.
  

## TODO

- Integrate the ShortcutCreator.exe app to the build process when nativefying an app so the second step is not required. I have not been able to do this yet. I suspect it needs to be added to the src/buildMain.js file. If anyone is able to do this, please let me know.

## Credits
- [Nativefier by jiahaog ](https://github.com/jiahaog/nativefier)
- This fork is based on the answers given by some GitHub users on the following issue: https://github.com/jiahaog/nativefier/issues/88 (@JiDoan et al.)
- The ShortcutCreator.exe program uses the following piece of code: https://emoacht.wordpress.com/2012/11/14/csharp-appusermodelid/
- Rodrigo Cavanha (me)
