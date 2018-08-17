# Git Commit Descriptions and Amends

`git` is a powerful and complex CLI that manages versions of your code. This lesson explores using the `git commit` message flag twice in one commit, and `git commit --amend`.

## The Manual

It is helpful to utilize [`git` documentation](https://git-scm.com/doc) often as you grow as a developer. Sometimes it is even easier to launch `git` documentation in your shell session. Let's do this right now for `git commit`, using the `--help` option. You can pass the `--help` option to any `git` command for more information.

```
git commit --help
```

Take a few minutes to read about some of the ways one can pass additional options and configuration to `git commit`.

---

## Git Descriptions

To commit our code, we have been using `git commit -m "subject"`, where `-m` is an alias for `--message`. Adding a description to a commit is just as simple. Instead of using the `-m` once, we use it twice.

```
git commit -m "Short subject of commit" -m "Longer, detailed explanation of why this commit is important to the application. This section should not be aimed at being short, it should strive to be thorough. The description is a great place to link documentation or a StackOverflow answer that describes why you took this approach in the commit."
```

## Why

Creating logical commits facilitates an easier code review for our team. This means they do a better job understanding your changes, offer better feedback, and make it easy for them to detect a possible bug, which can be a life saver for our application. It also creates a better git history for our project. A few weeks or months from now a developer may need to look back at the git history to identify how a feature was added.

## Git Amend

We just got done commiting some changes, but realize that we forgot to add one more line of functionality. What do we do?

`git` provides a command for just this scenario. `git commit --amend` will tack on more staged changes into the last commit we added.

### Example

For our mammal PR, we are fixing the `#charge` method on our Rhino model. We are about to commit a change that looks like:

```
$ git diff
diff --git a/app/models/rhino.rb b/app/models/rhino.rb
index f802ba8..9a348fe 100644
--- a/app/models/rhino.rb
+++ b/app/models/rhino.rb
@@ -2,6 +2,7 @@ class Rhino
   attr_reader :tusks

   def charge
+    stomp
     rear_head
     point_tusks
     run
```

But our team does not understand why a rhino starts it's charge by stomping. Let's fill them in with a commit description:

```
$ git add app/models/rhino.rb

$ git commit -m "Add stomp to Rhino#charge" -m "For more info on why our Rhinos need to stomp, check out: https://animals.howstuffworks.com/mammals/do-rhinos-really-stomp-out-fires.htm"
```

Perfect!

In Github, my commit shows that it has a description with a `...`:

![Commit with Description](https://raw.githubusercontent.com/powerhome/phrg-git-commit-messages-and-amends/master/commit-with-description.png?raw=true "Commit with Description")

And when a user clicks on the `...` it opens up with our text:

![Opened Commit Description](https://raw.githubusercontent.com/powerhome/phrg-git-commit-messages-and-amends/master/opened-commit-description.png?raw=true "Opened Commit Description")



