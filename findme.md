# findme
Web Exploitation, 100 points
### Description:
> Help us test the form by submiting the username as test and password as test! Additional details will be available after launching your challenge instance.

First, we launch the instance and we are provided with a link. The link takes us to a login page which asks for our username and password. We are told by the description that we should log in with the username **test** and the password **test!**. When we plug those in, we are taken to a page which looks like this:

![redirect](https://github.com/RBiebrich/PicoCTF/blob/main/assets/redirect.png)

I try entering the obvious keywords like "flag" into the search box, but we are sent to the same page everytime. I then try inspecting the page in dev tools, but find nothing. If you'll notice, the page tells us that we arrived here by redirection, meaning this page was not our original destination from the login page. So we can try going back **<-**. And sure enough there is an intermediate page where we quickly notice the following block of HTML in dev tools:

![flag_head](https://github.com/RBiebrich/PicoCTF/blob/main/assets/flag_head.png)

This header called "flag" lets us know we are on the right track, but further inspection of the page in dev tools yields nothing. We must examine something else for clues. The URL was my next subject. Immediately, we can recognize by the double equal signs (**==**) at the end of the URL, that the preceding string of text is likely to be a **base 64** encryption. I take the string following the "id=" and plug it in to CyberChef [https://gchq.github.io/CyberChef/] configured to decode from base 64.

![CC1](https://github.com/RBiebrich/PicoCTF/blob/main/assets/CC1.png)

Sure enough, the ouput is not only legible, but also appears to be in flag format, though only the second half. To find the first half, we can try going back **<-** again to see if there is another intermediate page that we were redirected from. There is! We can again take the string of text following "id=" from the URL and plug it into CyberChef.

![CC2](https://github.com/RBiebrich/PicoCTF/blob/main/assets/CC2.png)

There's our first half! So putting it all together, the flag is:

**picoCTF{proxies_all_the_way_3d9e3697}**
