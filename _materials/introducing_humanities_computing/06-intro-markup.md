---
title: "Introduction to File Formats & Plain Text"
permalink: /materials/introducing-humanities-computing/06-intro-markup
excerpt: "An introduction to markup languages and web styling of documents."
toc: true
---


So now we are starting to see how we can use the command line to create directories and files, and move them around. It seems like we're doing a lot of work to do something that we could do with a GUI, but the command line is actually much more powerful than a GUI. For example, we can use the command line to do things like search for text in a file, count the number of words in a file, and even delete a file.

Before we try that though, let's learn a bit more about what files are exactly.

## Introducing File Formats

What constitutes a document or a file might seem obvious, but is actually a robust and ongoing scholarly debate in Library and Information Sciences (LIS) and Computer Science (CS). To put it a bit simply, a file is a collection of data stored in a single unit, identified by a filename. It can be a document, an image, a video, a sound, or any other collection of data. The file extension is the part of the file name after the period. The file extension tells the computer what type of file it is and what program to use to open it.

<figure>
  <a href="https://www.filecenter.com/blog/wp-content/uploads/2022/04/The-Giant-List-of-Document-File-Types-and-Extensions.jpg">
    <img src="https://www.filecenter.com/blog/wp-content/uploads/2022/04/The-Giant-List-of-Document-File-Types-and-Extensions.jpg" alt="File Formats" class="image-popup">
  </a>
</figure>

There are many different file formats and each one has its own purpose. For example, a `.docx` file is a Microsoft Word document, a `.jpg` file is an image, and a `.mp3` file is an audio file. Some file formats are proprietary, meaning they are owned by a company and can only be opened by certain programs. For example, `.docx` files can only be opened by Microsoft Word. So if you try to open up a word document in a PDF Viewer you might see what looks like a bunch of gibberish.

<figure>
  <a href="https://www.fonelab.com/images/data-retriever/fonelab-data-retriever-how-to-corrupt-a-word-file-doc-text-only.jpg">
    <img src="https://www.fonelab.com/images/data-retriever/fonelab-data-retriever-how-to-corrupt-a-word-file-doc-text-only.jpg" alt="Corrupted Word Doc" class="image-popup">
  </a>
</figure>

This gibberish is actually the code that makes up the file, but since the PDF Viewer doesn't know how to read the code it just shows you the code itself rather than the data stored in the files.

Other file formats are open source, meaning they are not owned by a company and can be opened by many different programs. For example, `.txt` files are plain text files that can be opened by any text editor. When we used the `touch` command, we told the terminal to create a `.txt` file. This is because `.txt` files are the simplest file format and only contain text. They do not contain any formatting like bold, italics, or images.

### What is Plain Text?

Today, we have been talking a lot about *text*, from text commands to text files. But the core concept with both of these is the idea of plain text.

As scholars working with computers, we need to be aware of the ways plain text and formatted text differ. While a Word Document ant .txt file might look the same to us, the Word Document actually contains a lot of hidden formatting that the .txt file does not. In programming, we want to be explicit in our communications with computers and so plain text is preferable, but what is it exactly?

According to the [Unicode Standard](https://unicode.org/versions/Unicode13.0.0/),

> Plain text is a pure sequence of character codes; plain Unicode-encoded text is therefore a sequence of Unicode character codes.

This is a bit technical, but the key concept is that plain text shows if it is formatted or not (we call this markup), and usually contains no formatting. Plain text can be moved between programs more fluidly and can respond to programmatic manipulations. It is often manipulated in something called a *text editor* (like VS Code), which is a program that allows you to edit plain text files.

We can see an example of a plain txt file, via Project Gutenberg - a website that hosts public domain books in plain text format.

<figure>
  <a href="{{site.baseurl}}/assets/images/pride_prejudice_gutenberg.png">
    <img src="{{site.baseurl}}/assets/images/pride_prejudice_gutenberg.png" alt="Pride and Prejudice" class="image-popup">
  </a>
  <figcaption><i>Pride and Prejudice</i> by Jane Austen <a href="https://www.gutenberg.org/ebooks/1342">https://www.gutenberg.org/ebooks/1342</a></figcaption>
</figure>

I can download this file directly from my terminal using the command line:

```sh
curl https://www.gutenberg.org/files/1342/1342-0.txt > pride-and-prejudice.txt
```

In this example, I'm using the command `curl` to download the file from the internet. The `>` symbol tells the terminal to create a new file, store this data in the file, and save the file as `pride-and-prejudice.txt`. We can see that this file is a `.txt` file, meaning it is a plain text file. `curl` stands for "client URL" and is a command line tool for transferring data and downloading files from the internet. We can also use it to upload files to the internet.

If you are on a Windows computer, you can instead use the command `wget` to download the file from the internet. The syntax is the same, but the command is different.

```sh
wget https://www.gutenberg.org/files/1342/1342-0.txt > pride-and-prejudice.txt
```

`wget` stands for "web get" and is a similar command line tool for retrieving and downloading files from the web.

If you are using PowerShell and not WSL (again WSL is recommended!), the command looks slightly different:

```sh
wget https://www.gutenberg.org/files/1342/1342-0.txt -OutFile pride-and-prejudice.txt 
```

----

Now that we have downloaded this file, we can use the command line to display some of the text:

```sh
cat pride-and-prejudice.txt
```

`cat` stands for concatenate, and is a command line tool for displaying the contents of a file. We can see that this file contains the text of *Pride and Prejudice* by Jane Austen (though you'll likely only see the end of the text since it is so long, without scrolling for ages).

We can start to interact with this file in a number of ways.

First, let's count how many words are in this file. To do this, we can use the command `wc -w`. Let's try it out!

```sh
wc -w pride-and-prejudice.txt
```

`wc` stands for word count, and the `-w` flag tells the terminal to count the number of words in the file. You'll often notice that commands have flags like this, which are additional instructions for the command.

We can also use the command `wc` to count the number of lines in a file with the `-l` flag. Let's try it out!

```sh
wc -l pride-and-prejudice.txt
```

We should see that there are `14911` lines in this file.

We can also search for a specific word in a file using the command `grep`. Let's try to find out how often the word "pride" appears in this file. To do this, we can use the command `grep pride pride-and-prejudice.txt`. Let's try it out!

```sh
grep pride pride-and-prejudice.txt
```

This should give us the following output:

<figure>
  <a href="{{site.baseurl}}/assets/images/grep_pride.png">
    <img src="{{site.baseurl}}/assets/images/grep_pride.png" alt="Pride and Prejudice" class="image-popup">
  </a>
</figure>

How could we count the number of times the word "pride" appears in this file? We could use the command `wc -w` to count the number of words, but that would include the word "pride" in the title of the book. We could also use the command `wc -l` to count the number of lines, but that would include the line numbers. Instead, we can use the command `grep -c pride pride-and-prejudice.txt`.

We can see that the word "pride" appears 43 times in this file. How many times does the word "prejudice" appear?

## Homework Exercises

<figure>
  <a href="https://media.giphy.com/media/Xbn2CXq5u2Wc0/giphy.gif">
    <img src="https://media.giphy.com/media/Xbn2CXq5u2Wc0/giphy.gif" alt="maze" class="image-popup">
  </a>
</figure>

### Assignment 1: Find Your Way: Solve A Command-Line ~~Corn~~ Maze

Your first assignment, which you are welcome to work in pairs, is to solve an existing command line maze. You can find the maze here: as a [zip file]({{site.baseurl}}/assets/files/cli_exercises/zipped_gutenberg_maze.zip) or via our [GitHub repo](https://github.com/ZoeLeBlanc/is310-computing-humanities-2024/blob/gh-pages/assets/files/cli_exercises/gutenberg_maze.zip) All links should be working, but if you have trouble please try reloading the website and contacting the instructor! You should download the zip file, and use the command line to unzip the file and then solve the maze.

To unzip the file, you can use the command `unzip path/to/your/zip/file -d path/to/your/desired/directory` if you are running Unix/Linux (so Macs, Linux, and the Window Subsystems for Linux -- all of which are recommended). If you are using PowerShell, you will need to use the command `Expand-Archive -Path "path\to\your\zip\file" -DestinationPath "path\to\your\desired\directory`. Be sure to replace these dummy file paths with your actual desired ones (or ask AI for help!). Your goal with this maze is to use both the command line cheatsheet and your preferred AI chatbot to solve the maze. You should be able to solve the maze in ~30 minutes, but if you are struggling, please reach out to the instructors via Slack for help.

### Assignment 2: Time To Get Lost: Create A Command-Line ~~Corn~~ Maze

For our ~~Tuesday class next week~~ Thursday class, your goal is to create your own command line maze, which we will try to solve in class. You should work individually on this, though you are welcome to brainstorm ideas.

Your maze should have the following components:

- [ ] An entrance and a solution
- [ ] At least 5 directories (these can be nested)
- [ ] At least 5 files (these can be nested)
- [ ] One hidden file or directory
- [ ] Clues to help us solve the maze

You are welcome to use Project Gutenberg [https://www.gutenberg.org/](https://www.gutenberg.org/) for your files, but you can also use other files if you prefer. You can also use the command line to create your files, but you can also use a text editor if you prefer.

<div class="notice--info">⚡️ You will not be submitting either assignment until we get through the lesson on git and GitHub on Tuesday, so please just bring your <strong>completed</strong> assignments with you to class.</div>



Before we try that though, let's learn a bit more about what files are exactly.

## Introducing File Formats

What constitutes a document or a file might seem obvious, but is actually a robust and ongoing scholarly debate in Library and Information Sciences (LIS) and Computer Science (CS). To put it a bit simply, a file is a collection of data stored in a single unit, identified by a filename. It can be a document, an image, a video, a sound, or any other collection of data. The file extension is the part of the file name after the period. The file extension tells the computer what type of file it is and what program to use to open it.

<figure>
  <a href="https://www.filecenter.com/blog/wp-content/uploads/2022/04/The-Giant-List-of-Document-File-Types-and-Extensions.jpg">
    <img src="https://www.filecenter.com/blog/wp-content/uploads/2022/04/The-Giant-List-of-Document-File-Types-and-Extensions.jpg" alt="File Formats" class="image-popup">
  </a>
</figure>

There are many different file formats and each one has its own purpose. For example, a `.docx` file is a Microsoft Word document, a `.jpg` file is an image, and a `.mp3` file is an audio file. Some file formats are proprietary, meaning they are owned by a company and can only be opened by certain programs. For example, `.docx` files can only be opened by Microsoft Word. So if you try to open up a word document in a PDF Viewer you might see what looks like a bunch of gibberish.

<figure>
  <a href="https://www.fonelab.com/images/data-retriever/fonelab-data-retriever-how-to-corrupt-a-word-file-doc-text-only.jpg">
    <img src="https://www.fonelab.com/images/data-retriever/fonelab-data-retriever-how-to-corrupt-a-word-file-doc-text-only.jpg" alt="Corrupted Word Doc" class="image-popup">
  </a>
</figure>

This gibberish is actually the code that makes up the file, but since the PDF Viewer doesn't know how to read the code it just shows you the code itself rather than the data stored in the files.

Other file formats are open source, meaning they are not owned by a company and can be opened by many different programs. For example, `.txt` files are plain text files that can be opened by any text editor. When we used the `touch` command, we told the terminal to create a `.txt` file. This is because `.txt` files are the simplest file format and only contain text. They do not contain any formatting like bold, italics, or images.

### What is Plain Text?

Today, we have been talking a lot about *text*, from text commands to text files. But the core concept with both of these is the idea of plain text.

As scholars working with computers, we need to be aware of the ways plain text and formatted text differ. While a Word Document ant .txt file might look the same to us, the Word Document actually contains a lot of hidden formatting that the .txt file does not. In programming, we want to be explicit in our communications with computers and so plain text is preferable, but what is it exactly?

According to the [Unicode Standard](https://unicode.org/versions/Unicode13.0.0/),

> Plain text is a pure sequence of character codes; plain Unicode-encoded text is therefore a sequence of Unicode character codes.

This is a bit technical, but the key concept is that plain text shows if it is formatted or not (we call this markup), and usually contains no formatting. Plain text can be moved between programs more fluidly and can respond to programmatic manipulations. It is often manipulated in something called a *text editor* (like VS Code), which is a program that allows you to edit plain text files.

We can see an example of a plain txt file, via Project Gutenberg - a website that hosts public domain books in plain text format.

<figure>
  <a href="{{site.baseurl}}/assets/images/pride_prejudice_gutenberg.png">
    <img src="{{site.baseurl}}/assets/images/pride_prejudice_gutenberg.png" alt="Pride and Prejudice" class="image-popup">
  </a>
  <figcaption><i>Pride and Prejudice</i> by Jane Austen <a href="https://www.gutenberg.org/ebooks/1342">https://www.gutenberg.org/ebooks/1342</a></figcaption>
</figure>

I can download this file directly from my terminal using the command line:

```sh
curl https://www.gutenberg.org/files/1342/1342-0.txt > pride-and-prejudice.txt
```

In this example, I'm using the command `curl` to download the file from the internet. The `>` symbol tells the terminal to create a new file, store this data in the file, and save the file as `pride-and-prejudice.txt`. We can see that this file is a `.txt` file, meaning it is a plain text file. `curl` stands for "client URL" and is a command line tool for transferring data and downloading files from the internet. We can also use it to upload files to the internet.

If you are on a Windows computer, you can instead use the command `wget` to download the file from the internet. The syntax is the same, but the command is different.

```sh
wget https://www.gutenberg.org/files/1342/1342-0.txt > pride-and-prejudice.txt
```

`wget` stands for "web get" and is a similar command line tool for retrieving and downloading files from the web.

If you are using PowerShell and not WSL (again WSL is recommended!), the command looks slightly different:

```sh
wget https://www.gutenberg.org/files/1342/1342-0.txt -OutFile pride-and-prejudice.txt 
```

----

Now that we have downloaded this file, we can use the command line to display some of the text:

```sh
cat pride-and-prejudice.txt
```

`cat` stands for concatenate, and is a command line tool for displaying the contents of a file. We can see that this file contains the text of *Pride and Prejudice* by Jane Austen (though you'll likely only see the end of the text since it is so long, without scrolling for ages).

We can start to interact with this file in a number of ways.

First, let's count how many words are in this file. To do this, we can use the command `wc -w`. Let's try it out!

```sh
wc -w pride-and-prejudice.txt
```

`wc` stands for word count, and the `-w` flag tells the terminal to count the number of words in the file. You'll often notice that commands have flags like this, which are additional instructions for the command.

We can also use the command `wc` to count the number of lines in a file with the `-l` flag. Let's try it out!

```sh
wc -l pride-and-prejudice.txt
```

We should see that there are `14911` lines in this file.

We can also search for a specific word in a file using the command `grep`. Let's try to find out how often the word "pride" appears in this file. To do this, we can use the command `grep pride pride-and-prejudice.txt`. Let's try it out!

```sh
grep pride pride-and-prejudice.txt
```

This should give us the following output:

<figure>
  <a href="{{site.baseurl}}/assets/images/grep_pride.png">
    <img src="{{site.baseurl}}/assets/images/grep_pride.png" alt="Pride and Prejudice" class="image-popup">
  </a>
</figure>

How could we count the number of times the word "pride" appears in this file? We could use the command `wc -w` to count the number of words, but that would include the word "pride" in the title of the book. We could also use the command `wc -l` to count the number of lines, but that would include the line numbers. Instead, we can use the command `grep -c pride pride-and-prejudice.txt`.

We can see that the word "pride" appears 43 times in this file. How many times does the word "prejudice" appear?

## Homework Exercises

<figure>
  <a href="https://media.giphy.com/media/Xbn2CXq5u2Wc0/giphy.gif">
    <img src="https://media.giphy.com/media/Xbn2CXq5u2Wc0/giphy.gif" alt="maze" class="image-popup">
  </a>
</figure>

### Assignment 1: Find Your Way: Solve A Command-Line ~~Corn~~ Maze

Your first assignment, which you are welcome to work in pairs, is to solve an existing command line maze. You can find the maze here: as a [zip file]({{site.baseurl}}/assets/files/cli_exercises/zipped_gutenberg_maze.zip) or via our [GitHub repo](https://github.com/ZoeLeBlanc/is310-computing-humanities-2024/blob/gh-pages/assets/files/cli_exercises/gutenberg_maze.zip) All links should be working, but if you have trouble please try reloading the website and contacting the instructor! You should download the zip file, and use the command line to unzip the file and then solve the maze.

To unzip the file, you can use the command `unzip path/to/your/zip/file -d path/to/your/desired/directory` if you are running Unix/Linux (so Macs, Linux, and the Window Subsystems for Linux -- all of which are recommended). If you are using PowerShell, you will need to use the command `Expand-Archive -Path "path\to\your\zip\file" -DestinationPath "path\to\your\desired\directory`. Be sure to replace these dummy file paths with your actual desired ones (or ask AI for help!). Your goal with this maze is to use both the command line cheatsheet and your preferred AI chatbot to solve the maze. You should be able to solve the maze in ~30 minutes, but if you are struggling, please reach out to the instructors via Slack for help.

### Assignment 2: Time To Get Lost: Create A Command-Line ~~Corn~~ Maze

For our ~~Tuesday class next week~~ Thursday class, your goal is to create your own command line maze, which we will try to solve in class. You should work individually on this, though you are welcome to brainstorm ideas.

Your maze should have the following components:

- [ ] An entrance and a solution
- [ ] At least 5 directories (these can be nested)
- [ ] At least 5 files (these can be nested)
- [ ] One hidden file or directory
- [ ] Clues to help us solve the maze

You are welcome to use Project Gutenberg [https://www.gutenberg.org/](https://www.gutenberg.org/) for your files, but you can also use other files if you prefer. You can also use the command line to create your files, but you can also use a text editor if you prefer.

<div class="notice--info">⚡️ You will not be submitting either assignment until we get through the lesson on git and GitHub on Tuesday, so please just bring your <strong>completed</strong> assignments with you to class.</div>

## Additional Resources

In addition to the resources, I linked at the beginning of this lesson, I would recommend the following:

1. Ian Milligan and James Baker, "Introduction to the Bash Command Line," *The Programming Historian* 3 (2014), [https://programminghistorian.org/en/lessons/intro-to-bash](https://programminghistorian.org/en/lessons/intro-to-bash). This is an introduction to the Bash shell, which will serve well enough as an introduction to other shells like Zsh as well.
2. [Bash Basics Part 1 of 8 Access and Navigation](https://youtu.be/eH8Z9zeywq0?t=885)
3. [Beginner's Guide to the Bash Terminal](https://www.youtube.com/watch?v=oxuRxtrO2Ag)
4. [The Most Important Thing You'll Learn in the Command Line](https://www.youtube.com/watch?v=q7-aEspwwEI)
5. Go through the CodeAcademy [command line course](https://www.codecademy.com/learn/learn-the-command-line).
6. [Shell Scripting Tutorial](https://www.youtube.com/watch?v=hwrnmQumtPw)

[^1]: If you have serious concerns over GitHub Co-Pilot, I've just learned of Privy, a private and locally run version of a coding assistant [https://github.com/srikanth235/privy](https://github.com/srikanth235/privy). I haven't had a chance to try it yet, but seems like a promising alternative. Another option is [gpt4all](https://gpt4all.io/index.html) which let's you run a number of models locally (though anecdotally some of my previous students struggled to install it on their computers, so ymmv).

## Introducing Markdown

While `.txt` files are useful, in digital humanities we often use a file format called Markdown. Like txt, Markdown is a plain text file format that uses symbols to add formatting to the text, and has the file extension `.md`. You have already seen an example of this with the `README.md` files on GitHub.

Let's try creating a Markdown file in our folder. We can do this by using the `touch` command and adding `.md` to the end of the file name.

```sh
touch is310-computing-humanities.md
```

Now we can open this file in VS Code and add some text to it.

```sh
Computing in the Humanities is defined as the application of computational methods and tools to the study of humanistic questions.
```

Now if we wanted too, we could add some formatting to this text using Markdown.

Maybe we want to highlight the word "Computing in the Humanities" in bold. To do this we would put two asterisks on either side of the word.

```md
**Computing in the Humanities** is defined as ...
```

We could also put `defined` into italics by putting one asterisk on either side of the word.

```md
**Computing in the Humanities** is *defined* as ...
```

Finally, we could add a heading to this text by putting a hashtag in front of the heading.

```md
# Welcome to Computing in the Humanities
```

We can see what this looks like in VS Code by using the Markdown Preview extension. To do this, click on the icon in the top right corner of VS Code that looks like a magnifying glass. This will open a preview of the Markdown file in a new tab.

You should see something like this, where our Markdown file is on the left and the preview is on the right:

<figure>
  <a href="https://code.visualstudio.com/assets/docs/languages/Markdown/preview-scroll-sync.gif">
    <img src="https://code.visualstudio.com/assets/docs/languages/Markdown/preview-scroll-sync.gif" alt="Markdown Preview" class="image-popup">
  </a>
</figure>

Now you can see that our formatting has been applied to the text. This type of formatting is called Markdown syntax and it is a way of adding formatting to plain text files. What we did was exactly the same as what you do when you use the buttons in Word or Google Docs to add headers or styling to your text. The difference is that we are using symbols to add this formatting instead of buttons.

This may seem like a lot of extra work, but the advantages of Markdown are numerous.

1. It is much more sustainable than Word or Google Docs. This is because the text is saved as plain text and not in a format that is optimized for editing. This makes it future-proof so that you don't require a license or access to an application to see your files.
2. It can also be rendered by any text editor. This is because Markdown is a plain text format and not a rich text format. This makes it platform agnostic.
3. Finally, Markdown plays nicely with Github, which renders it directly in your browser. This makes it easy to push up your files into your repositories.

Markdown was created by John Gruber with help from Aaron Swartz in 2004. The goal was to create a file format that was easy to read and write, could be converted into web documents, and could be used by anyone. You can read more about the history of Markdown, in Bednarski, Dawid. “The History of Markdown: A Prelude to the No-Code Movement.” *Taskade Blog*, March 25, 2022. [https://www.taskade.com/blog/markdown-history/](https://www.taskade.com/blog/markdown-history/).

It's important to understand that Markdown has this history because many flavors of Markdown exist, and standardization of Markdown has been an ongoing project. For example, GitHub Flavored Markdown (GFM) is a flavor of Markdown that was created by GitHub in 2009. It is a superset of Markdown, meaning it adds additional features to Markdown. For example, GFM allows you to create tables in Markdown, which is not possible in regular Markdown. You can read more about GFM in “GitHub Flavored Markdown Spec.” GitHub, 2022. [https://github.github.com/gfm/](https://github.github.com/gfm/).

<figure>
  <a href="https://d11a6trkgmumsb.cloudfront.net/original/3X/c/b/cb7374a606a66c5b8e489afed76d93ed49dc7836.png">
    <img src="https://d11a6trkgmumsb.cloudfront.net/original/3X/c/b/cb7374a606a66c5b8e489afed76d93ed49dc7836.png" alt="Markdown Flavors" class="image-popup">
  </a>
</figure>

This figure shows some examples of how Markdown is written depending on the platform and standards. You can also see some of the discussions that go on to help shape these standards through the various repositories on GitHub that host the standards. For example, CommonMark is another popular Markdown style and improvements to it are discussed in this repository [https://github.com/commonmark/commonmark-spec/issues](https://github.com/commonmark/commonmark-spec/issues).

### First Exercise

Now that you have a basic understanding of Markdown, let's try improving the `README.md` file in our `is310-coding-assignments` repository. To do this, we are going to use the Markdown Preview extension in VS Code.

Try adding the following elements:

- [ ] A heading
- [ ] A subheading
- [ ] A bulleted list

*Bonus option*:
- [ ] A **Link** to our course website

You can use both AI tools and this [GitHub Markdown Cheatsheet](https://github.com/lifeparticle/Markdown-Cheatsheet) to help you. Once you've completed the exercise, push up your changes to GitHub.

## Homework Exercises

You can either complete this exercise prior to our next class meeting or during our next class meeting, but regardless should come to class with any questions or issues you have.

### Solve the Cloned Corn Maze

Your assignment before our next class meeting (so this upcoming Thursday) is to clone two of your peers' repositories and solve their mazes. You can find the links to their repositories in the discussion forum [https://github.com/ZoeLeBlanc/is310-computing-humanities-2024/discussions/1](https://github.com/ZoeLeBlanc/is310-computing-humanities-2024/discussions/1), and you should try solving the mazes that have yet to be solved based on replies.

Once you have solved the maze, you should reply to their post in the discussion, letting them know that you were successful and posting a screenshot of your solved maze in your terminal. If you have any issues with the maze, you're welcome to request help on their post, but please try to solve it on your own first.

## Collaborative Coding

So far we have been creating new repositories on GitHub and then pulling them to our local computer. But what if we want to pull a repository that already exists on GitHub? This is where **cloning** becomes useful.

### Cloning a Repository

Cloning is GitHub's term for downloading a repository from GitHub to your local computer. To clone a repository, we need to use the `git clone` command and then the URL of the remote repository.

```sh
git clone https://github.com/USERNAME/REPOSITORY
```

You can find the correct URL of a repository by clicking on the green `Code` button and then copying the URL. Do not copy the above example, since it's just a placeholder. You need to copy the URL of the repository you want to clone.

<figure>
  <a href="https://docs.github.com/assets/cb-69468/mw-1440/images/help/repository/https-url-clone-cli.webp"><img src="https://docs.github.com/assets/cb-69468/mw-1440/images/help/repository/https-url-clone-cli.webp" class="image-popup"></a>
</figure>

If you setup `SSH`, you should select the `Use SSH` button, but otherwise you can use `HTTPS` and then copy the URL.

Now we can clone the repository by using the `git clone` command and then the URL of the remote repository. Let's clone our `is310-test-repo`.

```sh
git clone git@github.com:ZoeLeBlanc/is310-test-repo.git
```

You should see something that looks like the following:

```sh
$ git clone https://github.com/ZoeLeBlanc/is310-test-repo.git
> Cloning into `is310-test-repo`...
> remote: Counting objects: 10, done.
> remote: Compressing objects: 100% (8/8), done.
> remove: Total 10 (delta 1), reused 10 (delta 1)
> Unpacking objects: 100% (10/10), done.
```

Now if we type `ls` we will see that we have a new directory called `is310-test-repo`. If we navigate into that directory, we will see that it has all the files from our remote repository.