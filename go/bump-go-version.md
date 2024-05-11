# 💭 Bump Go Version

To update the Go version used in your project, follow these steps:

1. Modify the Go version

    ```bash
    dev@dev:~/your-project$ go mod edit -go=[your-go-version]
    ```

    <br />

    > **ℹ️ NOTE**
    >
    > Remember to replace `[your-go-version]` with your target Go version.

2. Update modules and the dependencies to ensure that all dependencies are up-to-date

    ```bash
    dev@dev:~/your-project$ go get -u -d ./...
    ```

3. After updating the modules and dependencies, tidy up your module

    ```bash
    dev@dev:~/your-project$ go mod tidy
    ```

### Project with a Private Repository

1. Modify the Go version

    ```bash
    dev@dev:~/your-project$ go mod edit -go=[your-go-version]
    ```

    <br />

    > **ℹ️ NOTE**
    >
    > Remember to replace `[your-go-version]` with your target Go version.

2. Set the private repository and ensure that you are authenticated to access them

    ```bash
    dev@dev:~/your-project$ go env -w GOPRIVATE=github.com/user/*
    ```

3. Update modules and the dependencies to ensure that all dependencies are up-to-date

    ```bash
    dev@dev:~/your-project$ go get -u -d ./...
    ```

4. After updating the modules and dependencies, tidy up your module

    ```bash
    dev@dev:~/your-project$ go mod tidy
    ```

5. Optional: Update your vendor dependencies if needed

    ```bash
    dev@dev:~/your-project$ go mod vendor
    ```

## 📋 Related Articles
- [How to upgrade the go version in a go mod](https://stackoverflow.com/questions/60675415/how-to-upgrade-the-go-version-in-a-go-mod)
- [go get results in 'terminal prompts disabled' error for github private repo](https://stackoverflow.com/questions/32232655/go-get-results-in-terminal-prompts-disabled-error-for-github-private-repo/60323360#60323360)