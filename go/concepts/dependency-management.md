- The process of identifying, organizing, and resolving the external code libraries and packages that a software application or project depends upon
- It is important because it ensures that all required libraries and packages are available and compatible with each other and that any updates or changes to those dependencies are managed and controlled to prevent issues that could arise due to conflicting versions or changes
- The most common dependency management steps:
    - Import the packages you want in your code
    - Add your code to a module for dependency tracking (if it isn’t in a module already)
    - Add external packages as dependencies so you can manage them
    - Upgrade or downgrade dependency versions as needed over time

## Modular Code
- **Spaghetti Code**
    - A programming code that is complex, tangled, and difficult to understand and maintain
    - Often used to describe code that has been written in a haphazard and unstructured manner, making it difficult for other programmers to understand and modify the code
    - It can lead to code that is difficult to read, debug, and maintain over time, which can result in software that is unreliable, inefficient, and prone to errors
- **Modular Code / Structured Programming**
    - **Structured approach**
        - A methodical and organized approach to writing software and it involves using best practices such as defining requirements, creating a detailed design, and breaking down the project into smaller, manageable tasks
        - Its goal is to make the code more readable, maintainable, and scalable, which helps to reduce errors and improve software quality
    - **Modular code**
        - It refers to a programming technique where a large program is divided into smaller, independent modules or components
        - Each module performs a specific task, and these modules are designed to work together to achieve the overall functionality of the program
        - It can be reused in other projects, saving time and effort in future software development

## Dependencies
- Are packages that are required for your project(s) to run
- **Direct Dependency**
    - A dependency that your code directly imports
    - It is a package whose path appears in an `import` declaration in a `.go` source file
- **Indirect Dependency (`// indirect`)**
    - A dependency not directly used in your code and used by your direct dependencies
- The code written by a dynamic community might be efficiently maintained
    - An important criterion when choosing a dependency is to check the community’s dynamism around the project
    - A trendy project with many contributions might be “safer” than another maintained by a couple of developers

## Go Module
- A dependency management system in Go that makes dependency version information and easier to manage
- It is identified by a string which is called the “module path” and the requirements (if any) of the module are listed in a specific file
- A module is a collection of Go packages stored in a file tree with a `go.mod` file at its root and it can be dependent on other modules
- **`go.mod`**
    - It defines the module’s module path and its dependency requirements (`module github.com/your_github_username/repo`)
    - Each dependency requirement is written as a module path and a specific semantic version
- **`go.sum`**
    - It will contain cryptographic hashes of the module _direct_ and _indirect_ dependencies
    - `go.sum` will record the checksum of the module version used, but also the checksum of the `go.mod` file of the module; that’s why we have two lines per module
        ```
        github.com/your_github_username/repo v1.0.0 h1:sIEfKULMonD3L9COiI2GyGN7Sdz
        github.com/your_github_username/repo v1.0.0/go.mod h1:+IP28RhAFM6FlBl5iSYC
        ```
    - The hashing algorithm used is SHA256:
        - First checksum is the hash of all the files of the module
        - Second checksum is the hash of the `go.mod` file of the module
    - The `h1` string is fixed, it means that the `Hash1` function inside of the go library is used
        

**Replacement**
- Replace the code of a module with the code of another module with the directive '`replace`'
- It can be:
    - Stored **locally**
    - Stored on a **code sharing website** (e.g. Github, Gitlab, etc.)
- Replacement module should have the **same module directive** (the first line of the `go.mod` file)
- The replacement version depends on the location of the replacement
    - Local: Not required
    - Distant (Github, Gitlab): Required

### Quick Start
#### Initializing a new module
```bash
$ go mod init
# You can supply module path manually
$ go mod init github.com/your_github_username/repo
```

This command will create a `go.mod` file that defines the project's requirements and locks dependencies to their correct versions.

```go
// Go import path where the module is hosted
// **module** is a reserved keyword
module github.com/your_github_username/repo

// Go version used to develop the module
go 1.20

// Defines the dependencies that are used by the module
require (
	github.com/dependency/one    v1.0.0
	
	// v2 and later have the major version in the module path
	github.com/dependency/two/v2 v2.3.0
	
	// "**incompatible**" means the package hasn't been migrated to Go modules yet
	// or violate its versioning guidelines
	github.com/dependency/legacy v2.0.0+incompatible
)

// Replace the module with the 'github.com/fork/one'
replace github.com/dependency/one => github.com/fork/one

// Exclude to prevent a specific version(s) of a module from being loaded
github.com/dependency/two/v2 v2.0.0
github.com/dependency/two/v2 v2.1.0
github.com/dependency/two/v2 v2.2.0
```

#### Add or Update Dependencies
```bash
$ go get github.com/path/to/module

# Using Git Tags
$ go get github.com/path/to/module@v4.0.1

# Using Git branch name
$ go get github.com/path/to/module@master

# Using Git commit hash
$ go get github.com/path/to/module@08c92af
```

This will add a new dependency or update a dependency to your project.

#### Organize and clean up
```bash
$ go mod tidy
```

Depending on the current state of your repository, this will either prune the unused module or remove `// indirect` comment. The purpose of `go mod tidy` is to also add any dependencies needed for other combinations of OS, architecture, and build tags.

#### Go Documentation
Using the `go help` command will provide information about other Go commands. You can use it to get more details about a specific command by running `go help <command>`. To get a list of all available Go commands, you can run `go help` without any arguments.

```bash
$ go help
Go is a tool for managing Go source code.

Usage:

	go <command> [arguments]

The commands are:

	bug         start a bug report
	build       compile packages and dependencies
	clean       remove object files and cached files
	doc         show documentation for package or symbol
	env         print Go environment information
	fix         update packages to use new APIs
	fmt         gofmt (reformat) package sources
	generate    generate Go files by processing source
	get         add dependencies to current module and install them
	install     compile and install packages and dependencies
	list        list packages or modules
	mod         module maintenance
	work        workspace maintenance
	run         compile and run Go program
	test        test packages
	tool        run specified go tool
	version     print Go version
	vet         report likely mistakes in packages

Use "go help <command>" for more information about a command.

Additional help topics:

	buildconstraint build constraints
	buildmode       build modes
	c               calling between Go and C
	cache           build and test caching
	environment     environment variables
	filetype        file types
	go.mod          the go.mod file
	gopath          GOPATH environment variable
	goproxy         module proxy protocol
	importpath      import path syntax
	modules         modules, module versions, and more
	module-auth     module authentication using go.sum
	packages        package lists and patterns
	private         configuration for downloading non-public code
	testflag        testing flags
	testfunc        testing functions
	vcs             controlling version control with GOVCS

Use "go help <topic>" for more information about that topic.
```

### Private Package Dependencies
- **Issue**: Go doesn’t have the credentials to download it
- To use a private module, **you’ll need to have access to a private Go module**
- If you try to `go get` your private module into another module, you’ll likely see an error similar to:
    ```bash
    $ go get github.com/your_github_username/mysecret
    go get: module github.com/your_github_username/mysecret: git ls-remote -q origin in /Users/your_github_username/go/pkg/mod/cache/vcs/2f8c...b9ea: exit status 128:
    	fatal: could not read Username for '<https://github.com>': terminal prompts disabled
    Confirm the import path was entered correctly.
    If this is a private repository, see <https://golang.org/doc/faq#git_https> for additional information.
    ```
    
    Go is calling Git for you and can’t prompt them. At this point, to access your module, you’ll need to provide a way for Git to retrieve your credentials without your immediate input
    
- **Providing Private Module Credentials**
    - **For HTTPS: `.netrc` file**
        - The `.netrc` file contains various hostnames as well as login credentials for those hosts
        - To create a `.netrc` file and entry on Linux, MacOS or WSL (Windows Subsystem for Linux):
            ```bash
            # Open the .netrc file in your home directory
            $ nano ~/.netrc
            
            # Create a new entry in the file
            machine github.com                # the value should be the hostname
            login your_github_username
            password your_github_personal_access_token
            ```

### Package Visibility
- Visibility in this context means the file spaces from which a package or other construct can be referenced
- Go determines if an item is exported or unexported through how it is declared
    - Exporting an item makes it visible outside the current package
    - If it is not exported, it is only visible and usable from within the package it was defined
- The package visibility is **determined by the capitalization of identifiers**
    - Identifiers that _start with a capital letter are exported_ and can be accessed from outside the package
    - Those that _start with a lowercase letter are unexported_ and are only visible within the package they are declared in

### Version Tag `git commit`
- Tagging is generally used to capture a point in history that is used for a marked version release (e.g. `v1.0.1`)
- A tag is like a branch that doesn’t change
- It can designate a **released version** or a **pre-released version** of the software
    - **Pre-release version (or release candidate)**
        - Considered to be ready and is made available for last minutes tests
        - Not considered as a stable release
        - Have specific tags with appended characters (e.g. `1.0.0-alpha`, `1.0.0-alpha.1`)
- **Untagged version** is a specific state of the program at a given time
    - In the Git VCS, it is a “**commit**” and it will identify each commit with a SHA1 checksum (e.g. `409d7a2e37605dc5838794cebace764ea4022388`)

#### Creating a tag
- **Syntax**
    ```bash
    # Replace the <tag-name> with a semantic identifier
    $ git tag <tag-name>
    ```
- Tagging a version in Git
    - Ensure you’re in the branch you want to tag
        ```bash
        # Check in which branch you are in
        $ git branch --show-current
        # Switch to a branch that you want
        $ git switch <branch-name>
        ```
    - Decide on a version number for your tag (e.g. `v1.0.0`)
    - Run the `git tag` command
        ```bash
        $ git tag v1.0.0
        ```
    - You can use the `-a` option to create an annotated tag
        ```bash
        # It will include a message that describes the tag and information on who
        # created the tag and when it was created
        $ git tag -a <tag-name> -m <message>
        ```
    - You can also tag a specific commit
        ```bash
        # It will create a tag at the specified commit
        $ git tag <tag-name> <commit>
        ```
    - Push your tags to the remote repository
        ```bash
        # Pushing all your local tags to the remote repository
        $ git push origin --tags
        # Pushing specific tag to the remote repository
        $ git push origin <tag-name>
        ```

#### Lightweight Tags vs Annotated Tags
- **Lightweight Tags**
    - Are essentially ‘bookmarks’ to a commit, they are just a name and a pointer to a commit, useful for creating quick links to relevant commits
    - Creates a new tag checksum and store it in the `.git/` directory of the project’s repository
    - **Syntax**
        ```bash
        # This will create a lightweight tag
        $ git tag <tag-name>
        ```
- **Annotated Tags**
    - Stores extra meta data such as: tagger name, email, and date
    - Can be signed and verified with GNU Privacy Guard (GPG)
    - **Syntax**
        ```bash
        # This will create a new annotated tag
        $ git tag -a <tag-name>
        # This will create a new annotated tag with a message
        $ git tag -a <tag-name> -m "<tag message>"
        ```
    
#### Best Practices
- Consider Annotated tags as public, and Lightweight tags as private
- Annotated tags are generally the better practices as they store additional valuable meta data about the tags

#### Listing Tags
```bash
# List stored tags in a repository
$ git tag
# Refined list of tags
# This will return a list of all tags marked with a **-rc** prefix
$ git tag -l *-rc*
```

### Semantic Versioning
**Software versioning** is the process of assigning either unique version names or unique version numbers to unique states of computer software. **Breaking changes** are indicated by increasing the _major number (high risk)_; **new, non-breaking features** increment the _minor number (medium risk)_; and ll other **non-breaking changes** increment the _patch number (lowest risk)_.

```bash
$ git tag **vMAJOR.MINOR.PATCH**
```

1. **`MAJOR`** version when you make incompatible API changes
2. **`MINOR`** version when you add functionality in a backward compatible manner
3. **`PATCH`** version when you make backward compatible bug fixes

### Minimal Version Selection (MVS)
- A set of algorithms used under the hood by the `go` command line to:
    - Generate the `go.mod` file and the `go.sum`, that lists all dependencies used by a project
    - Update or downgrade one or several dependencies

#### Two Base Rules
- Each module will give a **list of module requirements**
    - Modules are identified by a **module path**
    - Each module required to specify a **minimal compatible version**
    - This list is in the `go.mod` file
- Each module should follow the **import compatibility rule**
    - Modules with the **same module path** should be **backward compatible**

#### How to add a new dependency?
- By calling `go get` it will:
    - Download the module required
    - Add the module required to your `go.mod` file

#### Which version is selected?
- Go will choose **by order of preference**
    - Latest tagged stable relese
    - Latest tagged pre-relese
    - Latest untagged version (latest known commit, also called latest _pseudo-version_)

#### Construction of the Build List
- Build list is the list of modules necessary to build a Go Program
- Each item of this list is composed of two things
    - Module path identifies a module
    - Revision identifier (which can be a tag or a commit id)
- The steps required create the build list for a given module are:
    - Initialize an empty list (`L`)
    - Take the list of modules required for the current module (`go.mod`)
    - For each module required:
        - Get the list of modules required by this module (`go.mod`)
        - Append those elements to the list (`L`)
        - Repeat the operation for elements appended to the list
    - In the end, the list may contain multiple entries for the same module path
        - If so, for each module path, keep the newest version
- To display the final build list of a module, you can use the command: `go list -m all`

#### How to upgrade a dependency to the latest minor or path?
- The first time you add a dependency to your project, Go will download a specific version: Tagged version, Tagged pre-release or Specific commit
- When you want to upgrade a dependency to its next version:
    ```bash
    # The '-u' flag will download the newer minor or patch
    $ go get -u github.com/path/to/module
    ```
    
#### Upgrade a dependency to a new major version
- When a module switch from `v0` or `v1` to `v2`, it should modify its path to comply with the import compatibility rule
- To specifically require the major version, you:
    ```bash
    $ go get -u github.com/path/to/module/v2
    ```
    

#### Upgrade All Modules to the Latest Version
- Because of the import compatibility rule, no breaking changes should be introduced (this operation only requests new patches and minor versions)
- Concretely to do so, you will type the following command:
    ```bash
    # This will output the upgraded dependencies
    $ go get -u ./...
    ```
    
#### Upgrade/Downgrade a Dependency to a Specific Version
- To target a specific version, you can also use the `go get` command:
    ```bash
    # The **X** can be a commit hash (b822ebd) or a version (v1.0.3)
    $ go get github.com/path/to/module/@**X**
    ```
    
### `go mod vendor`
- It will create a **vendor** folder with all the sources of your dependencies

### `go mod verify`
- It will check your locally stored dependencies
- This check is very useful to ensure that you are using the correct version of your dependencies and not an altered version

## 🔖 Extras
- [Managing dependencies](https://go.dev/doc/modules/managing-dependencies)
- [Dependency Management in Go](https://tech.goibibo.com/dependency-management-in-go-f29f52a0643b)
- [How to Use Go Modules](https://www.digitalocean.com/community/tutorials/how-to-use-go-modules)
- [Go Modules Cheat Sheet](https://encore.cloud/guide/go.mod)
- [Understanding Package Visibility in Go](https://www.digitalocean.com/community/tutorials/understanding-package-visibility-in-go)
- [How to Use a Private Go Module in Your Own Project](https://www.digitalocean.com/community/tutorials/how-to-use-a-private-go-module-in-your-own-project)
- [Git tag](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-tag)
- [Git Basics - Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
- [Semantic Versioning](https://semver.org/)
- [Software versioning](https://en.wikipedia.org/wiki/Software_versioning)
- [Chapter 17: Go modules](https://www.practical-go-lessons.com/chap-17-go-modules)