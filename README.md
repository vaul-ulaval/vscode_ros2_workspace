# VSCode ROS2 Workspace Template

This template will get you set up using ROS2 with VSCode as your IDE.

See [how I develop with vscode and ros2](https://www.allisonthackston.com/articles/vscode_docker_ros2.html) for a more in-depth look on how to use this workspace.

## Features

### Style

ROS2-approved formatters are included in the IDE.  

* **c++** uncrustify; config from `ament_uncrustify`
* **python** autopep8; vscode settings consistent with the [style guide](https://index.ros.org/doc/ros2/Contributing/Code-Style-Language-Versions/)

### Tasks

There are many pre-defined tasks, see [`.vscode/tasks.json`](.vscode/tasks.json) for a complete listing.  Feel free to adjust them to suit your needs.  

Take a look at [how I develop using tasks](https://www.allisonthackston.com/articles/vscode_tasks.html) for an idea on how I use tasks in my development.

### Debugging

This template sets up debugging for python files and gdb for cpp programs.  See [`.vscode/launch.json`](.vscode/launch.json) for configuration details.

### Continuous Integration

The template also comes with basic continuous integration set up. See [`.github/workflows/ros.yaml`](/.github/workflows/ros.yaml).

To remove a linter just delete it's name from this line:

```yaml
      matrix:
          linter: [cppcheck, cpplint, uncrustify, lint_cmake, xmllint, flake8, pep257]
```

## How to use this template

### Prerequisites

You should already have Docker and VSCode with the remote containers plugin installed on your system.

* [docker](https://docs.docker.com/engine/install/)
* [vscode](https://code.visualstudio.com/)
* [vscode remote containers plugin](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

### Get the template

Click on `"use this template"`

![template_use](https://user-images.githubusercontent.com/6098197/91331899-43f23b80-e780-11ea-92c8-b4665ce126f1.png)

### Create your repository

On the next dialog, name the repository you would like to start.

![template_new](https://user-images.githubusercontent.com/6098197/91332035-713ee980-e780-11ea-81d3-13b170f568b0.png)

Github will then create a new repository with the contents of this one in your account.  It grabs the latest changes as `"initial commit"`.

### Clone your repo

Now you can clone your repo as normal:

![template_download](https://user-images.githubusercontent.com/6098197/91332342-e4e0f680-e780-11ea-9525-49b0afa0e4bb.png)

Then use `git submodule update --init --recursive` inside your repository to update the submodules repositories.

### Open it in vscode

Now that you've cloned your repo onto your computer, you can open it in VSCode (`File->Open Folder` or `code .`).

When you open it for the first time, you should see a little popup that asks you if you would like to open it in a container.  Say yes!

![template_vscode](https://user-images.githubusercontent.com/6098197/91332551-36898100-e781-11ea-9080-729964373719.png)

If you don't see the pop-up, click on the little green square in the bottom left corner, which should bring up the container dialog

![template_vscode_bottom](https://user-images.githubusercontent.com/6098197/91332638-5d47b780-e781-11ea-9fb6-4d134dbfc464.png)

In the dialog, select `"Remote Containers: Reopen in container"`

VSCode will build the dockerfile inside of `.devcontainer` for you.  If you open a terminal inside VSCode, you should see that your username has been changed to `ros`, and the bottom left green corner should say "Dev Container"

![template_container](https://user-images.githubusercontent.com/6098197/91332895-adbf1500-e781-11ea-8afc-7a22a5340d4a.png)

### Update the template with your code
1. Add more repositories
   1. `cd src`
   2. `git submodule add <repo_url>`
   3. `cd ..`
2. Install ROS dependencies
   1. Open the command palette (`Ctrl+Shift+P`)
   2. Type `Task`, then select `Run Task`
   3. Select `install dependencies`
3. Build your workspace
   1. Open the command palette (`Ctrl+Shift+P`)
   2. Type `Task`, then select `Run Task`
   3. Select `build`
4. Setup your workspace
   1. Open the command palette (`Ctrl+Shift+P`)
   2. Type `Task`, then select `Run Task`
   3. Select `setup`
5. Develop!

## FAQ

### WSL2

#### The gui doesn't show up

This is likely because the DISPLAY environment variable is not getting set properly.

1. Find out what your DISPLAY variable should be

      In your WSL2 Ubuntu instance

      ```
      echo $DISPLAY
      ```

2. Copy that value into the `.devcontainer/devcontainer.json` file

      ```jsonc
      	"containerEnv": {
		      "DISPLAY": ":0",
         }
      ```

#### I want to use vGPU

If you want to access the vGPU through WSL2, you'll need to add additional components to the `.devcontainer/devcontainer.json` file in accordance to [these directions](https://github.com/microsoft/wslg/blob/main/samples/container/Containers.md)

```jsonc
	"runArgs": [
		"--network=host",
		"--cap-add=SYS_PTRACE",
		"--security-opt=seccomp:unconfined",
		"--security-opt=apparmor:unconfined",
		"--volume=/tmp/.X11-unix:/tmp/.X11-unix",
		"--volume=/mnt/wslg:/mnt/wslg",
		"--volume=/usr/lib/wsl:/usr/lib/wsl",
		"--device=/dev/dxg",
      		"--gpus=all"
	],
	"containerEnv": {
		"DISPLAY": "${localEnv:DISPLAY}", // Needed for GUI try ":0" for windows
		"WAYLAND_DISPLAY": "${localEnv:WAYLAND_DISPLAY}",
		"XDG_RUNTIME_DIR": "${localEnv:XDG_RUNTIME_DIR}",
		"PULSE_SERVER": "${localEnv:PULSE_SERVER}",
		"LD_LIBRARY_PATH": "/usr/lib/wsl/lib",
		"LIBGL_ALWAYS_SOFTWARE": "1" // Needed for software rendering of opengl
	},
```
