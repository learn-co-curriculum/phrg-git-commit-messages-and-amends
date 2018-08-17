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

`git` provides a command for just this scenario. `git commit --amend` will tack on more staged changes into the last commit we added. After our commit is modified, we will have to push with force to update it on Github.

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

$ git push origin rhino_branch
```

Perfect!

In Github, my commit shows that it has a description with a `...`:

![Commit with Description](https://raw.githubusercontent.com/powerhome/phrg-git-commit-messages-and-amends/master/commit-with-description.png?raw=true "Commit with Description")

And when a user clicks on the `...` it opens up with our text:

![Opened Commit Description](https://raw.githubusercontent.com/powerhome/phrg-git-commit-messages-and-amends/master/opened-commit-description.png?raw=true "Opened Commit Description")

Oh wait!! We forget that our Rhinos also growl! Let's ammend this change to our last commit:

```
$ git diff
diff --git a/app/models/rhino.rb b/app/models/rhino.rb
index 9a348fe..cfa730f 100644
--- a/app/models/rhino.rb
+++ b/app/models/rhino.rb
@@ -4,6 +4,7 @@ class Rhino
   def charge
     stomp
     rear_head
+    growl
     point_tusks
     run
   end
```

```
$ git add .

$ git commit --amend
[rhino_branch 3c5ea2e] Add stomp to Rhino#charge
 Date: Fri Aug 17 10:13:19 2018 -0400
 1 file changed, 2 insertions(+)

$ git push origin rhino_branch
To github.com:garettarrowood/app.git
 ! [rejected]        rhino_branch -> rhino_branch (non-fast-forward)
error: failed to push some refs to 'git@github.com:garettarrowood/app.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Uh oh! We forgot that since we overwrote a commit, we have to `--force` our push up to Github. Let's try one more time:

```
$ git push -f origin rhino_branch
Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (5/5), 557 bytes | 557.00 KiB/s, done.
Total 5 (delta 0), reused 0 (delta 0)
To github.com:garettarrowood/example_app.git
 + 1e71c2a...3c5ea2e rhino_branch -> rhino_branch (forced update)
```

In the last line in the above snippet, notice that commit changed from `1e71c2a` to `3c5ea2e`. Let's look on Github to see what happened there:

![New Git Sha](https://raw.githubusercontent.com/powerhome/phrg-git-commit-messages-and-amends/master/new-git-sha.png?raw=true "New Git Sha")

Great! We've created an accurate and informative commit!

## Resources

- [`git` documentation](https://git-scm.com/doc)
- [`git commit --message` documentation](https://git-scm.com/docs/git-commit#git-commit--mltmsggt)
- [`git commit --amend` documentation](https://git-scm.com/docs/git-commit#git-commit---amend)
