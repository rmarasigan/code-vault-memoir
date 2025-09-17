# 💭 Linux Go Installation

1. Go to the official Go website and its '[All releases](https://go.dev/dl/)' page. Copy the link address of the Linux Go package.

2. Download the file (replace `xx.xx` with the latest version):

    ```bash
    $ wget https://go.dev/dl/go1.xx.xx.linux-amd64.tar.gz
    ```

3. Extract it to `/usr/local` using the following command:

    ```bash
    $ sudo tar -C /usr/local -xzf go1.xx.xx.linux-amd64.tar.gz
    ```

4. Update your shell config (`~/.bashrc`):

    ```bash
    $ sudo vim ~/.bashrc
    ```

    Add the following lines (adjust paths as needed):
    ```bash
    # If you have a mounted storage for Go projects, set GOPATH there.
    # Otherwise, default to $HOME/go.
    # export GOPATH=$HOME/go        # Fallback to home directory
    export GOPATH=/mnt/storage/go   # For mounted storage
    export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
    ```

5. Save the changes by sourcing the file using the following command:

    ```bash
    $ source ~/.bashrc
    ```

6. Verify installation:

    ```bash
    $ go version
    $ go env      # Optional: shows Go environment settings
    ```

### Updating Go version

1. Before updating, check the current Go version:

    ```bash
    $ go version
    go version go1.xx.xx linux/amd64

    $ which go
    /usr/local/go/bin/go
    ```

2. Remove existing Go installation:

    ```bash
    $ sudo rm -rf /usr/local/go
    ```

3. Download the latest Go version. Navigate to your **Downloads** directory (or any preferred location), then download the desired Go release.

    ```bash
    $ cd Downloads/
    :~/Downloads$ wget https://go.dev/dl/go1.xx.xx.linux-amd64.tar.gz
    ```

    Replace `go1.xx.xx` with the version you want to install (e.g., `go1.24.7`).

4. Install the new version. Extract the archive into `/usr/local/`:

    ```bash
    :~/Downloads$ sudo tar -C /usr/local -xzf go1.xx.xx.linux-amd64.tar.gz
    ```

6. Verify installation:

    ```bash
    $ go version
    $ go env      # Optional: shows Go environment settings
    ```

## 📋 Related Articles
* [How to Install Go on Ubuntu 22.04 | Step-by-Step](https://www.cherryservers.com/blog/install-go-ubuntu)