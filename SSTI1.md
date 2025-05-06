# SSTI1
Web Exploitation
### Description:
> I made a cool website where you can announce whatever you want! Try it out! Additional details will be available after launching your challenge instance.

Upon starting the instance we see the updated description:
> I made a cool website where you can announce whatever you want! Try it out! I heard templating is a cool and modular way to build web apps! Check out my website here!

Clicking the website linked 'here' takes to a page that looks like this:

![website](https://github.com/RBiebrich/PicoCTF/blob/main/assets/SSTI1-website.png)

We can infer from the title of the challenge (and the hint) that we will be using a technique called **Server-side Template Injection** or **SSTI**. This is a way of using template syntax to inject a malicious payload which is executed server-side ([Read more about SSTI here](https://portswigger.net/web-security/server-side-template-injection])).
In order to do this, we need to confirm that this website is vulnerable to SSTI and if so figure out what template it uses.

To test for SSTI vulnerability, we can try **fuzzing**. Fuzzing is basically just injecting a bunch of special characters that are commonly used in template expressions and seeing what happens. Any unusual result, even an error, could indicate that our payload contained syntax that was interpretted by the server. An example fuzzing payload would be:
```
${{<%[%'"}}%\
```
In our case, we will be injecting the payload via the "What do you want to announce" box. As you can see, the fuzzing seems to indicate some sort of exception.
![fuzzing](https://github.com/RBiebrich/PicoCTF/blob/main/assets/SSTI1-fuzzing.png)
This is a good sign for our SSTI approach, but we still need to figure out what template engine the website uses in order to exploit it more deliberately.
There are many common templates, all with their own syntaxes. I found [this resource](https://www.yeswehack.com/learn-bug-bounty/server-side-template-injection-exploitation) which goes through many of them and provides example SSTI payloads for each.

After testing payloads targetting different template engines, we find in our case that **Jinja2** payloads (characterised by being wrapped in double curly braces "{{ }}") elicit some interesting responses from our website.
For example:
```Python
{{ 7+7 }}
```
which, rather than returning the literal string "{{ 7+7 }}", performs the operation and returns `14`.
Or, another example:
```Python
{{ config.items() }}
```
which displays a list of items from within the config object.
These payloads demonstrate that this Jinja2 syntax is being interpretted by the server, resulting in **Remote Code Execution (RCE)**.

Now we just need to figure out how to find the flag. To do this we may be able to search the server more directly if we can use our SSTIs to run Linux commands. Jinja2 is used with Flask, a Python framework. In Python, the **os library** can be used to run shell commands. We can use the example payloads in the [previously linked website](https://www.yeswehack.com/learn-bug-bounty/server-side-template-injection-exploitation) as a starting point.
Using the `os.popen` function to run our commands, we can start with a simple `ls`:
```Python
{{self._TemplateReference__context.cycler.__init__.__globals__.os.popen('ls').read()}}
```
That seems to have worked. The output looks like a list of files and/or directories (as you'd expect from an `ls` command) and one of them is called "flag"!
![ls](https://github.com/RBiebrich/PicoCTF/blob/main/assets/SSTI1-ls.png)

Let's assume that "flag" is a file and try to `cat` it:
```Python
{{self._TemplateReference__context.cycler.__init__.__globals__.os.popen('cat flag').read()}}
```
Sure enough, there's the flag!
![flag](https://github.com/RBiebrich/PicoCTF/blob/main/assets/SSTI1-flag.png)
**picoCTF{proxies_all_the_way_3d9e3697}**




