# Cronjobs

## **`crontab -e` Edit the crontab for the current user**

## **`crontab -l` List the crontab for the current user**

## `m h dom mon dow command`

## **`30 2 * * * /path/to/script.sh`**

### **Explanation:**

- **`m` (Minute):** The first field represents the minute when the cron job will run. In the example, **`30`** means the task will start 30 minutes past the hour.
- **`h` (Hour):** The second field represents the hour when the cron job will run. In the example, **`2`** means the task will run at 2:30 AM.
- **`dom` (Day of Month):** The third field represents the day of the month when the cron job will run. An asterisk (*) means "every day of the month."
- **`mon` (Month):** The fourth field represents the month when the cron job will run. An asterisk (*) means "every month."
- **`dow` (Day of Week):** The fifth field represents the day of the week when the cron job will run. An asterisk (*) means "every day of the week." The days of the week are represented numerically, with Sunday as 0 or 7.
- **`command`:** This is the actual command or script that will be executed. In the example, **`/path/to/script.sh`** is the script that will run at the specified schedule.

### **Additional Tips:**

- **Special Characters:**
    - An asterisk (*) in any field means "every" or "any." For example, **`* * * *`** would mean "every minute of every hour, every day."
    - You can use ranges and lists. For instance, **`1,15,30`** in the minute field means "at 1, 15, and 30 minutes past the hour."
- **Ranges:**
    - You can use a hyphen (-) to specify a range. For example, **`1-5`** in the hour field means "every hour from 1 to 5."
- **Examples:**
    - **`0 0 * * *`** would run a task at midnight every day.
    - **`0 12 * * 1-5`** would run a task at noon, Monday through Friday.