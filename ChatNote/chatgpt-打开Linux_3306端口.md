**You:**

linux放开3306端口

* * *

**ChatGPT:**

To open port 3306 on Linux, you can follow the steps below:

1.  Open the terminal on your Linux machine.
2.  Check if the MySQL or MariaDB service is running on your system. You can do this by running the command:
    
    ```lua
    systemctl status mysql
    ```
    
    or
    
    ```lua
    systemctl status mariadb
    ```
    
    If the service is not running, start it using the command:
    
    ```sql
    systemctl start mysql
    ```
    
    or
    
    ```sql
    systemctl start mariadb
    ```
    
3.  Check if the firewall is enabled on your system. You can check this by running the command:
    
    ```lua
    systemctl status firewalld
    ```
    
    or
    
    ```lua
    systemctl status ufw
    ```
    
    If the firewall is not enabled, skip to step 5.
4.  If the firewall is enabled, add a rule to allow incoming traffic on port 3306 using the following commands:
    
    ```css
    firewall-cmd --permanent --add-port=3306/tcp
    firewall-cmd --reload
    ```
    
5.  Verify that the port is open by running the command:
    
    ```perl
    netstat -tulpn | grep 3306
    ```
    
    You should see output that shows the port is listening for incoming connections.

That's it! Port 3306 should now be open on your Linux machine.