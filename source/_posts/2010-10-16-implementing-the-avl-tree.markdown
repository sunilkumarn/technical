---
author: sunilkumarn
comments: true
date: 2010-10-16 04:21:58+00:00
layout: post
slug: implementing-the-avl-tree
title: Implementing the Avl Tree
wordpress_id: 302
categories:
- C
---

AVL Tree is a **self-balancing binary search tree** and it was the first such structure to be invented. The  tree is said to be balanced because the difference in the heights of the child subtrees differ by atmost one. The operations Insertion, Deletion and Lookup on an Avl tree is of the order of  _Log n_ because of this balancing act. Here we take a look into the implementation of an AVL tree .

First we need to give a thought on the term 'balance Factor' and on the major operations that are involved in the making of such a tree.

**Balance Factor:**Balance factor is associated with each node of the Avl tree. Its the difference between the heights of the left subtree and right subtree of that particular node.

_Balance factor = height of left 	subtree – height of right subtree._

A balance Factor of 0 , +1 or -1 for each and every node of the tree indicates that its balanced. Whenever the balance Factor of any of the nodes equals – or + 2 , the tree becomes unbalanced and needs to undergo balancing.

Now lets just peep into the major operations that are involved in the build up of the tree.

There are 3 of them


1) Insertion:2) Deletion:3) Rotation:
Insertion is the usual process. There is nothing special in inserting a node into an Avl tree than in inserting it to a normal binary search tree.

Deletion is again the same with a minor difference which we will look in to as we continue.

Rotation is the balancing act- The operation that distinguishes an Avl tree from the normal binary search tree. Rotation needs to be performed whenever the tree goes out of balance – this could happen as a result of the first two operations ( Insertion and Deletion ).  The Balance factor may go beyond 1 and hence the tree will have to be balanced so that it remains as an Avl tree. Each of the three operations , specially that of rotation will have to be given due care in building up an Avl tree.

You could sum it up this way :

**Each insertion in an Avl tree is a combination of normal insertion into a binary search tree added  with finding out whether the tree is unbalanced or not followed by rotation if necessary.**

**Each deletion in an Avl tree is a combination of normal deletion into a binary search tree added with  finding out whether the tree is unbalanced or not followed by rotation if necessary.**

Before we get into more details of each of them , we will have a look into the way each node is implemented in the Avl tree.

Each and every node in the Avl tree is implemented as a structure .This is same as what is done in an ordinary BST , **since Avl tree is itself a BST.**

`struct node{`

`int data;
`
`struct node *parent,*left,*right;`

`}*p,*root=0;`

The field 'data' contains the actual data that is to be stored in the node .There are three pointers of the type 'struct node*'.

_*left –>    points to the left child of the node_

_*right –>   points to the right child._

_*parent –> points to the parent node ._

[![](http://sunilkumarn.files.wordpress.com/2010/10/node.png?w=231)](http://sunilkumarn.files.wordpress.com/2010/10/node.png)

Now lets get into the details of the different operations performed on the Avl tree.

Inserting into an Avl Tree can pictorially be represented as follows:

[![](http://sunilkumarn.files.wordpress.com/2010/10/untitled-1.png?w=300)](http://sunilkumarn.files.wordpress.com/2010/10/untitled-1.png)

Obviously , there are other functions that are involved  in the process, but the ones shown in figure are the significant ones.

**avl_insert :** As discussed earlier insertion into an avl tree consists of the normal insertion followed by the checks for balancing and rotation if necessary. Normal insertion is performed first . Then the function self_bal() is called :

**self_bal() **:   The newly inserted node would obviously be balanced since it wont have any children. Therefore the balance factors of the nodes that come above the inserted node in the tree ( starting from the parent ,right upto the root ) is to be checked. The function 'self_bal' invoked 'balance()' which performs the further  tasks. Again, since a newly created tree is always balanced and since parent of the root points to NULL , the function 'balance()' should not be called if the newly inserted node is the root itself.

**balance() **:   Invokes the function bal_fac() and checks if the balance factor is equal to +or- 2 . If yes , calls the appropriate rotate function.

**bal_fac()   :** Finds out the balance factor for each node.

**rotate()     :** Here rotate() is the just the cohesive name for the two types of rotate functions . According to the balance factor , the appropriate 'rotate' functions are called. We will discuss them in detail later.

The pictorial representation of 'delete()' would be no different.

[![](http://sunilkumarn.files.wordpress.com/2010/10/delete.png?w=300)](http://sunilkumarn.files.wordpress.com/2010/10/delete.png)

By now you would have understood that the central aspect in building up the whole Avl tree is the rotate() function, but before we get into implementing the 'rotate()' function , its of prime importance that we give a little more thought into implementing the 'delete()' function in the Avl tree. As mentioned above , there is just a minor change that needs to be made , due to the self balancing nature of the Avl tree.

While performing a 'delete()' function in a normal BST, we need to look into 5 different cases of how a particular node could be deleted and replaced with another one. Just peeping into them:

1)The victim node being a leaf  	node.

2)The victim node having a right 	child.


2.1) The victim node having a right 	child with a left subtree.




2.2) The victim node having a right 	child with no left subtree.


3)The victim node not having a 	right child.


3.1) The victim node having a left 	child with a right subtree.




3.2) The victim node having a 	left-child with no right subtree.


Certainly , from what we have discussed until now, its clear that one of these conditions never happens. You might have guessed it by now.

Case 3.1) would never happen in an Avl tree. This could be explained with the following figure.


[![](http://sunilkumarn.files.wordpress.com/2010/10/case3-11.png?w=252)](http://sunilkumarn.files.wordpress.com/2010/10/case3-11.png)


Consider you want to delete the node '10' after adding '9' to the tree , but adding '9' to the tree would leave it unbalanced.

[![](http://sunilkumarn.files.wordpress.com/2010/10/case3.png?w=250)](http://sunilkumarn.files.wordpress.com/2010/10/case3.png)

Hence the tree will be rotated so that it becomes balanced ( how?, we will come to know shortly) before you could delete '10' . Hence there will never be a case of a victim node have a 'left child with a right subtree' and does not have a right child of its own.

Now , its time that we move into the  in the creation of an Avl Tree, the 'rotate() ' function.

**Rotation** ,as mentioned ,**follows up a deletion from or an insertion into an Avl tree** in such a way that it destroys the balanced nature of the Tree.

There are 4 kinds of rotation which are implemented using 2 different rotation functions.

Its crucial at this stage that the following points are understood.

_Balance factor of a node = +2 implies that the node ( and ultimately the tree) is unbalanced because of  the left subtree of the node and there will be one right rotation for sure._

_Balance factor of a node = -2 implies that the node ( and ultimately the tree) is unbalanced because of the right subtree of the node and there will be one left rotation for sure._

_Whether a single rotation needs to be done or whether the tree needs to be rotated twice depends upon the balance factor of the child of the root of the subtree that is unbalanced. You needn't scratch your head on this , stuffs that follow would lighten up things._

Let 'point' be the root of the subtree which has made the tree go out of balance . The 4 kinds of rotation are:

1)**left  – left rotation:** If balance factor(BF) of point is 2 and the BF of the left child is > 0 , then a single 'right' rotation would do it for you.

2)**left  –  right rotation:** If BF of point is 2 and the BF of the left child is < 0 , then a    'left' rotation followed by a 'right' rotation would do it for you.

3**)right – left rotation:** If BF of point is -2 and the BF of the left child is > 0 , then a 'right' rotation followed by a 'left' rotation makes the tree balanced.

4)**right – right rotation:** If BF of point is -2 and the BF of the left child is < 0  , then a single 'left' rotation would be enough.

We will discuss rotation which happens as a result of the tree being unbalanced after an insertion or a deletion has been performed. The 'rotation' function is independent of whether the previous function was a deletion or an insertion , it depends only on the current structure of the tree.


_**Left-Right Rotation  &   Left-Left Rotation**_


Consider the figure :


[![](http://sunilkumarn.files.wordpress.com/2010/10/lr1.png?w=231)](http://sunilkumarn.files.wordpress.com/2010/10/lr1.png)


_Here the point of imbalance is '23' and the reason for imbalance is the left subtree of '23'. _Hence performing a 'right' rotation is essential . Now the left child of '23' has a BF of -1 ( < 0 ) . Hence a left rotation also needs to be performed. This leads to a left-right rotation .

left rotation : Left rotation is performed by invoking the rotate_left() function. The two parameters that are to passed to the function are 'root' and 'pivot' .

Root ---> point of imbalance , I.e 	'17'

Pivot ---> right-child of the point of imbalance I.e '19'.


[![](http://sunilkumarn.files.wordpress.com/2010/10/lr2.png?w=231)](http://sunilkumarn.files.wordpress.com/2010/10/lr2.png)







[![](http://sunilkumarn.files.wordpress.com/2010/10/lr3.png?w=278)](http://sunilkumarn.files.wordpress.com/2010/10/lr3.png)


The left rotation has been performed.

Now our problem has been reduced to performing a left-left rotation , you could see that from the figure .That is ,we need to perform a single 'right rotation':

right rotation : The right rotation is performed by invoking the rotate_right() function. The two parameters would be :

Root ---> point of imbalance : I.e 23

Pivot ---> left-child of the point of imbalance I.e '19'.


[![](http://sunilkumarn.files.wordpress.com/2010/10/lr4.png?w=278)](http://sunilkumarn.files.wordpress.com/2010/10/lr4.png)










[![](http://sunilkumarn.files.wordpress.com/2010/10/l11.png?w=258)](http://sunilkumarn.files.wordpress.com/2010/10/l11.png)





The right rotation also has been performed. Now you may have a look at the tree and would find out that its balanced .


_**Right-Left Rotation &  Right-Right Rotation.**_




[![](http://sunilkumarn.files.wordpress.com/2010/10/lf11.png?w=283)](http://sunilkumarn.files.wordpress.com/2010/10/lf11.png)


Consider the figure above :

_Here the point of imbalance is '17' and the reason for imbalance is the right subtree of '17'. _Hence performing a 'left' rotation is essential . Now the right child of '17' has a BF of 1( > 0 ) . Hence a 'right' rotation also needs to be performed. This leads to a right-left rotation .

right rotation : right rotation, as mentioned above is performed by invoking the rotate_right() function. The two parameters that are to passed to the function are 'root' and 'pivot' .

Root ---> point of imbalance , I.e 	'23'

Pivot ---> left-child of the point of imbalance I.e '19'.


[![](http://sunilkumarn.files.wordpress.com/2010/10/lf21.png?w=283)](http://sunilkumarn.files.wordpress.com/2010/10/lf21.png)




[![](http://sunilkumarn.files.wordpress.com/2010/10/lr31.png?w=300)](http://sunilkumarn.files.wordpress.com/2010/10/lr31.png)


The right rotation has been performed.

Now our problem has been reduced to performing a right-right rotation .That is ,we need to perform a single 'left rotation':

left rotation : We have seen that left rotation is performed by invoking the rotate_left() function. The two parameters would be :

Root ---> point of imbalance : I.e '17'

Pivot ---> right-child of the point of imbalance I.e '23'.


[![](http://sunilkumarn.files.wordpress.com/2010/10/lr41.png?w=300)](http://sunilkumarn.files.wordpress.com/2010/10/lr41.png)




[![](http://sunilkumarn.files.wordpress.com/2010/10/l12.png?w=258)](http://sunilkumarn.files.wordpress.com/2010/10/l12.png)


The left rotation also has been performed. The tree has been balanced .We have found out how a tree could be balanced through rotation.

There is another issue that we need to deal with while performing rotation. Two questions that we need to consider here are...

**1)What happens to the left node of the pivot during a left rotation.?**

**2)What happens to the right node of the pivot during the right rotation.?**

Consider the following figure .


[![](http://sunilkumarn.files.wordpress.com/2010/10/nw.png?w=195)](http://sunilkumarn.files.wordpress.com/2010/10/nw.png)


The tree as you could see is unbalanced with the root ('23') being the point of imbalance, the left subtree being the reason for the imbalance.. Since the BF of the left child of the root = -1 ( < 0 ) , we need to perform a left – right rotation . I.e a left rotation followed by a right rotation . For performing the left rotation , the root is '17'  and the pivot is '19'. As you could see from the figure ,the left child of the pivot is marked 'L' . Our question is what exactly happens to this 'L' after the left rotation has been performed. ? The answer is right there in our next figure .


[![](http://sunilkumarn.files.wordpress.com/2010/10/nw2.png?w=255)](http://sunilkumarn.files.wordpress.com/2010/10/nw2.png)


L has become the right child of '17 ' . To be more precise **,after a left rotation, 'L'( the left child of the pivot) has become the 'right child' of the node which was the parent of the 'pivot' before the rotation was performed.**

As you could see, the tree still remains unbalanced as we have not performed the 2nd phase of our left-right rotation . We perform the right rotation and make the tree balanced .


[![](http://sunilkumarn.files.wordpress.com/2010/10/nw3.png?w=255)](http://sunilkumarn.files.wordpress.com/2010/10/nw3.png)


I think , by now you would be able to guess out the solution for the second question that we had asked .

What happens to the right node of the pivot during the right rotation.?

The answer is certain , this is exactly the complement of what happens after a left rotation that we have just seen . **After performing a right rotation , the right child of the pivot becomes the left child of the node that was its parent before the rotation was performed**.

Here , I have tried and dealt with almost all issues that come up in the implementation of an Avl tree. You could download the complete source code[ here](http://github.com/sunilkumarn/Avl_Tree)
