# Matryoshka doll
Forensics, 30 points
### Description:
> Matryoshka dolls are a set of wooden dolls of decreasing size placed one inside another. What's the final one? Image: this

When we **wget** the linked file, we can see it is called **dolls.jpg**. If opened you will see this image:

![this](https://github.com/RBiebrich/PicoCTF/blob/main/assets/dolls.jpg?raw=true)

For an image file I will first use **exiftool** to see if we can find anything hidden in the file's metadata. While a lot of information comes up, none of it stands out as relevant. Another approach is likely needed.

From the description we can infer we will need to dig through layers (usually in the form of directories) in order to find this flag. But the file we are given is just an image. However, it is possible to hide files inside other files. We can try **unzipping** the file.

[Unzip Image]

Sure enough when we can see that something was in fact inside the file. Though it's not a file that popped out, but a directory called **base_images**. Let's **cd** into and **ls** to see what's inside.

[cd ls image]

Inside the directory there is another image file named **2_c.jpg**. Let's try **unzipping** this one, too.

[unzip 2]

Once again we have found a directory hidden within the file. Inside it is another image file called **3_c.jpg**. It seems we will just keep repeating these steps (**unzipping** the file, entering the hidden directory, and discovering a new file to **unzip**) for now.

[ unzip ...]

After a bit more **unzipping** it we find a file called **flag.txt**. That sounds promising! Let's **cat** the file to see what's inisde.

[flag]

We found the flag! **picoCTF{336cf6d51c9d9774fd37196c1d7320ff}**
