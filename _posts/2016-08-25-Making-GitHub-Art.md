---
layout: post
title: "Making GitHub Art"
author: bryan
date: 2016-08-25
comment: true
categories: [Articles]
published: true
noindex: false
---

## Background
The contribution heatmaps on GitHub profiles are interesting. Although they are intended to be passive data visualizations, they don't have to be. Specifically, they can act as a 7xN pixel -*very slowly*- scrolling display. After realizing this, I decided I had to do something to shape the blank canvas that is my GitHub commit log.

>"An artist is somebody who produces things that people don't need to have."
― Andy Warhol

## The plan
Ostensibly, it should be pretty straightforward. The color of each cell of the heatmap is based on the number of commits made that day, so one just needs to automate the appropriate number of commits per day to get the desired shading. For simplicity, I decided to start by using the darkest shade possible to build some text.

## The execution
And to be honest, it pretty much *was* that simple. The most difficult part was finding a Python library to automate the git commits. Many StackOverflow discussions essentially suggested rolling your own functions because it is relatively simple and flexible. Had I been building something I cared about more, that might have been the way to go, but I was determined not to spend more than a few minutes on this project and I didn't need a lot of flexibility. I really wanted to find something off-the-shelf with good documentation.

#### Connecting to GitHub
I tried a few valiant entries into the Python/GitHub API space, but what some lacked in functionality the others lacked in documentation. Finally, I tried [github3.py](https://github3.readthedocs.io/en/develop/) and found the right mix. Without too much trouble, I was able to automate connecting to GitHub and making commits. After a little research it looked like ~40 commits per day would be enough to keep the color scaling the way I wanted it.

There is a link to the GitHub repo at the end of this post. These are the main functions for connecting and committing to GitHub:

```python
from github3.py import login
import time

# Login helper
# Comma separated credentials are stored
# in the first row of auth.csv.
def github_login():
    with open('auth/auth.csv', newline='') as f:
        text = csv.reader(f)

        for row in text:
            user_name, password = row

    session = login(user_name, password)

    return(session)

# The function that submits the commits.
# The number of commits should be set to
# something quite a bit higher than your
# normal number of daily commits. Changing number_of_commits
# may also require changing sleep_time
# so that things still complete in a reasonable
# amount of time.
def do_typing(num_of_commits=30, sleep_time=20):

    me = github_login()

    repo = me.repository('your_github_username', 'GitHubTyper')

    for i in range(num_of_commits):
        # Create a file
        data = 'typing file'
        repo.create_file(path = 'files/dotfile.txt',
                         message = 'Add dot file',
                         content = data.encode('utf-8'))

        # Get the file reference for later use
        file_sha = repo.contents(path = 'files/dotfile.txt').sha

        # Delete the file
        repo.delete_file(path = 'files/dotfile.txt',
                         message = 'Delete dot file',
                         sha = file_sha)

        time.sleep(sleep_time)
```

#### Translating letters to useable format

With a way to connect in hand, the code needed to know *when* to connect. Basically, I needed an on/off switch for every day represented on the heatmap. If the switch is on, the committing function should run, making the cell dark. If it is off, the committing function shouldn't run, leaving the cell gray (or close to it, depending on what other commits are made that day).

Since we're using the heatmap to display text, a [matrix-based](https://www.google.com/search?q=dot+matrix+font&tbm=isch&tbo=u&source=univ&sa=X&ved=0ahUKEwjTwoCu19zOAhUMCcAKHWYZD2cQsAQIHQ&biw=1205&bih=673) font seemed to make sense. If you've seen dot-matrix font styles, these will look familiar. Each position in the matrix corresponds to a day on the heatmap. I used values of '1' and '0' to indicate on and off days, respectively. (And technically these are lists, not matrices, but they are laid out like matrices to make them easier to create.)

As an example, here is the setup for the letter 'A':

``` python
letters_dict = {
'A' : [0,1,1,1,0,0,
       1,0,0,0,1,0,
       1,0,0,0,1,0,
       1,0,0,0,1,0,
       1,1,1,1,1,0,
       1,0,0,0,1,0,
       1,0,0,0,1,0],
...
}
```
These matrices are time consuming to create, so I've only created the few that I needed. If you make more feel free to send them along via pull a pull request.

#### Automating and scheduling the runs
Now that I had a way to do commits programmatically and something to commit, I needed a way to schedule the Python script to run at the appropriate time. The ultimate goal was to be able to tell the script what I wanted to do at the beginning and have it run unsupervised for a few weeks until it completed.

This was achieved with PythonAnywhere.com and a little bash script. Python Anywhere is a Python-oriented hosting environment. Among many other things, it can be used to schedule Python scripts to run at certain times of the day. A free account allows one daily task and http calls to a whitelist of domains. Fortunately, one task is all we need to run and GitHub.com is on the whitelist.

After uploading the Python code, I created a really simple bash script that calls the main Python script and is scheduled to run daily:

``` sh
#!/bin/sh
python3.5 GitHubArt/main.py '$echo "Hi"' '2016-07-31'
```

And that's all. The first parameter is the message to display on the GitHub heatmap, the second is the date on which to start typing. Since the GitHub heatmap starts with Sunday at the top, this date should also be a Sunday.

## Philosophical Implications
I took a very obvious approach for someone with no artistic talent - I am using this functionality to print out a \*nix command. [Christo](https://en.wikipedia.org/wiki/Christo_and_Jeanne-Claude) would not be impressed. Honestly, though, the prospect of using this to make art seems really cool. Given that the intensity can differ for each cell in the heatmap, it is essentially as versatile as a grayscale palette. If I had an artistic bone in my body I might give it a shot. For now, I'll just use text and appreciate my simple creations as a Buddhist would, for their intrinsic and ephemeral beauty.

*You can find all the project files here:
[https://github.com/bryancshepherd/GitHubArt](https://github.com/bryancshepherd/GitHubArt)*
