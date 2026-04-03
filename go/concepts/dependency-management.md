# Dependency Management
Dependency Management is the process of identifying, organizing, and resolving external code libraries and packages that a software project depends on.

* It ensures all required libraries are available and compatible with each other.
* Updates or changes to those dependencies are also managed and controlled.
* This prevents issues from conflicting versions or unexpected changes.

The most common dependency management steps are:
* Import the packages you want in your code.
* Add your code to a module for dependency tracking (if it isn't in a module already).
* Add external packages as dependencies so you can manage them.
* Upgrade or downgrade dependency versions as needed over time.

### Spaghetti Code
Spaghetti Code refers to programming code that is complex, tangled, and difficult to understand and maintain.

* It often results from haphazard and unstructured writing.
* This makes it difficult for other programmers to read, debug, and modify the code.
* Over time, it leads to unreliable, inefficient software prone to errors.

### Modular Code
Modular Code (or Structured Programming) uses a methodical and organized approach to writing software.

* Developers define requirements, create detailed designs, and break projects into smaller, manageable tasks.
* The goal is to make code more readable, maintainable, and scalable.
* This reduces errors and improves overall software quality.

## Dependencies
Dependencies are packages required for your project to run.

* A **direct dependency** is a package your code directly imports—its path appears in an import declaration in a `.go` source file.
* An **indirect dependency** (marked `// indirect`) is not directly used in your code but is required by your direct dependencies.

When choosing dependencies, consider:
* Maintenance activity (recent commits, issue resolution)
* Stability and release practices
* Community adoption
* Security track record

Popularity alone does not guarantee reliability.

## Environment Variables

### `GOROOT`
`GOROOT` is an environment variable that points to where your Go installation resides.

```bash
$ go env GOROOT
/usr/local/go
```

* You can check it with `go env GOROOT` (typically `/usr/local/go`).
* It tells Go tools where to find installed files they need to run properly.
* The Go standard library's source code resides there, specifically in the `src` directory.

> [!IMPORTANT]
> 
> `GOROOT` pertains only to Go's own tools and libraries—it is not tied to your specific projects or dependency management.

### `GOPATH`
`GOPATH` specifies where your Go dependencies and working projects live on your computer.

```bash
$ go env GOPATH
/home/dev/go
```

* Check it with `go env GOPATH` (default: `$HOME/go` on Unix, `%USERPROFILE%\go` on Windows since Go 1.8).
* In modern Go (module mode), your project **does not need to be inside `GOPATH`**.
* Within the `$GOPATH` directory, you'll find three important sub-directories:
	* **`bin`**: Compiled binaries go here when you run `go install`.
	* **`src`**: Source code of projects and dependencies.
	* **`pkg`**: Compiled package files, organized by import paths (includes `pkg/mod/` for module cache).

#### Structure
```text
$GOPATH
├── bin
│   └── (compiled binaries from go install)
├── pkg
│   └── mod
│       └── cache
│       │   └── (cache files used during module resolution)
│       └── sumdb
│           └── (cached checksum database state)
└── src
```

### `GOBIN`
`GOBIN` determines where binaries are placed.

* It depends on the `GOPATH` and `GOBIN` environment variables.
* If `GOBIN` is not set, binaries install to `$GOPATH/bin`.
* If neither is set, Go uses the default `GOPATH`'s `bin` directory.
* Not every package generates an executable binary—only those with a `main()` function do.

### `GOCACHE`
`GOCACHE` is where Go stores build outputs to speed up future builds.

```bash
$ go env GOCACHE
/home/dev/.cache/go-build
```

* Check it with `go env GOCACHE` (typically `~/.cache/go-build`).
* By default, cache data lives in a `go-build` directory within your OS-specific user cache directory.
* The `GOCACHE` variable can override this location, though it's rarely recommended.
* Go periodically deletes unused cached items to free disk space.
* You can manually clean caches with these commands:
	* `go clean -cache` deletes build cache data.
	* `go clean -testcache` deletes test cache results.
	* `go clean -fuzzcache` deletes the fuzzing testing data in that directory.
	* `go clean -modcache` deletes module cache data.

### `GO111MODULE`
`GO111MODULE` is an environment variable that controls Go module usage.

* Since Go 1.16, module-aware mode is effectively enabled by default.
* The possible values are:
	* **`on`**: Module-aware mode is always enabled.
	* **`off`**: Module-aware mode is disabled (uses `GOPATH` mode).
	* **`auto`**: Module-aware mode activates only if a `go.mod` file exists in the current or parent directory.

#### Module-Aware Mode
Module-aware mode is a feature that allows Go commands to manage dependencies using modules instead of `GOPATH`.

* It enables versioned dependency management and reproducible builds.
* It uses the `go.mod` file to define dependencies.
* It uses the `go.sum` file to verify dependency integrity.
* Dependencies are downloaded into the module cache (`$GOPATH/pkg/mod`).

## Go Module
A Go Module is a dependency management system that makes version information easier to manage.

```text
github.com/who/which
├── go.mod
├── main.go                  # Part of the module.
└── pkg/
    └── mypackage/
        └── mypackage.go     # Still part of the same module.
```

* It is a collection of related Go packages that are versioned and released together.
* A module is identified by a "module path" string, with requirements listed in a `go.mod` file.
* A module consists of Go packages in a file tree rooted at `go.mod` and can depend on other modules.
* A **package** is a collection of related `.go` files in one directory.
* A **module** is a collection of related packages versioned together.

### `go.mod`

```text
# go.mod
module modulename
...
# import package from this module
import "modulename/any/package"
```

The **`go.mod`** file defines the module's name, Go version, and dependencies (*direct* and *indirect*).
* It declares the module path, which serves as the base import path for all packages in the module.
* Each dependency appears as a module path plus semantic version.
* As long as sub-directories lack their own `go.mod`, they belong to the same module.

### `go.sum`

```text
github.com/user/repo v1.0.0 h1:checksum
github.com/user/repo v1.0.0/go.mod h1:checksum
```

The **`go.sum`** file contains cryptographic hashes of direct and indirect dependencies.
* It records checksums for both:
	* Module content
	* Module `go.mod` file
* The `h1:` prefix indicates a hash computed using Go’s module hashing algorithm.
* The hash is based on SHA-256 and encoded in base64.
* It ensures that downloaded dependencies have not been altered.

### Replacements
Replacements replace the code of a module with the code of another module using the `replace` directive.

* It can point to:
    * Local directories
    * Forked repositories
* The replacement module ideally has the same module path, but this is not strictly required.

**Excludes** prevent specific module versions from loading.

## Quick Start

### Initializing a new module
To initialize a new module, run `go mod init <module-name>`.

```bash
$ go mod init
$ go mod init github.com/user/repo
```

> [!NOTE]
>
> If no module path is provided, Go will try to infer one based on the current directory.

This command creates a `go.mod` file that defines the module and its dependencies.

```go
// Go import path where the module is hosted
// "module" is a reserved keyword
module github.com/user/repo

// Go version used to develop the module
go 1.23

// Defines the dependencies used by the module
require (
	github.com/dependency/one    v1.0.0
	
	// v2+ must include the major version in the module path
	github.com/dependency/two/v2 v2.3.0
	
	// "+incompatible" means the module does not follow Go module versioning
	github.com/dependency/legacy v2.0.0+incompatible
)

// Replace a module with another source (e.g., fork or local path)
replace github.com/dependency/one => github.com/fork/one

// Exclude specific versions from being selected
exclude github.com/dependency/two/v2 v2.0.0
exclude github.com/dependency/two/v2 v2.1.0
exclude github.com/dependency/two/v2 v2.2.0
```

### Add or Update Dependencies
The `go get` command is used to update module dependencies in the `go.mod` file.

> [!TIP]
> Since Go 1.17+, go get is primarily used for updating dependencies.

When you run `go get`, Go:
* Resolves the module and version
* Updates `go.mod` with the **minimum required version**
* Updates `go.sum` with checksums

```bash
# Add or update a dependency providing a package
$ go get github.com/user/project/package

# Use a specific version (tag)
$ go get github.com/user/project@v1.2.3

# Use latest matching minor version
$ go get github.com/user/project@v1.2

# Use a branch
$ go get github.com/user/project@master

# Use a commit hash
$ go get github.com/user/project@08c92af

# Use latest version
$ go get github.com/user/project@latest

# Upgrade all dependencies to latest minor/patch versions
$ go get -u ./...

# Add missing dependencies for current module
$ go get .
$ go get ./...
```

> [!NOTE]
>
> `go get ./...` ensures all imported packages are resolved and added to `go.mod`, but does not upgrade versions unless requested.

#### How Go Resolves Dependencies
Go uses **Minimal Version Selection (MVS)** to resolve dependency versions.
* Each module specifies a **minimum required version**
* Go selects the **highest version required across all dependencies**

👉 This means:
* It does **not** pick the latest version globally
* It picks the **lowest version that satisfies all requirements**, which results in the **highest required version in the graph**

Compatibility relies on **Semantic Versioning (Semver)**:
* Breaking changes → new **major version**
* Non-breaking changes → **minor/patch**

> [!TIP]
> 
> To include test dependencies:
> ```bash
>  go get -t ./...
> ```

### Organize and Clean Up
`go mod tidy` synchronizes `go.mod` and `go.sum` with your code.

```bash
$ go mod tidy
```

**What it does**:
1. Scans all packages in your module (recursively)
2. Adds missing dependencies required by imports
3. Removes unused dependencies
4. Updates `go.sum` with correct checksums
5. Adds indirect dependencies (`// indirect`) when needed

**Other Useful Commands**:
* **`go install`**: Builds and installs executables (packages with `main()`).

	```bash
	go install module@version
	```
    
* **`go mod init`**: Initializes a module in the current directory.
* **`go mod download`**: Downloads all dependencies into the module cache (commonly used in Docker builds for caching).
* **`go mod why`**: Explains why a dependency exists.

	```bash
	go mod why github.com/user/project
	```
    
* **`go mod vendor`**: Copies dependencies into a `vendor/` directory.
* **`go mod verify`**: Verifies dependencies against `go.sum`.
* **`go list -m all`**: Displays the final build list of selected module versions.

## Versioning & Git Tags

**Semantic Versioning** assigns version numbers a `vMAJOR.MINOR.PATCH`. **Breaking changes** are indicated by increasing the _major number (high risk)_; **new, non-breaking features** increment the _minor number (medium risk)_; and other **non-breaking changes** increment the _patch number (lowest risk)_.

* Increment `MAJOR` for incompatible API changes.
* Increment `MINOR` for backward-compatible features.
* Increment `PATCH` for bug fixes.

### Git Tags
Git tags mark specific points in history (releases).

```bash
$ git tag vMAJOR.MINOR.PATCH
```

They can represent:
* **Pre-release version (or release candidate)**
    * Considered to be ready and is made available for last minutes tests.
    * Not considered as a stable release.
    * Have specific tags with appended characters (e.g. `1.0.0-alpha`, `1.0.0-alpha.1`).
* **Untagged version** is a specific state of the program at a given time.
    * In the Git VCS, it is a "**commit**" and it will identify each commit with a SHA1 checksum (e.g. `409d7a2e37605dc5838794cebace764ea4022388`).

#### Creating Tags
* Ensure you're on the desired branch: `git branch --show-current`.
* Run `git tag v1.0.0` for a *lightweight tag* (just a pointer).
* Run `git tag v1.0.0 <commit>` to tag a specific commit.
* Run `git tag -a v1.0.0 -m "Release notes"` for an *annotated tag* with metadata.
* Push with:
    * All your local tags to the remote repository: `git push origin --tags`
    * Specific tag to the remote repository: `git push origin <tag-name>`

#### Lightweight Tags vs Annotated Tags
* **Lightweight Tags**
    * Simple pointer to a commit.
    * No metadata.
    * Creates a new tag checksum and store it in the `.git/` directory of the project's repository
    * **Syntax**
        ```bash
        # This will create a lightweight tag
        $ git tag <tag-name>
        ```
* **Annotated Tags**
    * Include author, date, message.
    * Can be signed and verified with GNU Privacy Guard (GPG).
    * Recommended for releases.
    * **Syntax**
        ```bash
        # This will create a new annotated tag
        $ git tag -a <tag-name>
        # This will create a new annotated tag with a message
        $ git tag -a <tag-name> -m "<tag message>"
        ```
    
#### Best Practices
* Consider Annotated tags as public, and Lightweight tags as private.
* Annotated tags are generally the better practices as they store additional valuable meta data about the tags.

#### Listing Tags
```bash
# List all tags
$ git tag

# Filter tags (e.g., release candidates)
$ git tag -l *-rc*
```

## Go Documentation
Use the built-in Go help system for command and module documentation.
```bash
$ go help
```

### Common Help Commands
* **`go help`**: Lists all available Go commands and topics.
* **`go help <command>`**: Displays detailed usage for a specific command.
    
	```bash
	$ go help mod  
	$ go help get
	```
    
* **`go help mod`**: Covers module-related operations and sub-commands.

## Private Package Dependencies
### Problem
Go cannot authenticate automatically when fetching private modules.

Example error:
```bash
$ go get github.com/your_github_username/mysecret
go get: module github.com/your_github_username/mysecret: 
fatal: could not read Username for 'https://github.com': terminal prompts disabled
```

#### Why This Happens
* Go delegates fetching to Git
* Git cannot prompt for credentials in non-interactive mode

### Solution: Provide Credentials
#### Option 1 — `.netrc` (Recommended for HTTPS)
Create or update your `~/.netrc` file:

```bash
$ nano ~/.netrc
```

Add:

```bash
machine github.com  
login your_github_username  
password your_personal_access_token
```

> [!NOTE]
>
> Use a **GitHub Personal Access Token (PAT)** instead of your password.

## Package Visibility
Visibility determines whether identifiers are accessible **outside their package**.

| Identifier Type           | Visibility           |
| ------------------------* | -------------------* |
| Starts with **uppercase** | Exported (public)    |
| Starts with **lowercase** | Unexported (private) |

**Example**

```go
package example

// Exported
func PublicFunction() {}

// Unexported
func privateFunction() {}
```


> [!TIP]
>
> Go does **not** use keywords like `public` or `private`. 
> 
> Visibility is entirely determined by **identifier casing**, which is enforced at compile time.

## Minimal Version Selection (MVS)
* A set of algorithms used under the hood by the `go` command to:
    * Generate the `go.mod` and `go.sum` files, which list all dependencies used by a project
    * Select consistent versions across all dependencies (including upgrades or downgrades when needed)

### Two Base Rules
* Each module provides a **list of module requirements**
    * Modules are identified by a **module path**
    * Each requirement specifies a **minimum version**
    * This list is defined in the `go.mod` file
* Each module should follow the **import compatibility rule**
    * Modules with the **same module path** must be **backward compatible**

### How to add a new dependency?
* By calling `go get`, it will:
    * Download the required module
    * Add or update the module in your `go.mod` file

### Which version is selected?
* MVS selects the **highest minimum required version** across the dependency graph
* NOT simply "latest available"
* In practice, this means:
    * If multiple modules require different versions of the same dependency,
    * Go will choose the **highest version among those requirements**

> [!IMPORTANT]
>
> MVS does **not** automatically upgrade to the latest version unless explicitly requested.

### Construction of the Build List
* The build list is the set of modules required to build a Go program
* Each entry consists of:
    * A **module path**
    * A **version** (tag, pseudo-version, or commit)
* Steps to construct the build list:
    * Initialize an empty list (`L`)
    * Add all modules required by the main module (`go.mod`)
    * For each module:
        * Read its `go.mod`
        * Append its dependencies to `L`
        * Repeat recursively
    * If multiple versions of the same module appear:
        * Keep the **highest version** (per MVS rules)
* To display the final build list:
    ```bash
    $ go list -m all
    ```

### How to upgrade a dependency to the latest minor or patch?
* When first added, Go selects a version (tagged, pre-release, or pseudo-version)
* To upgrade:
    ```bash
    # '-u' upgrades to newer minor or patch versions
    $ go get -u github.com/path/to/module
    ```

### Upgrade a dependency to a new major version
* When a module moves from `v1` to `v2+`, the module path changes
* To upgrade:
    ```bash
    $ go get github.com/path/to/module/v2
    ```

#### Upgrade All Modules to the Latest Version
* This upgrades all dependencies to newer minor/patch versions:
    ```bash
    $ go get -u ./...
    ```

### Upgrade/Downgrade a Dependency to a Specific Version
* You can target a specific version, tag, branch, or commit:
    ```bash
    # X can be a version, branch, or commit hash
    $ go get github.com/path/to/module@X
    ```

## 🔖 Extras
* **Go Docs**
	* [Tutorial: Create a Go module](https://go.dev/doc/tutorial/create-module)
	* [Using Go Modules](https://go.dev/blog/using-go-modules)
* **Digital Ocean**
	* [How to Use Go Modules](https://www.digitalocean.com/community/tutorials/how-to-use-go-modules)
	* [Understanding Package Visibility in Go](https://www.digitalocean.com/community/tutorials/understanding-package-visibility-in-go)
	* [How to Use a Private Go Module in Your Own Project](https://www.digitalocean.com/community/tutorials/how-to-use-a-private-go-module-in-your-own-project)
* **Devtrovert**
	* [GOROOT, GOPATH, GOCACHE](https://blog.devtrovert.com/p/goroot-gopath-go-get-go-mod-tidy)
	* [go get, go mod tidy commands](https://blog.devtrovert.com/p/go-get-go-mod-tidy-commands)
* [Git tag](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-tag)
* [Git Basics * Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
* [Semantic Versioning](https://semver.org/)
* [Software versioning](https://en.wikipedia.org/wiki/Software_versioning)
* [Managing dependencies](https://go.dev/doc/modules/managing-dependencies)
* [Chapter 17: Go modules](https://www.practical-go-lessons.com/chap-17-go-modules)
* [Go Modules Cheat Sheet](https://encore.cloud/guide/go.mod)
* [What does go mod tidy do in Go](https://golangbyexamples.com/go-mod-tidy/)
* [Dependency Management in Go](https://tech.goibibo.com/dependency-management-in-go-f29f52a0643b)