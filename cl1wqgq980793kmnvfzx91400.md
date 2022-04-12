## Supercharge your Git History with better commit messages

---
title: Supercharge your Git History with better commit messages
published: true
description: A project’s long-term success rests (among other things) on its maintainability, and maintainers have few tools more powerful than the project’s log. It’s worth taking the time to learn how to care for your Git history properly. Here are some lessons I've picked up about writing Git commit messages that I think every developer should learn and implement.
tags: git, maintenance, project management
cover_image: https://thepracticaldev.s3.amazonaws.com/i/2zfm9olo1s8d2vkjrexe.jpg
---

Here are some lessons I've picked up about writing Git commit messages that I think every developer should learn and implement. Done right, it will increase the usefulness of your Git history and save your team a ton of time when maintaining code. 

## TL;DR

    Summarize changes in around 50 characters or less
    
    More detailed explanatory text, if necessary. Wrap it to about 72
    characters or so. The first line is treated as the subject of the 
    commit and the rest of the text as the body. The blank line 
    separating the summary from the body is critical (unless you omit 
    the body entirely); various tools like `log`, `shortlog` and 
    `rebase` can get confused if you run the two together.
    
    Explain the problem that this commit is solving. Focus on why you
    are making this change as opposed to how (the code explains that).
    Are there side effects or other unintuitive consequences of this
    change? Here's the place to explain them.
    
    Further paragraphs come after blank lines.
    
     - Bullet points are okay, too
     - Typically a hyphen or asterisk is used for the bullet, preceded
       by a single space
    
    If you use an issue tracker, put references to them at the bottom,
    like this:
    
    Resolves: #123
    See also: #456, #789

## Why good commit messages matter

A project’s long-term success depends (among other things) on its maintainability, and maintainers have few tools more powerful than the project’s log. When done right, the Git history can be a living, accurate and searchable record of how and why a project's code is the way it is. It will make reviewing the commits and pull requests of others easier to do without the need to consult them because it makes understanding why something happened months or years ago not only possible but efficient.

The ability to document code progression effectively is just as important as writing clean code or readable tests. Although code itself should be self-documenting, on its own it doesn’t tell the story of why the code is the way it is or how it came to be. A well-crafted Git commit message can communicate context about a change to fellow developers (and to your future self). A diff will tell you what changed but, when done well, the commit message can properly tell you why. Peter Hutterer put this point really well:

> Re-establishing the context of a piece of code is wasteful. We can’t avoid it completely, so our efforts should go to reducing it [as much] as possible. Commit messages can do exactly that and as a result, a commit message shows whether a developer is a good collaborator.

There is a vicious cycle here: if the commit history is unstructured and inconsistent, no one spends much time using or taking care of it. And because it doesn’t get used or taken care of, it becomes more unstructured and inconsistent.

It’s worth taking the time to learn how to care for your Git history properly. It may be a hassle at first but it will become habit quickly, and eventually a source of pride and productivity for all involved.

## Atomic Commits

Before getting into how to write a good commit message let's take a short look at the importance of making atomic commits. 

Large commits can be difficult to read, especially when they contain changes which are unrelated. It can be difficult to think about how you might split up a large commit, but a useful approach is to think about the purpose of each commit in the same way you might think about writing a checklist for completing the task. Each one should do one specific thing, if you find yourself wanting to use 'and' in your commit message, you're probably committing too much, split it. 

Think of atomic commits as the smallest amount of code changed which delivers value – whether it’s tidying up existing code or introducing a new (small) feature. Let's look at an example: 

    $ git log
    commit 42e769bdf4894310333942ffc5a15151222a87be
    Author: Sansa Stark <sansa@winterfell.com>
    Date:   Fri Jan 01 00:00:00 1982 -0200
       
    	Take the Iron Throne at King's Landing
    
    61 files changed, 937 insertions(+), 81 deletions(-)

In this example there are a lot of changes in this one commit. On the surface it might look like all these changes are related. However, if you were to break down the changes, you would have a better sense of what’s going on, for example:

    5feaccf Defeat Daenerys
    c829cbc Usurp Cersei
    a6455f8 Get hitched
    b529f6d Leverage on the Stark name
    b5bb6e4 Pander to Littlefinger for strategic reasons

Not only is each commit smaller, it also gives you a better story of what has happened to produce the final result. If a user wants to cherry pick specific updates or features they can do that much easier if the commits are atomic. 

A formal definition for an atomic commit is, "A single irreducible useful set of changes". The useful in that definition is very important. 

Now, on to the main part of the article. 

## Writing a great Git commit message

I'm sure we can all agree that the Git history often becomes useless when there is no convention and everybody does their own. To create a useful revision history, teams should first spell out their conventions on style, content and metadata of commit message. It's important to remove the guesswork on what commit messages should look like and make it all as simple as possible. The end result will be a remarkably consistent log that’s not only a pleasure to read but that actually does get read on a regular basis. 

Fortunately, there are well-established conventions for what makes a good and useful Git commit message. There’s nothing you need to re-invent. Just follow the guideline below and you’ll be on your way to committing like a pro.

### 1. Limit the subject line to 50 characters

50 characters is not a hard limit but a rule of thumb. Keeping subject lines at this length ensures that they are readable, and forces the author to think for a moment about the most concise way to explain what’s going on.

Tip: If you’re having a hard time summarizing, you might be committing too many changes at once. Strive for atomic commits (as explained above).

Most git tools are fully aware of this convention. They will warn you if you go past the 50 character limit and will truncate any subject line longer than 72 characters with an ellipsis. So shoot for 50 characters, but consider 72 the hard limit.

### 2. Capitalize the subject line

This is as simple as it sounds. Begin all subject lines with a capital letter. For example:

    Make a cup of coffee

Instead of 

    make a cup of coffee

### 3. Do not end the subject line with a period

Trailing punctuation is unnecessary in subject lines. Besides, space is precious when you’re trying to keep them to 50 chars or less. Example: 

    Make a cup of coffee

Instead of 

    Make a cup of coffee.

### 4. Use the imperative mood in the subject line

Imperative mood just means “spoken or written as if giving a command or instruction”. A few examples:

- Make a cup of coffee
- Turn on the computer
- Watch YouTube videos

The imperative can sound a little rude; that’s why we don’t often use it. But it’s perfect for Git commit subject lines. One reason for this is that **Git itself uses the imperative whenever it creates a commit on your behalf.**

For example, the default message created when using `git merge` reads:

    Merge branch 'newfeature'

And when using git revert:

    Revert "Add the thing with the stuff"
    
    This reverts commit cc87791524aedd593cff5a74532befe7ab69ce9d.

So when you write your commit messages in the imperative, you’re following Git’s own built-in conventions.

Writing this way can be a little awkward at first. We’re more used to speaking in the indicative mood, which is all about reporting facts. That’s why commit messages often end up reading like this:

- Fixed bug
- Changing behavior of button

And sometimes commit messages get written as a description of their contents:

- More fixes for broken stuff
- Sweet new API methods

To remove any confusion, here’s a simple rule to get it right every time. A properly formed Git commit subject line should always be able to complete the following sentence:

- If applied, this commit will "***Your subject line here"***

For example:

- If applied, this commit will "***Refactor subsystem X for readability"***
- If applied, this commit will "***Update getting started documentation"***

Notice how this doesn’t work for the other non-imperative forms:

- If applied, this commit will "***Fixed bug"***
- If applied, this commit will "***Changing behavior of Button"***

Remember: Use of the imperative is important only in the subject line. You can relax this restriction when you’re writing the body.

### 5. Separate subject from body with a blank line

From the git commit manpage:

> Though not required, it’s a good idea to begin the commit message with a single short (less than 50 character) line summarizing the change, followed by a blank line and then a more thorough description. The text up to the first blank line in a commit message is treated as the commit title, and that title is used throughout Git. For example, Git-format-patch(1) turns a commit into email, and it uses the title on the Subject line and the rest of the commit in the body.

Firstly, not every commit requires both a subject and a body. Sometimes a single line is fine, especially when the change is so simple that no further context is necessary. For example:

    Fix typo in introduction to user guide

Nothing more need be said; if the reader wonders what the typo was, they can simply take a look at the change itself, i.e. use `git show` or `git diff` or `git log -p`.

If you’re committing something like this at the command line, it’s easy to use the -m option to git commit:

    $ git commit -m "Fix typo in introduction to user guide"

However, when a commit merits a bit of explanation and context, you need to write a body. For example:

    Forget that the final GOT season ever existed
    
    The battle of winterfell turned out to be a major dissappointment 
    and did not live up to the hype. This commit deletes all evidence 
    that the final season ever happened, we prefer to just wonder how
    things were going to end that accept how they actually ended.

Commit messages with bodies are not so easy to write with the -m option. You’re better off writing the message in a proper text editor.

The separation of subject from body pays off when browsing the log. Here’s the full log entry:

    $ git log
    commit 42e769bdf4894310333942ffc5a15151222a87be
    Author: kit harington <kit.harington@got.com>
    Date:   Fri May 10 00:00:00 2019 -0200
    
    	Forget that the final GOT season ever existed
    
    	The battle of winterfell turned out to be a major dissappointment 
    	and did not live up to the hype. This commit deletes all evidence 
    	that the final season ever happened, we prefer to just wonder how
    	things were going to end that accept how they actually ended.

And now `git log --oneline`, which prints out just the subject line:

    $ git log --oneline
    42e769 Forget that the final GOT season ever existed

There are a number of other contexts in Git where the distinction between subject line and body will be useful but none of them will work properly without the blank line in between.

### **6. Wrap the body at 72 characters**

Git doesn't wrap text automatically. When you write the body of a commit message, you must mind its right margin, and wrap text manually.

The recommendation is to do this at 72 characters, so that Git has plenty of room to indent text while still keeping everything under 80 characters overall.

Some Git tools and text editors will already do this for you. 

### 7. Use the body to explain what and why vs. how

This commit from Bitcoin Core is a great example of explaining what changed and why:

    commit eb0b56b19017ab5c16c745e6da39c53126924ed6
    Author: Pieter Wuille <pieter.wuille@gmail.com>
    Date:   Fri Aug 1 22:57:55 2014 +0200
    
       Simplify serialize.h's exception handling
    
       Remove the 'state' and 'exceptmask' from serialize.h's stream
       implementations, as well as related methods.
    
       As exceptmask always included 'failbit', and setstate was always
       called with bits = failbit, all it did was immediately raise an
       exception. Get rid of those variables, and replace the setstate
       with direct exception throwing (which also removes some dead
       code).
    
       As a result, good() is never reached after a failure (there are
       only 2 calls, one of which is in tests), and can just be replaced
       by !eof().
    
       fail(), clear(n) and exceptions() are just never called. Delete
       them.

Think how much time the author is saving fellow and future committers by taking the time to provide this context here and now. If he didn’t, that context would probably be lost forever if he leaves.

Focus on making clear the reasons why you made the change, the way things worked before the change (and what was wrong with that), the way they work now, and why you decided to solve it the way you did. In most cases, you can leave out details about how a change has been made. Code is generally self-explanatory in this regard (and if it is so complex that it needs to be explained, then use source comments).

Try to look at the commit from the perspective of the developer maintaining your code. What questions might they ask when looking at your code changes? What might not be immediately obvious?

The future maintainer that thanks you may be yourself!

## Closing tips

### Revise history before sharing

When writing your code, you are bound to change direction or even make mistakes like introducing typos or bugs.

    324d079 Fix typo in enrolment flash message
    3a85f77 Only display enrol button for users who can enrol
    4cc4778 Allow users to enrol on courses

In this example, the developer has introduced a typo in their first commit, which they have fixed in a later commit.

Before you share your commit history, it’s important to think about what is useful information for someone else reading the history. You shouldn’t think of your Git history as a “truthful” log of what you worked on step-by-step. Just as we refactor code, we should refactor our commits before sharing them with others so that we don't clutter the history.

Git has powerful tools that make it simple to re-order, reword and refactor your commits until they tell the clearest story possible. Use Git's interactive rebasing functionality to tell a clearer story:

    $ git rebase -i
    
    3a85f77 Only display enrol button for users who can enrol
    773e345 Allow users to enrol on courses

### Read Pro Git

Check out the [Pro Git](https://git-scm.com/book/en/v2) book, it's available online for free.

## Acknowledgements

This article strongly derives from Chris Beams' [article](https://chris.beams.io/posts/git-commit/), Seb Jacobs' [article](https://about.futurelearn.com/blog/telling-stories-with-your-git-history), Sebastian Feldmann's [talk](https://www.youtube.com/watch?v=f-Br8cud2eI) and my own experience maintaining projects with less that helpful Git histories.