<div align=center>

# Creating a proper Open-Source Project with DevOps!


Tutorial on how to create a proper open-source project to support many platforms, powered by Rojo, GitHub actions, Wally, and more!



</div>

## Table of Contents

- [Creating a proper Open-Source Project with DevOps!](#creating-a-proper-open-source-project-with-devops)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Step 1: Setting up and installing Foreman](#step-1-setting-up-and-installing-foreman)
    - [Adding PATH - `*nix` systems.](#adding-path---nix-systems)
    - [Add PATH - Windows](#add-path---windows)
  - [Step 2 - Project Structure and Foreman Setup](#step-2---project-structure-and-foreman-setup)
  - [Step 3 - Setting up our Rojo project](#step-3---setting-up-our-rojo-project)
  - [Step 4 - Developing with Rojo](#step-4---developing-with-rojo)
  - [Step 5 - Setting up Wally (Optional)](#step-5---setting-up-wally-optional)
  - [Step 6 - Setting up StyLua and Selene](#step-6---setting-up-stylua-and-selene)
  - [Step 7 - Creating our DevOps workflow!](#step-7---creating-our-devops-workflow)
  - [Conclusion](#conclusion)


## Introduction 

I've created this tutorial because of how useful I found it in creating an open-source package that supports nearly every platform! 

As someone who loves to use 3rd party tools such as Rojo, one of the problems I've had is with people who want to use my package, but do not use Rojo or do not understand how to compile the code for themselves, so I strived to add support for adding an `.rbxm` file, so users can drag and drop. However, this complicated things. As someone who is passionate about DevOps, I like to automate my entire developmnt process.

And so, this tutorial exists!

## Step 1: Setting up and installing Foreman

**If you already have foreman installed, you can skip this step!**




Foreman is a toolchain manager for Roblox, similar to `rustup`. Because of the amount of tools that we may use, it is a good idea to install `Foreman` to make it easier to manage! 

First, make sure you have [Rust](https://www.rust-lang.org/) 1.53.0 or greater installed, as we will be installing Foreman from `cargo`. 

To install, simply run:

```
cargo install foreman
```

After this completes, you will need to add the `.bin` to your `PATH` variable.

### Adding PATH - `*nix` systems.

On `*nix` systems, you will need to open your enviromental variable file. On bash, run the following commands:

```
sudo nano ~/.bashrc
```

Once `nano` is opened, you will see a lot of items in `nano`.

Scroll all the way to the bottom, and add the following line:

```
export PATH="$HOME/.foreman/bin/wally/:$PATH"
```

Then, close and safe the file by using <kbd> CTRL </kbd> + <kbd> X </kbd>, then ensure you hit <kbd> Y </kbd> and then <kbd> ENTER </kbd>

Finally, restart your terminal or set it's source with:

```
source ~/.bashrc
```

### Add PATH - Windows

To add Foreman to your `PATH` variable on Windows, first open your search, and search for `Enviormental Variables`

After you do this, a panel will open. At the bottom, you will see `Enviormental Variables`

When the next UI opens, scroll down to `System Variables`

If you see a Variable called `PATH`, hit `Edit`. Otherwise, hit `New`

A new UI will open if you are editing, with a lot of other file paths. To the right, hit `New`, and you will have the option to add a variable to your `PATH`

Finally, hit `Browse Directory`, and look for where `cargo` installed foreman. Once found, browse to `.foreman/bin`, and select that. Hit `OK`, and you are done!

Make sure you restart all terminal instances that are open. 

To test, run `foreman --help`. A menu should come up! If one did not, repeat the steps above.

## Step 2 - Project Structure and Foreman Setup

Now that we have Foreman installed, it is time to setup our project. 

For our project structure, we will follow something similar to this:

![image](https://lh3.googleusercontent.com/KLZ8xgBuGMVSq5xon_dGMAaRVBa1pgm8pzwFfJsLwz_332qz75394TLrkSODniKr2l-fkeWJBKBoKIr4GiQyuckzP3i_KWb0xYhMSlTzuX5pOZdBhV-uEvEOeuYE-z0y67vSs7JzrqOE_9KpXULQrQRQa_XNVEw-lwfd8EIpNNHduK2M0ZRkPbIV8CzC8wkCO6QC9o0Cqa3FtQXHE9SqbQr6BweqG1Cj7n60_opiUN07S7VHlqbAtM6I1-JvgckYYvWc3B60rAx4kN8RXRjQ7MXmwlQC55OMsnlBOsNKGT7QA0i2pbpCsN28FEQV5OuglYiIJ0EVVud-h7xZx68jMne65DKYdywQOfGpmgOuFuP1pILbdQTmK_31Mf3nZQPLsPc2D3BXRBOoOe2ZTbDk79Cr-btSExWSOS2RSNwXRVbmX7NXtCZi9olrsipQbeXKtBzuKeYlDAtvjhvsJYOOG40bLm1QB8vQm14pngH2A2rZrcwqIvS20EjyckQI6dh0-94NsJeaawP2o2naEAMwZEoAQL6fNSVqDiw86_3Y8I480THrUeFjMmNKxzEiSyRbz2dLfVpV_vVq19wtYKd-rkAn_IKktu62Q2-CNrmX57nb5rwX83EIBrpNA-5jhXKBNA8vbmOwZWuBfc-MbNI5VTYqoLfgbMT2uCabymWrQgp0n5TnFFfXJVQ7eZS5rLkbTLuDfcOxacb5VTbl3Q6G_DZTAL0y-trgpYebGj6CaYCqbqmy3xn4fiwXeSE-=w890-h551-no?authuser=0)


We need to create our repository! 

First, create a new GitHub project, and make sure it's public. Add your preferred license. 

Next, clone your `git` repository and open it with your preferred text editor. I'll be use VSCode, but you can use whatever you want!

We are going to create a few folders in our directory:

* `.github/workflows`
* `Include`
* `lib`
* `testing`
* `Packages`

You'll also want a `README.md` file, and a `.gitignore` file. Make sure to add `Packages` to your `.gitignore` file!

Finally, we need to setup our tools, which will be installed via `Foreman`

Create a new file called `foreman.toml`.

For this tutorial, will we be using `Rojo`, `Wally`, `Stylua`, and `Selene`. There are many other tools available, but for the scope of this tutorial, this is it! Inside the file, write:

```toml
[tools]
rojo = { github = "rojo-rbx/rojo", version = "7.0.0" }
wally = { source = "UpliftGames/wally", version = "0.3.1" }
stylua = { source = "JohnnyMorganz/stylua", version = "0.13.1" }
selene = { source = "Kampfkarren/selene", version = "=0.9.2" }
```

After you've saved the file, you need to install the tools. Run:

```
foreman install
```

and wait for the tools to install. You can confirm they installed by running `foreman list`. Make sure all the tools listed above work by testing `Rojo`:

```
rojo --version
```

Just as listed above, you should see the following output:

```
Rojo 7.0.0
```

Congrats! You've now installed the tools we will use for this tutorial!

## Step 3 - Setting up our Rojo project

To being setting up our `rojo` project, run `rojo init`. 

This will add a `src` directory, a `default.project.json` file, and update your `.gitignore` file with some more information.

Begin by deleting the `src` directory, as we will be using the `lib` directory instead.

Next, we will update our `default.project.json` file, but before, I urge you to read and understand how `rojo` project's work. They have great documentation, I suggest reading this before continuing: 

https://rojo.space/docs/v7/project-format/

Our `default.project.json` is rarely going to be used, so replace the contents with the following:

```json
{
  "name": "[PROJECT_NAME_HERE]",
  "tree": {
    "$path": "lib"
  }
}
```

However, we need to add multiple other files here, as well. 

Create a `testing.project.json` and `pack.project.json`.

The `testing` file structure is how you can test your code in studio. I've found one issue I used to have with creating packages is the ability to test the code, so by doing this, we can easily test our own code.

The `pack` file will be used later on to pack our source code into a `.rbxm` file, so it can be dragged and dropped by a non-rojo/wally user. 

`testing.project.json`
```json
{
    "name": "[PROJECT_NAME] testing",
    "tree": {
      "$className": "DataModel",
      "ReplicatedStorage": {
        "$className": "ReplicatedStorage",
        "[PROJECT_NAME]": {
            "$path": "Include/Linking.lua",
            "Packages": {
                "$path": "Packages",
                "[PROJECT_NAME]": {
                    "$path": "lib"
                }        
              }
        
      }
       
      },
      "ServerScriptService": {
          "$className": "ServerScriptService",
          "Server": {
            "$path": "testing"
          }
      }
    }
    
  }
  ```

  `pack.project.json`
  ```json
  {
    "name": "[PROJECT_NAME]",
    "tree": {
      "$path": "Include/Linking.lua",
      "Packages": {
        "$path": "Packages",
        "[PROJECT_NAME]": {
            "$path": "lib"
        }        
      }
    }
  }
  ```

Great! That's all done! :D

Finally, let's set up the rest of our files. For my example, we are making a very simple module that prints out a name when given.

To begin, let's create our linking script. This is really important to ensure our project is packed correctly, especially if you add a dependency from `Wally`, as we need this dependency to get packed in our `.rbxm` file.

The structure looks like this:

`Include/Linking.lua`
```lua
return require(script.Packages["[PACKAGE_NAME]"])
```

In my example, I have the following:

```lua
return require(script.Packages["hello_world"])
```

Next, let's create our basic module! If you have your own module, this is the place to put it. Please ensure the main entry point into your module is named `init.lua`! This is extremely important! 

`init.lua`
```lua
local module = {}

function module.welcome(string) 

return ("Hello, " .. string .. "!") 

end

return module
```

Great! We now have a module setup, but we have no way of ensuring it works. Let's create a test script under the `testing` folder. In my case, I named it `script.server.lua`. 

`script.server.lua`
```lua
local hello_world = require(game.ReplicatedStorage.hello_world)

print(hello_world.welcome("world"))
```
By doing this, we will test to make sure our module works as intended! 

Congratulations, you've set up your `Rojo` project! In the next section, I'll explain how to work with Rojo in the development process.

## Step 4 - Developing with Rojo

For most people, developing without actively testing is not going to work well. To combat this, we will use Rojo's syncing feature, so you can develop in your text editor, while still seeing the update in your studio instance.

If you are on Windows, you can install the `Rojo 7` plugin via:
```
rojo plugin install
```

Otherwise, install the plugin via the Roblox marketplace. Please ensure you are using `Rojo 7`! Otherwise, it will not work.

Once installed into your studio, find it in your plugins bar and click on it.

To begin a live syncing session (in which your changes you make in your editor go right to studio) run the following command:

```
rojo serve testing.project.json
```

This will then start up a server on the `Rojo` designated port! By running this, you can easily test your changes in real-time in studio! Once you hit `Connect` in the plugin inside of studio, you can then run the game, and if your code works, everything is all good now! 

If you prefer to pack your code and dependencies into a `.rbxm` file, you can do this using the `rojo build` command.

First, add the following entry into your `.gitignore` file:

```
pack.rbxm
```

This will ensure you do not commit your built `.rbxm` direcly to your project! (We will handle this in the CI/CD pipeline.)

To build the file, run:

```
rojo build -o pack.rbxm pack.project.json
```

You can then drop that `.rbxm` into studio! Please note that does not add testing files, but the module and it's dependencies. The **only** way to test is by serving to studio!


## Step 5 - Setting up Wally (Optional)

If we want to publish our package to `Wally` (which you should do!), we need to initalize `Wally` in our project. Run:

```
wally init
``` 

in your terminal to create a `wally.toml` file.

Next, open the newly created file. The basic structure of `Wally` is as follows:

```toml
[package]
name = "fxllencode/devops-rblx-tutorial"
version = "0.1.0"
registry = "https://github.com/UpliftGames/wally-index"
realm = "shared"

[dependencies]
```

For more information and keys, check out the Wally documentation:

https://github.com/UpliftGames/wally#manifest-format

For this tutorial, set the `name` key to your GitHub name + your package name. This is how people will install your package. Then, edit the `SemVer` code to whatever version your package is on. Package format uses the `SemVer` standard, so numbers are `Major.Minor.Patch`. 

Do not edit the registry or realm as that is out of the scope of this tutorial. 

Once we have set this up, we need to setup our auth tokens for deployment to the registry. Begin with:

```
wally login
``` 

and follow the propmts given. Once this is complete, we need to get the token from the internal file. 

Navigate to your `Wally` installation (usually  `%userprofile%\.wally\auth.toml` on Windows or `~/.wally/auth.toml` on Linux) and open it using a text editor.

You should see the following:
```toml
# This is where Wally stores details for authenticating with registries.
# It can be updated using `wally login` and `wally logout`.

[tokens]
"https://api.wally.run/" = "[REDACTED]"
```

If you properly logged in, you should see a token inside the quotes. Copy that token, and open your GitHub repository on the website. 

Go to your repository settings -> secrets -> actions -> new repository secret.


Create a new secret with the name `WALLY_AUTH_TOKEN` and a value of what you copied, and hit "Add secret". That's it! Do not login or logout of Wally after you have done that. We will handle publishing in our CI/CD workflow. 

## Step 6 - Setting up StyLua and Selene

When creating a codebase, code styling is very important. We want to ensure that your code is readable and does not contain flaws regardless of who contributes to the code or not. This is where `Selene` and `StyLua` come into play! `Selene` is a rust-based linter for Luau, that can help detect codebase issues. `StyLua` will format your Lua code to ensure the style is the same around the board. 

First, let's setup `StyLua`. To ensure it was installed via `Foreman`, run:

```
stylua --version
```

in the terminal.

You should see:

```
stylua 0.13.1
```

Next, let's format our module! Keep in mind that we will not be formatting our `testing` foldler as it is meant for testing, and people will not be downloading it, so it's style does not matter.

To style your module, run:

```
stylua lib
```

Your code is now formatted according to the `StyLua` standards! Great! Next, let's lint our code and check it for errors. To start, we need to ensure we have `selene` installed. Run the following commmand:

```
selene --version
```

Your output should be:

```
selene 0.9.2
```

Next, we need to generate the Roblox-std library.

```
selene generate-roblox-std
```

This should create a new file, `roblox.toml`. Finally, create a new file called `selene.toml`, and add the following contents:

```toml
std = "roblox"
```

Finally, let's lint our library:

```
selene lib
```

If there is no output, we are good! However, if you see an output, you should follow it's suggestion to improve your code.

We now have fully setup linting and style guides!

## Step 7 - Creating our DevOps workflow!

Now that we have the basis for our module, we should start automating things! 

We will have one workflow, our CI workflow. This workflow has 2 parts, one runs on commit to main branch, and one runs on the dev branch. (You should never commit to main until the code is production ready as this will deploy it to Wally and create a release!)

To start, enter your `.github/workflows` folder and create a new file called `CI.yaml`. This workflow will be the heart of our DevOps workflow! 

An example workflow is below:
`CI.yaml`
```yaml
name: Releases

on: push

jobs: 

  deploy:
    if: ${{ github.ref == 'refs/heads/main' }}
    runs-on: ubuntu-latest


    steps:
      - name: Checkout Main
        uses: actions/checkout@v3

      - name: Setup Foreman
        uses: Roblox/setup-foreman@v1
        with: 
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Install Foreman Toolchains
        run: foreman install

      - name: Install Dependencies
        run: wally install

      - name: Create Packages Directory
        run: mkdir -p Packages

      - name: Run Stylua
        run: stylua lib --check

      - name: Run Selene
        run : selene lib

      - name: Build pack.rbxm
        run: rojo build -o pack.rbxm pack.project.json

      - name: Upload pack.rbxm as build artifact
        uses: actions/upload-artifact@v3
        with: 
          name: hello_world
          path: pack.rbxm

      - name: Get Release from wally.toml
        uses: SebRollen/toml-action@v1.0.0
        id: read_toml
        with: 
          file: 'wally.toml'
          field: 'package.version'


      - name: Publish to Wally
        env: 
          WALLY_TOKEN: ${{ secrets.WALLY_AUTH_TOKEN }}
        run: |
          mkdir =p ~/.wally
          printf "[tokens]\n\"https://api.wally.run/\" = \"%s\"" "$WALLY_TOKEN" >> ~/.wally/auth.toml
          wally publish

      - name: Release
        uses: softprops/action-gh-release@v1
        with: 
          name: ${{ steps.read_toml.outputs.value }}
          tag_name: ${{ steps.read_toml.outputs.value }}
          files: pack.rbxm
          generate_release_notes: true
          draft: true

  development: 
    if: ${{ github.ref == 'refs/heads/dev' }}
    runs-on: ubuntu-latest

    steps:
      - name: Checkout development
        uses: actions/checkout@v3

      - name: Setup Foreman
        uses: Roblox/setup-foreman@v1
        with: 
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Install Foreman Toolchains
        run: foreman install

      - name: Run Stylua
        run: stylua lib --check

      - name: Run Selene
        run : selene lib

      - name: Install Dependencies
        run: wally install

      - name: Create Packages Directory
        run: mkdir -p Packages

      - name: Build test-pack.rbxm
        run: rojo build -o test-pack.rbxm pack.project.json

      - name: Build testing place
        run: rojo build -o testing.rbxl testing.project.json

      - name: Upload test-pack.rbxm as build artifact
        uses: actions/upload-artifact@v3
        with: 
          name: hello_world
          path: test-pack.rbxm

      - name: Upload testing.rbxl as build artifact
        uses: actions/upload-artifact@v3
        with: 
          name: hello_world
          path: testing.rbxl
```

Whew! That is a lot! In essence, our code follows this pattern: 

Checkout code -> Setup Foreman -> Install Tools -> Install Wally Dependencies -> Styling and Lints -> Pack -> Publish to Wally -> Create Release

For in-development progress, it does not publish, instead creates lints, styling, and example places, so developers can simply download the build artifacts! 

Once this workflow in the `main` branch passes, head to the `Releases` area in GitHub, and you will see a draft release, in which most information is already filled out for you, but you can edit the changelog if you wish. When ready, hit release, and now people can download it either from `wally` or from the release directly! 

## Conclusion

Congratulations! You've just setup a fully-working DevOps workflow for your open-source package! I hope you found this useful!

Here are some tips:

* Make sure you develop on the `dev` branch, and pull request your updates into `main` when it is production ready.
* Before you push into `main`, don't forget to bump your package's version in `wally.toml`
* Feel free to add even more tools, like `testez`, etc.
  


You can view the example project here:

https://github.com/FxllenCode/devops-rblx-tutorial

Thanks! If you have any questions, please let me know. :D