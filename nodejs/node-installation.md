# 💭 Linux Node.js Installation

> [!NOTE]
>
> NVM stands for "Node Version Manager", a script that lets you manage several Node.js versions on your system.

1. Download and install `nvm`:

    ```bash
    $ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
    ```

    The installer version (`v0.40.3`) will eventually be outdated. Check the [nvm releases](https://github.com/nvm-sh/nvm/releases) page for the latest installer.

2. Restart the shell:

    ```bash
    $ source ~/.bashrc
    ```

3. Install Node.js (LTS version):

    ```bash
    $ nvm install 24        # Or use: nvm install --lts
    $ nvm alias default 24  # Optional: set as default Node.js
    ```

4. Verify the Node.js version:

    ```bash
    $ node -v
    $ nvm current
    
    # You can list installed Node.js versions:
    $ nvm ls

    # Switch to a specific version:
    $ nvm use <version_number>
    ```

5. Verify npm version:

    ```bash
    $ npm -v
    ```

### (Optional) Install AWS CDK

1. Install CDK globally (inside your user environment, not with `sudo`):

    ```bash
    $ npm install -g aws-cdk
    ```

2. Verify the AWS CDK version:

    ```bash
    $ cdk --version
    ```

## 📋 Related Articles
* [Download Node.js](https://nodejs.org/en/download/current)
* [AWS CDK Toolkit](https://catalog.us-east-1.prod.workshops.aws/workshops/10141411-0192-4021-afa8-2436f3c66bd8/en-US/20-prerequisites/70-toolkit)