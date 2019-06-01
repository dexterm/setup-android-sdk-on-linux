# HOW TO SETUP ANDROID SDK MANAGER ON LINUX WITHOUT DOWNLOADING ANDROID studio


### Install JAVA (any provider, openjdk or oracle java)

### Download SDK Manager 
point your browser to https://developer.android.com/studio/index.html#download and scroll down to Command line tools only Download the sdk depending on your platform (Windows, Max, Linux) or directly download the linux version

          $ wget https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip

### Extract the downloaded zip file to a location of your choice, preferrably $HOME folder
        - $ mkdir -p $HOME/Android
        - $ unzip sdk-tools-linux-4333796.zip -d $HOME/Android
        The files will be extracted to your home folder/Android

##4. Add the SDK folder to PATH environment variable
     - export PATH=$HOME/Android/tools:$HOME/Android/tools/bin:$PATH        
     - Optionally you can add the path to .bashrc
     1. Open the `.bashrc` file in your home directory (for example, `/home/your-user-name/.bashrc`) in a text editor.
     2. Add `export PATH="your-dir:$PATH"` to the last line of the file, where *your-dir* is the directory you want to add.
     3. Save the `.bashrc` file.
     4. Restart your terminal.