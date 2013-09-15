---
author: sunilkumarn
comments: true
date: 2011-12-18 08:42:41+00:00
layout: post
published: false
slug: git-a-brief-on-reverts-resets-merges-and-conflicts-2
title: Git -> A brief on Reverts, Resets, Merges and conflicts.
wordpress_id: 486
---

Git, as we know is a fast, open source, distributed version control system that is quickly replacing subversion in open source and corporate programming communities. As a developer, many a times i have been amazed by the power of git and how it takes care of our code repo. It track files in the project, we periodically commit the state of the project when we want a saved point. The history of our project is shared with other developers for collaboration, merge between their work and ours, and compare or revert to previous versions of the project or individual files.   
  
As mentioned earlier, Git, at a fast pace, is replacing subversion in open source and corporate programming communities. Hence most open source developers would have had a taste of git and its power. We all would have done a git init, push, pull, rebase and stuff in our day to day programming activity and those would be quite trivial to most developers.   
  
However they are certain facets of git(merges, conflicts, reverts and such) which does create some kind of trouble to developers, atleast when they use it for the first time. What made me write down this post is an incident that happened to my collegue while he was on work. Will get into that shortly.  
  
First, let me just put in a bit on **Reverts and Resets** done in git.  
  
Git provides us multiple methods for fixing up mistakes while in development mode. This is important, because it saves not just our work but the others who are involved in the same project.  
  
If you have actually done a mess with your working directory, but actually haven't commited the changes, the best way is to perhaps do a hard reset.

$ git reset --hard HEAD  
  
This would just wipe off the changes that you have made in your git index and also any outstanding changes that you have made in your repo.  
  
Now suppose you have commited your changes, but haven't pushed it into master,  and then suddenly you feel like you shoudn't have made the previous commit(or a sequence of your previous commits), you could again reset hard. This is as simple as doing   
  
$ git reset --hard HEAD~n.   
  
This would set the HEAD of the git index to 'n' commits prior to your current head.  
  
The problem with doing a git reset --hard is very obvious now.   
  
o -> o -> o -> D -> C -> B -> A.  This is what your commit log looks like with at A its head. Suppose you do   
  
$git reset --hard HEAD~3 .   
  
Now your commit log would be.  
  
o -> o -> o -> D  
  
This means that the changes that you made right from A to C have been vanished and you are not going to get it back.   
The bottom line is simple. You are not able to change the effects made by a single commit(ofcourse, the exception is your last commit as we have already seen).   
  
git-revert is just for that.   
  
Commit log: o -> o -> o -> D -> C -> B -> A  
  
At any point of time, you realize that 'C' is bound to break your code(hopefully it still hasn't), you may well want to undo the changes made by C. This could be done by   
  
$git revert (commit id of C).   
  
This would create a new commit that undoes the commit C. You will be given a chance to enter a new commit message, but the default message that indicates its 'the reverse of the commit C' would be the most indicative commit message to have.   
  
o -> o -> o -> D -> C -> B -> A -> rC  
where rC is the reverse of C.   
  
This revert is a straightforward revert(i.e. it just undoes the data made by the commit reverted). Since all thats being talked about is a single branch, there aren't any complications that would arise here.  
  
Now let me talk about the incident that i had mentioned ealier.  
What had happened what this.   
  
My friend did this:  
$ git pull origin experimental  
  
while he was still sitting in his master branch. The master branch and the experimental branch have now merged. This was totally unintentional(he never planned to do a merge). There were no merge conflicts however. The mainline code broke. We had to revert this faulty merge.   
  
Master - >          P  ->  x ->  M   
                                        /  
                                       /  
Experimental ->        A -> B      
  
This would give you a picture. P is the point of branching. x is some commit made in the mainline branch totally unrelated to the side line branch.   
The side line branch itself has got two commits of its own, A and B. M is the merge commit (experimental has been merged with master).   
The code broke.   
  
We need to revert M(the merge commit).   
  
Master - >          P  -> x ->  M -> W  
                                        /  
                                       /  
Experimental ->        A -> B  
  
Now as seen, the merge has been reverted(W is the reverse of M). This was done with   
$git revert -m 1 (Commit id of M).

This adds W to the commit log as well.  
  
Now the faulty code in the experimental branch was worked upon and fixed and its been made ready for the merge. The experimental branch is now merged with the master branch.   
What was weird(for us, at that point of time) and noticeable was that the code changes that were made after the 'merge revert' appeared in the master branch whereas the ones made before the revert didn't appear. i.e.   
  
Master - >          P -> x -> M -> W -> x -> x -> M2  
                       
Experimental ->        A -> B ------- C -> D  
  
Again, x are the commits unrelated to the experimental branch. M2 is the second merge. Commits in the experimental branch,C and D, fixes the faulty code in A and B. Whats to be noticed is that, after the updated experimental branch has been merged, none of the changes made by A and B would appear in the master branch, whereas the changes made in C and D would.  
  
The reason was found out soon.  
  
Linus Torvalds explains the situation:  
  
    _Reverting a regular commit just effectively undoes what that commit _  
_    did, and is fairly straightforward. But reverting a merge commit also_  
_    undoes the _data_ that the commit changed, but it does absolutely_  
_    nothing to the effects on _history_ that the merge had._  
  
_    So the merge will still exist, and it will still be seen as joining_  
_    the two branches together, and future merges will see that merge as_  
_    the last shared state - and the revert that reverted the merge brought_  
_    in will not affect that at all._   
  
Thats what just happened here. W(merge revert) undoes the data made by M(merge) but does nothing to the commit history brought in by M.   
There fore when the second merge,M2, is made, the commit history is checked and M is found to be 'last shared state'. Hence, only those changes that has been made after the 'last shared state', M, will be merged into the master branch now(i.e. commits C and D). None of the data created in A and B would merge, because as per the commit history, they are already merged.   
  
Solution to this problem is also explained by Linus himself. The fix is to 'revert the revert that brought in W', i.e, revert W before you do in the next merge,M2.  
  
Thus the main line commit log would be   
P -> x -> M -> W -> x -> x -> Y -> M2.   
  
where Y is the reverse of W and M2 is the merge made after that.  
  
$ git revert W   
  
adds Y to the commit log.  
  
The above commit log would be equivalent to P -> x -> M -> x -> x -> M2. (where there is no W nor a Y and then the second merge has been performed, M2). Now this would be fine, and all the changes made in the experimental branch should be seen in the master branch(ignoring merge conflicts). If there are any merge conflicts arising, git leaves the index and the working tree in a special state that gives us all the information needed to resolve the merge.   
  
A merge conflict would throw in the following message:   
CONFLICT (content): Merge conflict in sample_script.rb   
Automatic merge failed; fix conflicts and then commit the result  
  
Trying to switch to the experimental branch would give you this  
error: you need to resolve your current index first  
  
The files with conflicts will have markers upon them.  
  
<<<<<<< HEAD:sample_script.rb  
"We would be starting off now"  
=======  
"This would be the end"  
>>>>>>> d31f96832d54c2702914d4f605c1d641511fef13:sample_script.rb  
  
Now we need to resolve these conflicts manually followed by adding the file and commiting.   
  
$ git add sample_script.rb  
$ git commit -a  
  
The commit message would already be filled in indicating that its a conflict resolving commit. I always prefer not to add in anything extra on that.  
  
It would also be helpful to have the 'gitk' tool when you are analyzing your commit logs, specially when you have more than once branch. You would be given a neat graphchical representation of your working directory.  
  
$ sudo apt-get install gitk   
if you already don't have one.

[![Image](http://sunilkumarn.files.wordpress.com/2011/12/screenshot-2.png?w=443)](http://sunilkumarn.files.wordpress.com/2011/12/screenshot-2.png)

This definitely would be helpful in getting the bigger picture.
