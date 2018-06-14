# Using Bayes Classifier for Learning Action-Relevance 

20th June 2017

### Naive Bayes Classifier

For classifying data using Naive Bayes, we take an assumption that attributes (value attained by the training variable) are independent of each other given the target value. 

Mathematically, suppose our training variables attain values a1, a2, a3, .., an and the target function attains value vj, then
        
        P(a1, a2, .., an | vj) = P(a1 | vj) * P(a2 | vj) * .. * P(an | vj)
 
Now to predict the probability of vj given attribute values a1, a2, .. an, we write

        P(vj | a1, a2, .., an) = P(a1, a2, .., an | vj) * P(vj)[Normalization Factor Ignored]
 
And since we assume attributes are independent given target value, therefore,

        P(vj | a1, a2, .., an) = P(a1 | vj) * P(a2 | vj) * .. * P(an | vj) * P(vj)

Some subtle and crucial assumptions and requirements that must be kept in mind while implementing Naive Bayes Classifier are:
 
1. All the attributes should be independent given target. 

So while designing, we should be careful that attributes are not highly coupled.

2. Target Function should take a value from a finite domain.

Therefore it becomes important to have a finite set of output values.

3. Number of tuples Γ given for training should be high when compared to the following product,

        |a1| * |a2| * .. * |an|

for the classifier to classify successfully.

This suggest two either we build a huge training set or we build variables smartly and remove every possible redundant assignment for them. We will do the latter.

#### Training Instances

Let the training instance be represented by the tuple Γ, where:
 
Γ = <initModel.xml, domain.aggl, target.aggt, result.plan>
 
The goal is, given a bunch of such tuples 

    (Γ1, Γ2, .., ΓN)

to be able to estimate the probability of using an action in domain.aggl for a new tuple ΓM.
 
where,

        Γ1 = (initModel1.xml, domain.aggl, target1.aggt, result1.plan)
        Γ2 = (initModel2.xml, domain.aggl, target2.aggt, result2.plan)
        …
        ...
        ...
        ΓN = (initModelN.xml, domain.aggl, targetN.aggt, resultN.plan)
        and,
        ΓM = (initModelM.xml, domain.aggl, targetM.aggt, resultM.plan)

The complexity of the instance lies within the structure of the files. In the following section, I'll explain how we're planning to handle it.

#### Design of Variables
 
As you might have noticed, the *.domain* file is common in between all the training tuples (Γ), we will not be using it for training purpose. But, it will be considered for extracting information about the domain (like the set of all valid action **A** etc.).
 
Since we are interested in plans, the target values must lie in the *.plan* file. Therefore, **A**, the set of all valid actions, is nothing but the set of target values.
 
Now one might think that *.plan* files consist of multiple actions, so shouldn’t the power set of **A** be considered as target value? Well, yes, that is a more accurate way but as stated above, **A** can be very large. Hence training for such huge set of target values might not be effective. 
 
To circumvent this situation and make **A** as the set of our target values, whenever we get m actions in the .plan files, we feed them one by one to our classifier and train the same variables m times with our target value as one of the m actions (each considered one by one).
 
Also, our set of target values, **A**, will satisfy condition 2 stated above, since the set of all actions is always finite.
 
Now comes the most important of all, designing the variables and what should be the set of values (attributes) taken by them, such that condition 1 and 3 are satisfied. 
 
Before diving into the problem right away, let’s discuss the structure of two files: *initModel.xml* and *target.aggt* which we’ll be considering during the design of these variables.
 
 
*InitModel.xml*
 
This file has two tags that we care about:
<symbol id=”number” type=”name”>         : [represents a node]
<link src=”id1” dst=”id2” label=”name”>    : [represents a directed edge between nodes]
 
Two different initial models can have same id’s assigned to completely different object and vice-versa. To avoid that and maintain homogeneity, we will replace all id’s by their types.

*Target.aggt*
 
Note : We’ll ignore precondition part for training. 
 
{
    id:type(x,y)
    id1->id2(relation)
}
 
 
Having discussed the types of files we’ll be dealing with, let’s start the design of our variables.
 
There’ll be two kinds of variables:
 
1. *Node/Position type*

In this we check target file and consider all:

        id:type(x, y)

and convert it to *typeI:type* (value of variable for Bayes Classifier). Since types can be modified during the planning, *typeI* basically represents initial type of *id* (from file *initModel.xml*).
 
We also consider instances where id is replaced by some variable:

        var:type(x, y)

And convert it to *type:type*, since *var* basically requires any node of type *type* to full-fill the condition. We also record type of *var* to be *type* for it’s use in next category of variable. 

This basically encapsulates the change of type (if it happens).
 
2. *Edge/Relation type*
 
In this we check target file and consider all:

        id1->id2(relation)

and also consider all the relations that were present in the *initModel.xml* under *<link>* tag, that had *src=id1* and *dst=id2*. It’ll be represented as following:
 
Let’s say there are *N* initial relations (from *initModel.xml*) between *id1* and *id2* : *relation1, relation2, .., relationN*. Then then we’ll have following types of assignment for variables:
 
 
* Unary Assignments
    
        type(id1), relation1, type(id2)
        type(id1), relation2, type(id2)
                :
                :
        type(id1), relationN, type(id2)
        type(id1), targetRelation, type(id2)

Unary Assignments represent final relation between the two id's.
     
* Binary Assignments
    
        type(id1), relation1, targetRelation, type(id2)
        type(id1), relation2, targetRelation, type(id2)
                :
                :
        type(id1), relationN, targetRelation, type(id2)

Binary Assignment link the **transition** of relations between two id's from *initModel.xml* to *target.aggt*.
    
In case *var* (general variable) is present instead of one of the id’s (say *id1*) we’ll consider all *<link>* tuples and check if following two conditions are true : 
* type(src) matches with type(var)
* id2 = dst
 
If yes, we’ll consider all relations between them and include all tuples as described above. 
 
For the case *var* is present instead of *id2*  we’ll consider all *<link>* tuples and check if following two conditions are true : 
* id1 = src
* type(dst) matches with type(var)
 
If yes, we’ll consider all relations between them and include all tuples as described above. 
 
If both *src* and *dst* are replaced by *var1* and *var2* respectively and check all *<link>* tuples for following condition:
* type(src) matches with type(var1)
* type(dst) matches with type(var2)
 
If yes, we’ll consider all relations between them and include all tuples as described above.
 
We can also include nary (>2) cluster of relations but keeping point 3 in mind, we’ll limit them to unary and binary only.

Following is an example which demonstrates how are the variable selected. The example is minimalistic for understanding purpose.

Example:

Let initModel.xml be:
```
<AGMModel>
    <symbol id="1" type="object"> </symbol>
    <symbol id="2" type="objectSt"> </symbol>
    <symbol id="3" type="object"> </symbol>
    <symbol id="4" type="objectSt"> </symbol>
    <link src="1" dst="2" label="reachable"> </link>
    <link src="3" dst="4" label="reachable"> </link>
    <link src="1" dst="4" label="noReach"> </link>
    <link src="3" dst="2" label="noReach"> </link>
</AGMModel>
```
and target.aggt be:

```
{
    1:object(1,1)
    VS:objectSt(1,1)
    1->VS(table)
}
precondition
{
}
```

Following are the variables (taking true values) for the example.

1. **Node Type**
(object:object) [From id 1]
(objectSt:objectSt) [From both id 2 and 4 because of general variable]
2. **Relation Type**
i. **Unary**
(object, table, objectSt) [From both id 2 and 4 because of general variable]
ii. **Binary**
(object, table, reachable, objectSt) [From id 2 as a result of general variable]
(object, table, noReach, objectSt) [From id 4 as a result of general variable]

Note that Binary Assignments is useful for learning as they not only tell about the final relations but the *transition* of relations between two objects of some type. As we're interested in planning, capturing *transition* using these type of variables for crucial for learning. 
 
During the testing phase, to remove bias by a new relation or a new id encountered, we apply laplacian smoothing and treat it as a variable which hasn’t been encountered with any of our target variables.


* * *

Lashit Jain
