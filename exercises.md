# Prerequisites

- [Install git on your machine](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Upload your public key to Github](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account)

# Exercises

## Config

```sh
git config user.email <YOUR_EMAIL_ADDRESS>
git config user.name <YOUR_NAME>
```

## Cloning

Fork then clone [this repository](https://github.com/syk0saje/git-gud)

```sh
git clone git@github.com:<YOUR_GITHUB_HANDLE>/git-gud.git
git log
git show
```

## Review and merge a feature branch

```sh
git branch --all
git checkout fix-diff-syntax
git log
git show
```

After you've made sure that everything is in order, it's time to merge the
feature branch into master.

```sh
git checkout master
git merge fix-diff-syntax
```

### Realize a mistake too late

Uh oh! Looks like the commit message from the branch we just merged does not
follow the formatting rules. It's a good thing we haven't pushed yet. Let's fix
that:

```sh
git commit --amend -m "Fix `git diff` syntax"
```

### Push and delete merged branch

Now we can push to `master`!

```sh
git push origin master
git branch -d fix-diff-syntax
```

Go to Github and delete the `fix-diff-syntax` branch after checking that it's
been merged into `master`. Then, delete the remote-tracking branch on your local
repository:

```sh
git fetch -p
```

### Update other branches

Let's go ahead and look at the other branch, `exercises`:

```sh
git checkout exercises
git log
```

Oh no! It looks like it still has the old commit that we just amended. How can
we fix this? Well, it looks like this branch has only added one commit on top of
the broken one, in which case we can use `cherry-pick`:

- First, take note of the commit hash that we want to apply. Use `git show` and
save the hash for "Add exercises.md" somewhere. We'll use it later

- Next, `reset` the branch to use the correct version of the commit:

  ```sh
  git reset master
  ```

- Lastly, let's replay the commit that we saved earlier:

  ```sh
  git cherry-pick <COMMIT_HASH_YOU_SAVED_EARLIER>
  ```

Voila! `exercises` should now be using the corrected commit. Let's push this to
Github:

```sh
git push origin exercises
```

Oops! This isn't a fast-forward so Github is complaining about our push. But we
are sure that we want to overwrite the version of this branch on Github and the
only other person working on this branch is Bob and he probably won't lose any
work because he is a lazy asshole. So let's go ahead and force push:

```sh
git push origin exercises -f
```

## Git some work done!

Looks like this repo could use a readme. Whoever made this had their priorities
all mixed up. It's time to contribute!

```sh
git checkout -b add-readme
```

Examining the code, it looks like you need to run `run.sh` to use the program.
Try it out!

```sh
./run.sh
```

Looks like it works! Copy and paste this into a `README.md` file:

```text
# Instructions

1. Run `readme.sh` for great justice.
1. ???????
1. PROFIT!!!1
```

Now let's stage the stuff and check if we're ready to commit:

```sh
git add --all
git status
```

Hang on, what is that `top-secret.config`? THAT HAS SENSITIVE INFORMATION! It
was probably (definitely) made by running the program! We need it here but we
don't want to add it to the repository. Let's unstage it, then.

```sh
git reset top-secret.config
```

Let's also create a `.gitignore` file so that it can't be accidentally committed
in the future. Make a `.gitignore` file and write this in it:

```text
top-secret.config
```

Now, even if you `git add --all`, the file won't be included! Go ahead and try
it out yourself.

Finally, we can commit, push, and file a pull request:

```sh
git commit -m "Add README.md and .gitignore"
git push origin add-readme
```

Go to the `add-readme` branch on Github and file a pull request!

## Git some work undone!

Hang on, it looks like that "Remove excess tildes" commit broke the markdown
formatting! It's pretty deep in the history though and the commit is very simple
so we can use `revert` in this case.

First, we need to take note of the commit that we want to revert. Use `git log`
and take note of the commit hash of the "Remove excess tildes" commit.

```sh
git checkout master
git checkout -b revert-tildes
git revert <COMMIT_HASH_YOU_WANT_TO_REVERT>
git push origin revert-tildes
```

## Erase History

Let's pretend that our history is too embarrassing to share but we want to keep
using git. We can forget everything and start from scratch, as if all this code
came into existence just now!

```sh
rm -rf .git
git init
git add --all
git commit "Restart history"
git log
```
