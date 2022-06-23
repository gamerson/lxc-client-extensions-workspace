# Client Extension Dev Experience Demo

Follow the instructions in the Setup/Install section to install all the tools / env needed for the Demo section.  

## Setup/Install Tools

1. Install JDK11 from here: https://www.azul.com/downloads/?version=java-11-lts&package=jdk

2. Install blade with this command:
   ```bash
   curl https://raw.githubusercontent.com/liferay/liferay-blade-cli/master/cli/installers/local -fsSL | bash
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



## LXC Deployment Demo

1. Go into client-extensions folder in the workspace
   ```bash
   cd client-extensions
   ```

2. Pick a project to build, e.g. `demoRemoteApps`
   ```bash
   cd demoRemoteApps
   ```

3. Build client extension zip.  This extension zip contains two remote apps (a customElement and iframe remote app)
   ```bash
   gw clean build
   ```

4. Deploy to extension environment 
   ```bash
   lcp deploy -r liferayperf.sh -p demo1ext-uat --extension dist/demoRemoteApps.zip
   ```

5. Go to the virtualInstance of DXP that is linked to this ext enviorment and go to the Remote Apps control panel and it should display the two deployed remote apps.  In this case the DXP instance is at: https://foox-uat.liferayperf.sh/
   **Go to Applications > Custom Apps > Remote Apps**

   | Name                   | Type              | Status     |
   | ---------------------- | ----------------- | ---------- |
   | Demo Custom Element    | Custom Element    | [Approved] |
   | Demo Theme Favicon     | Theme Favicon     | [Approved] |
   | Demo Global JavaScript | Global JavaScript | [Approved] |
   | Demo Global CSS        | Global CSS        | [Approved] |
   | Demo Theme CSS         | Theme CSS         | [Approved] |
   | Demo IFrame            | IFrame            | [Approved] |


6. On a widget page in DXP, go to widgets menu and add either remote app from the `Remote Apps` category to a page.



## Local Deployment Demo

1. Go to workspace folder, start docker daemon, then start DXP in a container and display the logs
   ```bash
   cd <workspace_dir>
   gw startDockerContainer logsDockerContainer
   ```

   ```bash
   > Task :startDockerContainer
   Starting container with ID 'lxc-client-extensions-demo-liferay'.
   
   > Task :logsDockerContainer
   Logs for container with ID 'lxc-client-extensions-demo-liferay'.
   [LIFERAY] To SSH into this container, run: "docker exec -it b5fc7c32d671 /bin/bash".
   
   [LIFERAY] Executing scripts in /usr/local/liferay/scripts/pre-configure:
   
   [LIFERAY] Executing 100_liferay_image_setup.sh.
   [LIFERAY] Copying /home/liferay/configs/local config files:
   
   /home/liferay/configs/local
   `-- portal-ext.properties
   ```

   

2. Any client extension projects already in the workspace wil be deployed to DXP and will be available in the UI. View them in the Control Panel > Applications > Custom Apps > Remote Apps

   1. | Name                | Type           | Status     |
      | ------------------- | -------------- | ---------- |
      | Demo Custom Element | Custom Element | [Approved] |
      | Demo IFrame         | IFrame         | [Approved] |

3. To deploy a project again after any change to client extension configuration or source code, go to the client extension project and run `gw deploy` command.
   ```bash
   cd client-extensions/demoGlobal
   gw deploy
   ```

   A few seconds later, the client extension will be redeploy in DXP

   ```bash
   2022-06-23 14:59:26.632 INFO  [fileinstall-directory-watcher][BundleStartStopLogger:80] STOPPED demoGlobal_1.0.0 [1649]
   2022-06-23 14:59:26.757 INFO  [Refresh Thread: Equinox Container: 0066fe5a-33d8-4771-8a51-68e3d0ccc696][BundleStartStopLogger:77] STARTED demoGlobal_1.0.0 [1649]
   
   ```

   

## Creating a new Client Extension Project

1. Go into client-extensions folder in the workspace
   ```bash
   cd <workspace_dir>/client-extensions
   ```

   

2. Start new client extension wizard with this command:
   ```bash
   blade create -t client-extension -d . newClientExt
   ```

   ```bash
   ✔ Metadata downloaded successfully
   ? Select an extension type: (Use arrow keys)
     customElement
   ❯ globalCSS
     globalJS
     iframe
     themeCSS
     themeFavicon
     themeJS
   ```

   ```bash
   ? Select an extension type: globalCSS
   ? What should be the extension name? MyGlobalCSS
   Successfully created project newClientExt in lxc-client-extensions-demo/client-extensions
   ```
   
   
   
3. Build the client extension project with gradle
   ```bash
   cd newClientExt
   gw build
   ```

3. The output zip is at `newClientExt/dist/newClientExt.zip`
   
5. Deploy to local or deploy to LXC accordingly.
   ```bash
   cd newClientExt
   gw deploy
   ```

   ```bash
   cd newCLientExt
   lcp deploy -r liferayperf.sh -p demo1ext-uat --extension dist/newClientExt.zip
   ```

   

