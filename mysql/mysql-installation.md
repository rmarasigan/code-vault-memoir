# 💭 Linux MySQL Installation

### MySQL Setup

1. Update package lists and upgrade existing packages (*recommended*):

    ```bash
    dev@dev:~$ sudo apt update
    dev@dev:~$ sudo apt upgrade
    ```

2. Install the MySQL Server:

    ```bash
    dev@dev:~$ sudo apt install mysql-server
    ```

3. Verify MySQL server version to confirm installation:

    ```bash
    dev@dev:~$ mysqld --version
    ```

4. Check MySQL service status (*and start if needed*):

    ```bash
    dev@dev:~$ sudo systemctl status mysql
    dev@dev:~$ sudo systemctl start mysql   # If not running
    ```

4. Secure your MySQL installation (*recommended*):

    ```bash
    dev@dev:~$ sudo mysql_secure_installation
    ```

    Securing includes setting a strong password, removing unnecessary accounts and databases, and restricting access to enhance overall security.

    * **Password Validation**

        If it skips setting a password and uses OS credentials instead:

          * The root user logs in without providing a password (`sudo mysql`). In this case, the authentication method `auth_socket` is used by default. This means root can log in without a password but only when connected locally through the root or `sudo` user.
          
          * External applications or GUI tools cannot connect as root with a password unless root's authentication is changed (not recommended).

    * **Remove Anonymous Users**

        It is advisable to remove this user for security reasons.

    * **Disallow Root Login Remotely**

        Limiting the root user's connection to the local machine (`localhost`) is advisable to mitigate potential security risks, such as credential brute-force attacks.

    * **Remove Test Database**

        Remove default insecure database.

    * **Reload Privilege Tables**

        Reloading the privilege tables is necessary to apply the changes made throughout the `mysql_secure_installation` process.

5. Create a dedicated MySQL User.

    ```bash
    # Log in as MySQL root user
    dev@dev:~$ sudo mysql

    # Create a new user with a password (replace `your_user` and `your_password`)
    mysql> CREATE USER 'your_user'@'localhost' IDENTIFIED BY 'your_password';

    # Grant full privileges to the new user.
    mysql> GRANT ALL PRIVILEGES ON *.* TO 'your_user'@'localhost' WITH GRANT OPTION;

    # Reload privilege tables.
    mysql> FLUSH PRIVILEGES;
    ```

    In this way, you avoid issues related to the root user's default socket authentication, and it's safer and clearer for development purposes.


### MySQL Workbench Setup

1. Install MySQL Workbench Community GUI client.

    ```bash
    dev@dev:~$ sudo snap install mysql-workbench-community
    mysql-workbench-community 8.0.36 from Tonin Bolzan (tonybolzan) installed
    ```

### Connecting with MySQL Workbench
1. Open MySQL Workbench
2. Create a new connection using:

    * Hostname: `localhost`
    * Port: `3306` (default MySQL port)
    * Username: `your_username`
    * Password: `your_password`

3. Test the connection and save.

## 📋 Related Articles
* [How to Install MySQL on Ubuntu 22.04](https://phoenixnap.com/kb/install-mysql-ubuntu-22-04)