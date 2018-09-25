# Code Review
I am working with Dr. Amelung's research group at the Rosenstiel School of Marine and Atmospheric Science (RSMAS) to improve their software development processes. This document should serve as a How To for using Code Review on Dr. Amelung's team or setting it up on your own team!

## Prerequisites
##### TODO

## What is Code Review?
Code Review has become an industry standard amongst software teams within the past 5-10 years. 

When a developer makes a code modification (adding a new feature, refactoring old code, etc), another developer will read over the code before it is committed to the `master` repository and comment on coding style, any logical errors, and different or better approaches to the same problem. After the second developer has commented, the first developer will go back and fix any suggestions made. 

A key aspect of Code Review is that the reviewer can block the code writer from merging the changes to `master` before the comments the reviewer made are put into place.

We will get more into the process of how this works, but let's first dive into why do this in the first place.

## Why Code Review?
Code Review **enables knowledge to be shared across multiple members of a team**. When a developer reviews another developer's code, the reviewer learns about another part of the codebase. Also, as a developer writing code, your reviewer may have more knowledge about that subsystem than you and might see a better way to approach the problem.

Code Review also **ensures a codebase converges to a consistent style**. People tend to have different coding styles which can make it more difficult to understand a codebase. If a whole codebase has the same style, I can understand any part of the codebase more quickly. Consistent style improves readability and Code Review encourages developers to first converge via discussion on a consistent coding style and then maintain a more-uniform style.

Code Review **increases code legibility**. If one person _must_ read your code before you commit it, you will know your code is at least legible to one person. Code reviewers will (in a good Code Review process) push for more legible solutions.

So in the end, Code Review should lead to a cleaner codebase with more people understanding the codebase. Although initially Code Review slows down the developer process (you can't just `push` to `origin:master`!), the speed of the team will increase. You, a developer, will have knowledge about more subsystems and will be able to move more freely around the codebase. You will also be able to iterate on your coding abilities more quickly by getting more feedback from those around you.

## Code Review Guidelines
Code Review can improve your efficacy as an individual and the output of your team, however the process can become impractical if executed haphazardly. Here are some guidelines on how to have an effective Code Review process.

### Guidelines for Code Writers

#### 1. Review before merging with `master`
You should (almost) always go through the review process before merging your code with `master`. For really small changes or mission critical changes, it is okay to commit right to master. However, for any non-critical update, it should go through code review. This encourages a culture where you put your thought and time into each commit and may lead to fewer errors and thoughtful code over time. It also means that all code written and updated will be seen by at least 2 people and that your reviewer can stay up to date on your progress.

As part of the review process, your code reviewer will often ask for you to make changes to your code. This is a great forum to discuss stylistic approaches, trade-offs, etc. However, both the reviewer and the reviewee must agree that the code is "good to go" before merging your changes with `master`.

#### 2. Review your own code before asking others to review your code
Reviewing your own code is a great skill to hone. Thoroughly reviewing your own code line by line will help you catch bugs and style issues before anyone else even looks at your code. This also respects the time of your peers; submitting unpolished code for review wastes the time of your reviewer. Instead of just focusing on the BIG issues, your reviewer also has to comment on your commented code that should be deleted, your unused functions, or your personal code comments.

#### 3. Keep your changes as small as possible, but no smaller
A common phenomenon is that larger commits are less likely to be well-reviewed. It's hard to read and understand lots of code. Your code will receive better feedback and your reviewer will be happier ( :D ) if you break one LARGE commit into multiple smaller commits. 

A good rule of thumb is to keep each commit to 50-100 lines.

#### 4. Add titles, summaries, and comments to your commits
Although code itself should make sense on its own, oftentimes more context on the problem at hand is needed. Provide background information, context on the problem, and details on why this implementation was chosen in your commit. 

Also, title your commits! Git will show ~40 characters for the subject of a commit. Below the subject, using 2 new lines, provide a summary or a couple of paragraphs on your commit. This longer response will be useful not only for your reviewer but for anyone looking back at your code changes.

And of course, comment your code! These comments should provide meaningful information that one would not be able to pick up from just reading the code itself. For more info on writing good comments, check [this](https://blog.codinghorror.com/code-tells-you-how-comments-tell-you-why/) out.

#### 5. Remember: You are interested in seeing your code *im*proved, not just *ap*roved
It's tough when you've put in 30 minutes to 2 hours into a commit and then someone asks you to redo part or all of it. However, it's all about the process. Good software developers are process-driven over outcome-driven. We need to ship code, but we want to ship the best code we can. Remember that you and your reviewer are both looking to accomplish the same outcome. Your reviewer is your best resource to improving your code and your coding process.

### Guidelines for Code Reviewers
TODO

## Code Review on GitHub
GitHub supports Code Review via Pull Requests. Here are a couple of resources I found useful to understanding the Pull Request flow: [About Pull Requests](https://help.github.com/articles/about-pull-requests/) and [GitHub Code Review](https://github.com/features/code-review/).

Although you should be doing this already, you are required to create a separate branch with your changes before creating a Pull Request and then `push` this new branch to `origin`. Some of this is automated for you using a tool called `hub` (discussed below), however I would strongly suggest that you [understand branching](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging). Branching gives you super powers.

### Setting Up Pull Requests from the Command Line
You can [create Pull Requests](https://services.github.com/on-demand/github-cli/open-pull-request-github) from GitHub's online client. However, it is super useful to create Pull Requests from the command line. Here's how:

1. Install [Hub](https://github.com/github/hub). This can be done on a Mac by using `brew install hub` or on a Linux Machine as follows:
	1. `wget -O hub-linux.tgz https://github.com/github/hub/releases/download/v2.5.1/hub-linux-386-2.5.1.tgz`. The link to the compiled distribution may have changed. Look [here](https://github.com/github/hub/releases) for more info.
	2. `tar -xvf hub-linux.tgz`
	3. Add an alias to your `.*rc` file of choice. For example, in your `~/.cshrc` file, you would add `alias hub '~/hub-linux/bin/hub`
		- NOTE: This is not the optimal way to do this. We can't add it to a `bin` directory on Pegasus. I need to reach out to CCS to figure out a good standard on how to do this
	4. `hub`: Test to see if `hub` works
2. Add `hub` to your `git` command. You can do this by adding the following 2 lines to your `~/.gitconfig` file:
```
[alias]
	hub = !hub
```
3. Done! You can test that this all works by running `git hub` from an arbitrary folder

Note that `hub` can completely replace `git`. Some people like to do this by aliasing `hub` to `git`, so that whenever you type in `git`, `hub` runs. This works as `hub` is a wrapper on top of `git`, however it can make unclear what program is doing what. Read more [here](https://news.ycombinator.com/item?id=14429450).

### Creating a Pull Request from the Command Line
1. Check out `master` and make sure you have pulled the latest codebase: `git checkout master && git pull`
2. Create a new branch and check it out: `git branch my_branch && git checkout my_branch`
	1. Equivalently, you could do the following: `git checkout -b my_branch`. This creates a branch and checks it out in one step
3. Make changes to your current branch and commit them: 
```
git add file1.py file2.py 
git commit 
```
4. Create a Pull Request! `git hub pull-request --push`. This creates the Pull Request *and* pushes a new branch to the remote (GitHub in our case)
	- You can do this in two separate steps too: `git push origin -u my_branch && git hub pull-request`
5. Head to your GitHub repository online and check out your Pull Request!
