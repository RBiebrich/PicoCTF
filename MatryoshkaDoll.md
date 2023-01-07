# Matryoshka doll
Forensics, 30 points
### Description:
> Matryoshka dolls are a set of wooden dolls of decreasing size placed one inside another. What's the final one? Image: this

When we **wget** the linked file, we can see it is called **dolls.jpg**. If opened you will see this image:

![this](https://github.com/RBiebrich/PicoCTF/blob/main/dolls.jpg?raw=true)


For an image file I will first use **exiftool** to see if we can find anything hidden in the file's metadata. While a lot of information comes up, none of it stands out as relevant. Another approach is likely needed.

From the description we can infer we will need to dig through layers (usually in the form of directories) in order to find this flag. But the file we are given is just an image.
