# 💭 Linux MySQL 8.0.35 Reset Password

1. Stop the MySQL server process.

    ```bash
    dev@dev:~$ sudo service mysql stop
    ```

2. Start the MySQL (`mysqld`) server/daemon process with the `--skip-grant-tables` option.

    ```bash
    dev@dev:~$ sudo mysqld_safe --skip-grant-tables &
    ```

    Adding the `&` at the end runs the command in the background, allowing you to use the terminal for the next steps.

3. Connect to the MySQL server as the `root` user.

    ```bash
    dev@dev:~$ mysql -u root
    ```

4. Reset the `root` password inside the MySQL shell.

    ```bash
    mysql> UPDATE mysql.user SET authentication_string=null WHERE User='root';
    mysql> FLUSH PRIVILEGES;
    mysql> exit;
    ```

5. Restart the MySQL server normally.

    ```bash
    dev@dev:~$ sudo service mysql stop
    dev@dev:~$ sudo service mysql start
    ```

6. Connect again as the `root` user and set the new password.

    ```bash
    dev@dev:~$ mysql -u root
    mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'your_password';
    mysql> exit;
    ```

5. Test the new password.

    ```bash
    dev@dev:~$ mysql -u root -p
    ```

## 📋 Related Articles
- [How to find out the MySQL root password](https://stackoverflow.com/questions/10895163/how-to-find-out-the-mysql-root-password)
- [How to reset the root password in MySQL 8.0.11?](https://stackoverflow.com/questions/50691977/how-to-reset-the-root-password-in-mysql-8-0-11)