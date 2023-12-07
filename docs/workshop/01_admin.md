
## One-Time (Administrator) Preparation

After your Infrastructure has been installed/configured, but before you can actually start using Merlin:

1. IBM i Integration in Merlin
2. Create Application definition - source and object libraries
3. (If needed) migrate source from physical files -> IFS -> git repo

### IBM i integration in Merlin

<!-- panels:start -->

<!-- div:left-panel -->

6 actions on IBM i performed sequentially from Merlin by the Administrator: 

1. Enable ansible
2. Validate PTF level
3. Install certificates
4. Enable IBM i Developer
5. Enable remote debugger
6. Enable Arcad (install Arcad solutions on the target IBM i)

<!-- div:right-panel -->

![](01/Picture1.png)

<!-- panels:end -->

---

### Creation of an Arcad Application

<!-- panels:start -->

<!-- div:left-panel -->

All these prep steps can be done from Merlin GUI or green screen.

*Green screen steps coming soon.*

<!-- div:right-panel -->

![](01/from_merlin.png)

<!-- panels:end -->