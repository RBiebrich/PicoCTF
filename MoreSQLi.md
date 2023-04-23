# More SQLi
Web Exploitation, 200 points
### Description:
> Can you find the flag on this website. Additional details will be available after launching your challenge instance.

First, we launch the instance and we are provided with a link. The link takes us to a login portal which we can assume, given the title of the challenge, we will be attacking with **SQL injection** (also called **SQLi** for short).

![injectable](https://github.com/RBiebrich/PicoCTF/blob/main/assets/injectable.png)

To start, let's put in some test inputs to see what happens. I'll use 42 but you can type whatever you'd like. We can see this conveniently returns the entire SQL query that the portal is using to verify the input:

![structure](https://github.com/RBiebrich/PicoCTF/blob/main/assets/structure.png)

Now that we know the exact structure of the query we will not need to do any blind SQL injection. Instead, we can precisely craft an input that will get us in. As you can see in the query, the password comes first so we will be injecting there.

My injection looks like this:
```SQL
42' OR 1==1 --
```
Where:
> - 42 is an arbitrary password I chose (choose whatever you'd like for this)
> - ' is there to close the string. This is crucial for our SQL injection to work
> - OR is the boolean operator which says that only one of the statements needs to be **true**
> - 1==1 is a statement which will always evaluate to **true**
> - -- is used in SQL to comment out whatever follows. In our case, that renders the username useless, but you still need to write something in the username box

Injected into the existing query structure, this results in the query:
```SQL
SELECT id FROM users WHERE password = '42' OR 1==1 --AND username = '42'
```
We're in! This takes us to the following page:

![loggedin](https://github.com/RBiebrich/PicoCTF/blob/main/assets/loggedin_png.png)

The data displayed on this page seems to be of no use to us, search bar and the URL seem to be unimportant, and a quick once over in dev tools reveals nothing. This may seem like a dead end. Perhaps we were redirected here? Back spacing takes us to no intermediate pages so if there were redirects, we will have to look for their requests in Burp Suite. We open up Burp Suite, navigate to the proxy tab, and click **open browser**. We can paste the URL of the login page into the Chromium window and then make sure to turn **inetercept on** in Burp Suite. From here we can just repeat our SQL injection, only this ime Burp Suite is intercepting all the requests. We can hit **forward** in Burp Suite, until the webpage in Chromium is fully logged in. Now we can move to the logger tab to see if there were any redirects. Sure enough, there's a 302 request, the signature of a redirect. If we inspect this request we can see this block of HTML:

![burp_flag](https://github.com/RBiebrich/PicoCTF/blob/main/assets/burp_flag.png)

We found the flag!

**picoCTF{G3tting_5QL_1nJ3c7I0N_l1k3_y0u_sh0ulD_c8b7cc2a}**
