**You:**

报错：nohup python3 /Users/yutianran/MyGithub/MyCode/python/main/app\_flask.py >> log/app\_flask\_\`date +'%Y%m%d'\`.log 2>&1 & \[1\] 51514 (base) yutianran@yutianran MyCode % \[1\] + suspended (tty output) nohup python3 /Users/yutianran/MyGithub/MyCode/python/main/app\_flask.py >> 2

* * *

**ChatGPT:**

It seems like the command you executed using `nohup` got suspended.

This could be due to several reasons, such as the terminal output being redirected to a file that does not exist or the program encountered an error that caused it to stop.

To troubleshoot this issue, you can try the following steps:

1.  Check the log file to see if any error messages have been written to it: `tail -f log/app_flask_$(date +'%Y%m%d').log`
    
2.  Check if the process is still running using the `ps` command: `ps -ef | grep app_flask.py`
    
3.  If the process is still running, try to bring it back to the foreground using the `fg` command: `fg %1`
    
4.  If the process is no longer running, you may need to restart it.
    

In addition, it's worth noting that the command you used to redirect the output to a log file is using backticks (`), which may not work on all systems. You could try using $( ) instead, like this:` nohup python3 /Users/yutianran/MyGithub/MyCode/python/main/app\_flask.py >> log/app\_flask\_$(date +'%Y%m%d').log 2>&1 &\`