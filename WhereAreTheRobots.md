# Where Are The Robots
Web Exploitation, 100 points
### Description:
> Can you find the robots? [followed by a link to a webpage]

From the title we can assume this challenge will have something to do with the **robots.txt** file of the linked webpage. From Wikipedia:
> A robots.txt file is a standard used by websites to indicate to visiting web crawlers and other web robots which portions of the website they are not allowed to visit.

In other words, it is often used by website admins as a place to specify which parts of the site they would like hidden from search engines. We follow the link and find it displays the following:

![welcome.png](https://github.com/RBiebrich/PicoCTF/blob/main/assets/welcome.png)

This, once again, suggests we check **robots.txt**. So at the end of the url we type **/robots.txt** and hit enter. This is what we find:

![robots.png](https://github.com/RBiebrich/PicoCTF/blob/main/assets/robots.png)

The **Disallow** header contains the list of URL paths that the website admin doesn't want us to see. So we take the **/8028f.html** and plug it into the end of the URL (replacing **/robots.txt**) and hit enter. This brings us here:

![robo_flag.png](https://github.com/RBiebrich/PicoCTF/blob/main/assets/robo_flag.png)

We found the flag! **picoCTF{ca1cu1at1ng_Mach1n3s_8028f}**
