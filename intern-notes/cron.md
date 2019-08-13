## What is Cron?
- a program installed in Unix OS (ex: Mac, Linux)
- used for running programs on specified time
- cron tab is the command used to make cron do its job

## Cron tab format
- Order of time: minute hour day month weekday
    + weekday (0 and 7 for Sun, 1 to 6 = Mon to Sat)
    + space ( ) is used to separate each time unit
    + comma (,) is used for having multiple entries for a time unit
    + asterisk (*) is wildcard, meaning every 
- Examples
    ```
        10 8,14,19 * * * /hoge/piyota.exe
    ```
    + on every weekday, every month, and every day at 8:10, 14:10 and 19:00, execute piyota.exe
    
    ```
        */5 * * * 5 /usr/bin/python test.py > /tmp/test.log 2>&1
    ```
    + on every Friday, every 5 minutes, execute command