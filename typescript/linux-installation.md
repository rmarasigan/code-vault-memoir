# 💭 Linux Installation

1. Ensure you have Node.js installed globally on your machine.

    ```bash
    dev@dev:~$ node --version
    ```

    * If Node.js is not installed on your machine, download and import the Nodesource GPG key.

      ```bash
      dev@dev:~$ sudo apt-get update
      dev@dev:~$ curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
      ```

    * Create a `deb` repository.

      ```bash
      dev@dev:~$ export NODE_MAJOR=20
      dev@dev:~$ echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
      ```

      <br />

      > **ℹ️ NOTE**
      >
      > `NODE_MAJOR` can be changed depending on the version you need.
      >
      > * `NODE_MAJOR=16`
      > * `NODE_MAJOR=18`
      > * `NODE_MAJOR=20`

    * Run update and install

      ```bash
      dev@dev:~$ sudo apt-get update
      dev@dev:~$ sudo apt-get install nodejs -y
      ```

2. Install the TypeScript compiler globally on your machine.

    ```bash
    dev@dev:~$ npm i -g typescript
    ```

3. Check if the installation is successful.

    ```bash
    dev@dev:~$ tsc -v
    ```

## 📋 Related Articles
- [NodeSource Node.js Binary Distributions](https://github.com/nodesource/distributions)