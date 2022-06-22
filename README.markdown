# Client Extension Dev Experience Demo

Follow the instructions in the Setup/Install section to install all the tools / env needed for the Demo section.  

## Setup/Install

1. Install JDK11 from here: https://www.azul.com/downloads/?version=java-11-lts&package=jdk

2. Install blade with this command:
   ```bash
   curl https://raw.githubusercontent.com/liferay/liferay-blade-cli/master/cli/installers/global -fsSL | bash
   ```

3. Add blade to your path:
   **MacOS:**

   ```bash
   echo 'export PATH="$PATH:$HOME/Library/PackageManager/bin"' >> ~/.bash_profile
   source ~/.bash_profile
   ```

   Linux: 
   ```bash
   echo 'export PATH="$PATH:$HOME/jpm/bin"' >> ~/.bash_profile
   source ~/.bash_profile
   ```

4. Update to the latete blade snapshot
   ```bash
   blade update -s
   ```

   

5. Ensure that blade 4.1.0 snapshot is installed
   ```bash
   blade version
   ```

   It should return something like this:
   ```bash
   blade version 4.1.0.SNAPSHOT202206222041
   ```

6. Add alias `gw` which will be used to invoke gradle
   ```bash
   echo "" >> ~/.bashrc && echo "alias gw='blade gw'" >> ~/.bashrc
   ```

   

7. Clone this repository

   ```bash
   git clone git@github.com:gamerson/lxc-client-extensions-workspace.git
   ```

   

8. Install `lcp` cli:
   ```bash
   curl https://cdn.liferay.cloud/lcp/stable/latest/install.sh -fsSL | bash
   ```

9. Add Liferay cloud performance environment config
   ```bash
   lcp remote set liferayperf.sh liferayperf.sh
   ```

10. Login to liferayperf.sh remote
    ```bash
    lcp login
    ```



## Deployment Demo

1. Go into client-extensions folder
   ```bash
   cd client-extensions
   ```

   

2. Pick a project to build
   ```bash
   cd demoRemoteApps
   ```

   

3. Build client extension zip
   ```bash
   gw clean build
   ```

   

4. Deploy to extension environment 
   ```bash
   lcp deploy -r liferayperf.sh -p demo1ext-uat --extension dist/demoRemoteApps.zip
   ```

5. Go to the virtualInstance of DXP that is linked to this ext enviorment and go to the Remote Apps control panel and it should display the two deployed remote apps.  In this case the DXP instance is at: https://foox-uat.liferayperf.sh/
   **Go to Applications > Custom Apps > Remote Apps**

   | Name                | Type           | Status     |
   | ------------------- | -------------- | ---------- |
   | Demo Custom Element | Custom Element | [Approved] |
   | Demo IFrame         | IFrame         | [Approved] |

   

