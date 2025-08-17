# 💭 Linux Git Installation

1. Update your package lists:

    ```bash
    $ sudo apt update
    ```

2. Install Git using the apt package manager:

    ```bash
    $ sudo apt install git
    ```

3.  Verify the installation:

    ```bash
    $ git --version
    ```

4. Configure your global username and email, which will be associated with your Git commits (*recommended*):

    ```bash
    $ git config --global user.name "YOUR_USERNAME"
    $ git config --global user.email "YOUR_USER_EMAIL"
    ```

5. You can verify the configuration by listing all global settings:

    ```bash
    $ git config --list
    ```

### (Optional) GitHub Authentication with `.netrc`
1. Edit your `.netrc` file:

    ```bash
    $ sudo vim ~/.netrc
    ```

2. Add the following:

    ```bash
    machine github.com
      login YOUR_USERNAME
      password YOUR_PERSONAL_ACCESS_TOKEN
    ```

    Replace:
    - `YOUR_USERNAME` → your GitHub username
    - `YOUR_PERSONAL_ACCESS_TOKEN` → your generated PAT