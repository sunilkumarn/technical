---
author: sunilkumarn
comments: true
date: 2011-12-18 08:43:37+00:00
layout: post
title: Git Reset, Revert, Merge Conflicts and the Faulty Merge
wordpress_id: 550
---

<p style="text-align: left;">As a developer, I find <a title="Git" href="http://git-scm.com/" target="_blank">Git</a> both powerful and amazing. It tracks changes to source code and enables collaboration between developers. This post shows you how to easily fix wrong commits that are commonly made in development.</p>
<h1><strong><span style="color: #000000;">Remove uncommitted changes</span></strong></h1>
<p>To reset changes in all files which are staged(currently in your index) which are modified but not yet staged:</p>
<pre style="text-align: left;" class="prettyprint"><span class="pln">$ git reset </span><span class="pun">--</span><span class="pln">hard HEAD</span></pre>
<h1 style="text-align: left;"><span style="color: #000000;"><strong>Remove most recent unpublished commits</strong></span></h1>
<p style="text-align: left;">Let’s assume that your commit history is like this, with A being the latest commit in HEAD:</p>
<pre style="text-align: left;" class="prettyprint"><span style="color: #000000;"><strong><span class="pln">o </span><span class="pun">-&gt;</span><span class="pln"> o </span><span class="pun">-&gt;</span><span class="pln"> o </span><span class="pun">-&gt;</span><span class="pln"> D </span><span class="pun">-&gt;</span><span class="pln"> C </span><span class="pun">-&gt;</span><span class="pln"> B </span><span class="pun">-&gt;</span><span class="pln"> A</span></strong></span></pre>
<p style="text-align: left;">You&nbsp; have already committed your changes A, B, C and D and these changes have not been shared or published with any upstream repositories. You would like to get rid of the recent three commits.</p>
<pre style="text-align: left;" class="prettyprint"><span class="pln">$ git reset </span><span class="pun">--</span><span class="pln">hard HEAD</span><span class="pun">~</span><span class="lit">3</span></pre>
<div style="text-align: left;">Now your history would look like:</div>
<pre style="text-align: left;" class="prettyprint"><span style="color: #000000;"><strong><span class="pln">o </span><span class="pun">-&gt;</span><span class="pln"> o </span><span class="pun">-&gt;</span><span class="pln"> o </span><span class="pun">-&gt;</span><span class="pln"> D</span></strong></span></pre>
<p style="text-align: left;">The changes in commits A through C are now irretrievable. But what if you needed to remove an existing commit from somewhere in between HEAD and the beginning of history or a commit which has been shared upstream ?</p>
<h1 style="text-align: left;"><strong><span style="color: #000000;">Remove an existing published commit</span></strong></h1>
<p style="text-align: left;">Again, let’s assume your commit history looks like this:</p>
<pre style="text-align: left;" class="prettyprint"><span style="color: #000000;"><strong><span class="pln">o </span><span class="pun">-&gt;</span></strong></span><strong><span style="color: #000000;"><span class="pln"> o </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> o </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> D </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> C </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> B </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> A</span></span></strong></pre>
<p style="text-align: left;">Say you want to remove the change introduced by the commit C which has the ID <em>750ccd</em> which is also the third last commit:</p>
<pre style="text-align: left;" class="prettyprint"><span class="com"># Reference commit by ID</span><span class="pln"><br>$ git revert </span><span class="lit">750ccd</span><span class="pln"><br><br></span><span class="com"># Reference commit by position from most recent commit in HEAD</span><span class="pln"><br>$ git revert HEAD</span><span class="pun">~</span><span class="lit">2</span></pre>
<div style="text-align: left;">This introduces a new commit which reverses the changes introduced by commit C. You will be given a chance to enter a new commit message, but the default message that indicates its ‘the revert of the commit C’ is self explanatory.</div>
<div style="text-align: left;"></div>
<div style="text-align: left;">Now your history would look like,</div>
<pre style="text-align: left;" class="prettyprint"><span style="color: #000000;"><strong><span class="pln"> o </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> o </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> o </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> D </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> C </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> B </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> A </span><span class="pun">-</span><a href="http://piratepad.net/ep/search?query=%3E"><span style="color: #000000;"><span class="pun">&gt;</span></span></a><span class="pln"> rC </span><strong><strong><span class="pln"> </span><span class="com"># rC : reverted commit C </span></strong></strong></strong></span></pre>
<div style="text-align: left;">
<blockquote><p>The revert commit only removes the data.</p></blockquote>
<h1><span style="color: #000000;"><strong>Reverting A Faulty Merge</strong></span></h1>
<p>Now let me talk about the incident that had happened to my colleague while at work.</p>
</div>
<pre style="text-align: left;" class="prettyprint"><span class="pln">$ git pull origin experimental</span></pre>
<p style="text-align: left;">My friend did this while HEAD is still in master branch resulting in a faulty merge. The experimental branch has now been merged into the branch master. The merge was unintentional. There were no merge conflicts at this point. The mainline code broke. We had to revert this faulty merge.</p>
<div style="text-align: left;"><a href="http://42.mobme.in/?attachment_id=694" rel="attachment wp-att-694"><img class="alignnone size-medium wp-image-694 hoverZoomLink" src="http://42.mobme.in/wp-content/uploads/2011/12/faulty-merge-0-163x300.png" alt="" width="163" height="300"></a></div>
<div style="text-align: left;"></div>
<p style="text-align: left;">This would give you a picture. P is the point of branching. x is some&nbsp; commit made in the mainline branch totally unrelated to the side line branch. The side line branch itself has got two commits of its own, A&nbsp; and B. M is the merge commit (experimental has been merged with master).&nbsp; The code broke. Hence, we need to revert M(the merge commit).</p>
<p><a href="http://42.mobme.in/?attachment_id=695" rel="attachment wp-att-695"><img class="alignnone size-medium wp-image-695 hoverZoomLink" src="http://42.mobme.in/wp-content/uploads/2011/12/faulty-merge-1-163x300.png" alt="" width="163" height="300"></a></p>
<div style="text-align: left;">Now as seen, the merge has been reverted(W is the reverse of M). This was done with</div>
<pre style="text-align: left;" class="prettyprint"><span class="pln">$ git revert </span><span class="pun">-</span><span class="pln">m </span><span class="lit">1</span><span class="pln"> </span><span class="pun">(</span><span class="typ">Commit</span><span class="pln"> id of M</span><span class="pun">)</span></pre>
<div style="text-align: left;">This adds W to the commit log as well. Now the faulty code in the&nbsp; experimental branch was worked upon and fixed and its been made ready&nbsp; for the merge (again!). The experimental branch is now merged with the&nbsp; master branch. What was weird(for us, at that point of time) and&nbsp; noticeable was that the code changes that were made after the ‘merge&nbsp; revert’ appeared in the master branch whereas the ones made before the&nbsp;revert didn’t appear.</div>
<div style="text-align: left;"><a href="http://42.mobme.in/?attachment_id=696" rel="attachment wp-att-696"><img class="alignnone size-medium wp-image-696 hoverZoomLink" src="http://42.mobme.in/wp-content/uploads/2011/12/faulty-merge-2-163x300.png" alt="" width="163" height="300"></a></div>
<p style="text-align: left;">Again, x are the commits unrelated to the experimental branch. M2 is&nbsp; the second merge. Commits in the experimental branch,C and D, fixes the&nbsp; faulty code in A and B. Whats to be noticed is that, after the updated&nbsp; experimental branch has been merged, none of the changes made by A and B&nbsp; would appear in the master branch, whereas the changes made in C and D&nbsp; would.The reason was found out soon.</p>
<div style="text-align: left;"></div>
<div style="text-align: left;">Linus Torvalds explains the situation:</div>
<p style="text-align: justify; padding-left: 30px;"><em>Reverting a regular commit just effectively undoes what that commit did, and is fairly straightforward. But reverting a merge commit also undoes the _data_ that the commit changed, but it does absolutely nothing to the effects on _history_ that the merge had. So the merge will still exist, and it will still be seen as joining the two branches together, and future merges will see that merge as the last shared state – and the revert that reverted the merge brought in will not affect that at all.</em></p>
<p style="text-align: left;">Thats what just happened here. W(merge revert) undoes the data made by M(merge) but does nothing to the commit history brought in by M. There fore when the second merge, M2, is made, the commit history is checked and M is found to be ‘last shared state’. Hence, only those changes that were made after the ‘last shared state’, M, will be merged into the master branch now(i.e. commits C and D). None of the data created in A&nbsp; and B would merge, because as per the commit history, they are already&nbsp; merged.</p>
<p style="text-align: left;">Solution to this problem is also explained by Linus himself. The fix is to ‘<strong>revert the revert that brought in W</strong>‘, i.e, revert W before you do in the next merge,M2.</p>
<div style="text-align: left;">Thus the main line commit log would be</div>
<pre style="text-align: left;" class="prettyprint"><strong><span style="color: #000000;"><span class="pln">P </span><span class="pun">-&gt;</span><span class="pln"> x </span><span class="pun">-&gt;</span><span class="pln"> M </span><span class="pun">-&gt;</span><span class="pln"> W </span><span class="pun">-&gt;</span><span class="pln"> x </span><span class="pun">-&gt;</span><span class="pln"> x </span><span class="pun">-&gt;</span><span class="pln"> Y </span><span class="pun">-&gt;</span><span class="pln"> M</span><span class="str">' </span></span></strong></pre>
<div style="text-align: left;">where Y is the reverse of W and M2 is the merge made after that.</div>
<div style="text-align: left;"></div>
<pre style="text-align: left;" class="prettyprint"><span class="pln">$ git revert </span><span class="pun">(</span><span class="pln">commit id of W</span><span class="pun">)</span></pre>
<div style="text-align: left;">adds Y to the commit log. The above commit log would be equivalent to</div>
<pre style="text-align: left;" class="prettyprint"><strong><span style="color: #000000;"><span class="pln">P </span><span class="pun">-&gt;</span><span class="pln"> x </span><span class="pun">-&gt;</span><span class="pln"> M </span><span class="pun">-&gt;</span><span class="pln"> x </span><span class="pun">-&gt;</span><span class="pln"> x </span><span class="pun">-&gt;</span><span class="pln"> M</span><span class="str">'</span></span></strong></pre>
<div style="text-align: left;">where there is no W nor a Y and then the second merge has been&nbsp; performed, M2. Now this would be fine, and all the changes made in the&nbsp; experimental branch should be seen in the master branch(ignoring merge&nbsp; conflicts). If there are any merge conflicts arising, git leaves the&nbsp; index and the working tree in a special state that gives us all the&nbsp; information needed to resolve the merge.</div>
<p><span style="color: #000000;"><strong>Resolving a Merge Conflict</strong></span></p>
<p>A <strong>Merge conflict</strong> would throw in the following message:</p>
<p><strong><code>CONFLICT (content): Merge conflict in sample_script.rb Automatic merge failed; fix conflicts and then commit the result</code></strong></p>
<div style="text-align: left;">Trying to switch to the experimental branch would give you this</div>
<p><strong><code>error: you need to resolve your current index first</code></strong></p>
<div style="text-align: left;">The files with conflicts will have markers upon them.</div>
<p style="text-align: left;"><strong><code>&lt;&lt;&lt;&lt;&lt;&lt;&lt; HEAD:sample_script.rb<br>
"We would be&nbsp; starting off now"<br>
=======<br>
"This would be the end"<br>
&gt;&gt;&gt;&gt;&gt;&gt;&gt;&nbsp; d31f96832d54c2702914d4f605c1d641511fef13:sample_script.rb</code></strong></p>
<div style="text-align: left;">Now we need to resolve these conflicts manually followed by adding the file and commit it.</div>
<pre style="text-align: left;" class="prettyprint"><span class="pln">$ git add sample_script</span><span class="pun">.</span><span class="pln">rb<br>$ git commit </span><span class="pun">-</span><span class="pln">a</span></pre>
<div style="text-align: left;">The commit message would already be filled in indicating that its a&nbsp; conflict resolving commit. I always prefer not to add in anything extra&nbsp; on that.</div>
<div style="text-align: left;"></div>
<div style="text-align: left;"><strong>gitk</strong></div>
<div style="text-align: left;"></div>
<p style="text-align: left;"><strong></strong>It would also be helpful to have the ‘gitk’ tool when you are&nbsp; analyzing your commit logs, specially when you have more than once&nbsp; branch. You would be given a neat graphical representation of your&nbsp; working directory.</p>
<pre style="text-align: left;" class="prettyprint"><span class="pln">$ sudo apt</span><span class="pun">-</span><span class="pln">get install gitk</span></pre>
<div style="text-align: left;">if you already don’t have one.</div>
<div style="text-align: left;"></div>
<div style="text-align: left;"><a href="http://42.mobme.in/?attachment_id=519" rel="attachment wp-att-519"><img class="alignnone size-full wp-image-519" src="http://42.mobme.in/wp-content/uploads/2011/12/gitk.png" alt="" width="442" height="324"></a></div>
<div style="text-align: left;"></div>
<div style="text-align: left;">This definitely would help in getting a better picture.</div>
<div style="text-align: left;"></div>

