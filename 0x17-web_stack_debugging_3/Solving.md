# How to fix HTTP 500 Internal Server Error

- Published on February 23, 2022

![](https://media-exp1.licdn.com/dms/image/C4D12AQGhJ-YSZyXtyA/article-cover_image-shrink_720_1280/0/1645564838971?e=1675296000&v=beta&t=m04voCwwUwsFr3w7X1dCA3e18Xm5eKpe0lErOKM6gn4)

[![Shannel Bejarano](https://media-exp1.licdn.com/dms/image/D4E03AQEzDcJamP5cOQ/profile-displayphoto-shrink_100_100/0/1640651752894?e=1675296000&v=beta&t=BvBV2Pau6RnFSg3QQi5kYvCr4x11FgOOxfzM7peHC_M)

](https://www.linkedin.com/in/shannel-bejarano/)

[## Shannel Bejarano

](https://www.linkedin.com/in/shannel-bejarano/)

Software Development Student at Holberton

[6 articles ](https://www.linkedin.com/in/shannel-bejarano/recent-activity/posts/)Follow

This Postmortem article is based on one of my Holberton School projects, where we have to fix a server which has a **500 internal error.**

The response code** 500 Hypertext Transfer Protocol (HTTP) Internal Server Error** indicates that the server has encountered an unexpected condition that prevents it from completing the request. So let's get started.

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQHGarl-FezCuQ/article-inline_image-shrink_1500_2232/0/1645487367417?e=1675296000&v=beta&t=zZgS_56iHh8EBwePYSTzDhWlSk3YhixJX1fy-SJUXQc)

Here is an image showing the **500 error** we were talking about before, to start debugging we are going to use the "**ps auxf"** tool which will display the status of a Process. Remember that the output of "**ps auxf** " is a table where each row is a process and the columns contain the following information:

- **USER:** user under which the process is running
- **PID:** ID of the process
- **%CPU:** percentage of time the process has been running since it was started
- **%MEM** : percentage of physical memory used
- **VSZ:** virtual memory of the process measured in KiB
- **RSS:** "resident set size", it is the amount of physical memory not swapped that the task has used (in KiB)
- **TT:** terminal controlling the process (tty)
- **STAT** : status code of the process (detailed information below)
- **STARTED** : start date of the process
- **TIME:** accumulated CPU time
- **COMMAND:** command with all its arguments

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQFpaQSFn6puXg/article-inline_image-shrink_1500_2232/0/1645552739171?e=1675296000&v=beta&t=y5GRo8-XQ_F_P5siwV7lslcFuQcfiU90iQ2QVC7O5yw)

note that in the results of the **COMMAND** column we find** apache2** information with two PIDs at the bottom, with the **USER www-data** and the **PID 1354** , and also the **USER www-data** and the **PID 1358 **

taking into account the above we are going to use another of our debugging tools, in this case we are talking about "**strace** " which will allow us to trace the system calls and the signals received during the execution of the process and return the results of the command with the errors it found. We will do it in the following way **strace -p `<PID>` ** and in another terminal window we will execute again our command **curl -sI 127.0.0.1** , as I show in the following image testing each one of our **PID** until we get something related to the **error 500** . we use **tmux** to detach the window, for more information on configuring tmux [here](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/).

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQHRT_J834ReEw/article-inline_image-shrink_1500_2232/0/1645557377032?e=1675296000&v=beta&t=eOBDCYEBnf3NAAdWMW8yZArmd8qnEcqp4NPkKjA9Iwk)

run the **curl -sI 127.0.0.1** again in the other window and now we will see the errors and now let's take a look.

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQFZPIIYOXlfaA/article-inline_image-shrink_1500_2232/0/1645557800970?e=1675296000&v=beta&t=dC8Vpa6WhY4FlIqILr-csfR8gz4SEvUppiSdgnQoH4c)

this was part of the **strace** output but let's take a closer look at part of it..

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQEGDOIU21XCUA/article-inline_image-shrink_1500_2232/0/1645558058630?e=1675296000&v=beta&t=YgmLhhpZKCx4RVojXx0C2TfaPg32RBCRNyy-JfbK7to)

You will see that the strace will not generate any more output at which point we can stop our search, at this point we can see that there is an **ENOENT** error. which is short for **Error NO ENTry (or Error NO ENTity)** , and can actually be used for more than just files/directories. that is to say, the file that we are looking for is not found or does not exist, for that we go to the path where this error appears and we will look for the file as such.

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQEFdchTfxIB1Q/article-inline_image-shrink_1500_2232/0/1645558978069?e=1675296000&v=beta&t=PnQhbLdAppjyH-n_zJSJ7FWIbIvMk4HmAzSHoq56aXQ)

yes, the file exists, so that was not the problem, the problem was that where the file is called was misspelled as **class-wp-locale.phpp** so it did not recognise the file. so we researched the name of that file on the internet and found out that it was a **WordPress** configuration file, we also found out that there are two **WordPress** configuration files, with sensitive and important information that will make your site work correctly or even not work, these files are called **wp-config.php** and **wp-settings.php**

Now we will use the **grep 'word' filename ** command to help us find any lines containing the word **class-wp-locale.phpp** in the file. note that the configuration files are in the **html** directory as shown below.

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQFl8cWGbn1hew/article-inline_image-shrink_1500_2232/0/1645560886823?e=1675296000&v=beta&t=cVRS1r5u8sWHv-DEOqqWH58iiWtw0V9F1pg3CipMyf4)

we have found the error !!!! and as we can see we have checked both files and the error is in the **wp-settings.php** file.

Now we just need to access the file and change that word to the correct one, it can be done manually but for this project we will use a** puppet file** like the following one:

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQEplyfQBoTR6Q/article-inline_image-shrink_1500_2232/0/1645561968634?e=1675296000&v=beta&t=CW9sgZBdujo9KiIjcoV8HtWaFW0f3E5u1D04mdguEFM)

to be able to run it we will do it in the following way **puppet apply <file.pp> ** like this:

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQENqKdOdOwrAw/article-inline_image-shrink_1500_2232/0/1645562186553?e=1675296000&v=beta&t=MBxVrNC_DaFa9KbK3hChuw6xVkYKSSgfglIOYitQ0OQ)

we checked with the **curl -sI 127.0.0.1** command and successfully **fixed the 500 error !!!**

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQHLDpwIylPKNg/article-inline_image-shrink_1500_2232/0/1645563231490?e=1675296000&v=beta&t=As1f1ZiHmU5-kRYEagocYLcO1pCshcRfeiIg58VBV4A)

I hope you found it useful, have a excellent day!

[https://www.linode.com/docs/guides/use-the-ps-aux-command-in-linux/](https://www.linode.com/docs/guides/use-the-ps-aux-command-in-linux/)

[https://man7.org/linux/man-pages/man1/strace.1.html](https://man7.org/linux/man-pages/man1/strace.1.html)

[https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)

[https://wordpress.org/support/article/editing-wp-config-php/#:~:text=One%20of%20the%20most%20important,WordPress%2C%20the%20wp%2Dconfig.](https://wordpress.org/support/article/editing-wp-config-php/#:~:text=One%20of%20the%20most%20important,WordPress%2C%20the%20wp%2Dconfig.)

[https://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/](https://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/)

[https://www.puppetcookbook.com/posts/exec-a-command-in-a-manifest.html](https://www.puppetcookbook.com/posts/exec-a-command-in-a-manifest.html)

[https://linuxhint.com/use-linux-strace-command/](https://linuxhint.com/use-linux-strace-command/)
