# Interactive Git Tutorial
This tutorial serves as a way to get your hands dirty using git. The branching
model used here is based on http://nvie.com/git-model

## Overview
You will begin the tutorial by cloning this git repository. From there, you
will develop a simple PHP application with a few features. This guide will
walk you through each step as you create feature branches, RC branches,
and complete a 'production' release of the app.

## Step 1: Fork This!
Let's get started! First you'll need to make sure your system has git installed,
that you have a GitHub account, and that you've added your local SSH key to
your GitHub account.

Fork this repository on GitHub, and then follow the instructions to clone your
new repo on your development machine.

## Step 2: Create Your `next` Branch
You should now be in your cloned `git-tutorial` repository. Let's see what branches
you have:
    $ git branch
    * master
	
Right now there's just one branch, `master`, and you're on it (as denoted by the \*).
We want a branch called `next` where you'll develop the next release, so let's create
it:
    $ git branch next
    $ git branch
    * master
      next
	
Now we've got a `next` branch, but we're still working in `master`. Switch to `next`:
    $ git checkout next
    Switched to branch 'next'
	
Finally, let's push the `next` branch to `origin` so other developers can use it:
    $ git push origin next
	
Great! Now we can get down to business.

## Step 3: Check out the Movie Plot App
Now that you're ready to develop, it would probably be good to know what you're working
on. Let's run the app to see what it does:
    $ php movie_plot.php
    He's a fat goat wrangler with a lust for antiques. She's a sadistic dentist with a 
    penchant for melodrama. They fight crime.
	
Ok, so it's a PHP script that generates amazingly profitable movie plot ideas. Take a
moment now to browse through the code for `movie_plot.php` to see how it works.

It's a pretty straight-forward script. There are arrays for adjectives, occupations,
and quirks, which the script puts together Mad Libs style to get an action movie plot.

## Step 4: Feature Development
Your bosses at the movie studio have 3 new features they want you to develop. You can do 
these in any order, and you'll want to use feature branches to keep your work organized.
You won't know until the end of this exercise which features will actually be included in
the next release.

To create a feature branch, first think of a name and then create it:
    $ git checkout -b randomness
    Switched to a new branch 'randomness'
**Note:** we used `git checkout -b` to both create the branch *and* start working in it.)

Do your work in `movie_plot.php`, and then commit it:
    $ git commit -a -m "implemented randomness change"
**Note:** the -a option will commit all modified files.

Once you're done working on a particular feature branch and you've committed your changes,
you can start the next feature by branching off of `next` again:
    $ git branch new_branch next
	
When all your feature branches are done, move on to the next step.

### Feature: Improve Randomness
The execs have noticed that some plots come up more often than others. They've seen one
too many buddy cop flicks, and they want you to fix it! The reason is that the script
is using PHP's rand() function, when it should really be using the improved mt_rand()
function instead. For this task, you need only replace rand() with mt_rand().

### Feature: More Content
Right now, the choices for adjectives, occupations, and quirks are a little sparse.
For this task, add at least 1 more choice to each array.

### Feature: Prevent Duplication
Sometimes the script will generate a plot where both lead characters have the same
quirk or occupation adjective. Talk about boring! Modify the script so that her
quirk, occupation, and adjective is never the same as his.

## Step 5: CRITICAL BUGFIX!!!
Oh no! The studio execs have noticed a typo in the movie plot generator! Instead of
'grisly', the program has been creating 'girsly' characters! The screenwriters are
threatening to go on strike, so you'd better fix this fast.

Since we don't have time to wait for the next release for this fix, we will create
a hotfix branch:
    $ git checkout -b hotfix-0.1.1 master
(Notice that we branched directly from `master` so we can deploy this fix to production.)

Go fix the typo in `movie_plot.php`, and then commit it:
    $ git commit -a -m "fixed typo"
	
Now you need to merge that hotfix back into `master` and `next`, so it can be deployed
to production, and also included in future releases.
    $ git checkout master
    $ git merge --no-ff hotfix-0.1.1
    $ git tag -a 0.1.1
    $ git checkout next
    $ git merge --no-ff hotfix-0.1.1

Now that we're done with that hotfix branch, we can delete it:
    $ git branch -d hotfix-0.1.1
	
This fix needs to go live now, so push your `master` branch back to `origin`:
    $ git push origin master
	
And push `next` so other developers get that backported fix:
    $ git push origin next

Phew! Now the operations team can push the app into production. Good work!

## Step 6: Prepare for Release
Your bosses have decided they are ready for a new release of the movie plot app.
They only want to include the randomness improvement and the duplication prevention
features, since they still need the studio execs to vet the content you added.

Go ahead and merge the appropriate feature branches into `next`, and delete each
feature branch once it's merged:
    $ git checkout next
    $ git merge --no-ff randomness
    $ git branch -d randomness
    $ git merge --no-ff dedupe
    $ git branch -d dedupe
**Note:** make sure you use the branch names you chose.
**Note:** we used the --no-ff option to prevent a fast-forward merge. This way git
will preserve the history of the feature branch.

If you have conflicts, you will need to edit `movie_plot.php` to resolve them,
use `git add` to add the file to the index, and then commit.

Now that `next` reflects a state of the app that is ready for acceptance testing,
we'll create a release-candidate branch:
    $ git checkout -b rc-0.2.0 next
	
Test the application to make sure it is all working as expected. If there are bugs,
fix them and commit the fix directly to `rc-0.2.0`. Remember that only bugfixes
should be committed to an rc branch!

## Step 7: Push the Release
Our new version is ready to go live! Just merge the rc branch into `master`:
    $ git checkout master
    $ git merge rc-0.2.0
	
At this point you can tag `master` with the new version number:
    $ git tag -a 0.2.0
	
Then push the new release back to the `origin` server:
    $ git push origin master
	
If you made any fixes in the rc branch, merge it back into `next`, then push:
    $ git checkout next
    $ git merge rc-0.2.0
	$ git push origin next

Lastly, delete the rc branch since we're done with it:
    $ git branch -d rc-0.2.0
	
## Conclusion
Aside from having a top-notch movie plot generator, you should also now have a pretty 
good feel for how to work with git and this particular branching model.

## Resources
A successful Git branching model: http://nvie.com/git-model
Understanding Git Conceptually: http://www.eecs.harvard.edu/~cduan/technical/git/
Git Documentation: http://git-scm.com/documentation