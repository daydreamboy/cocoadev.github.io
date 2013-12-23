---
layout: page
title: TableViewWithStaticItems
---

Ok, here is what I have. I have a list of items in an General/NSTableView, specifically categories that each contain a list of items in another tableview. What I want to accomplish is to compile these items into one list of the items, and turn the categories into iTunes-style smart lists and/or regular lists. I'm using General/CoreData and Bindings, so is there any easy way to do this? I looked into Fetched properties, but I'm not sure if that is the way to go. I also want to have an "Entire Library" item at the top of the list, which I suspect would be a list with no predicate. Any ideas? Thanks. --General/LoganCollins

*This is no different than a "master/detail" design. Your smart list object has a to-many relationship to the contained objects and the contained objects have a to-many relationship to container objects (if you want one contained object to belong to more than one list/container). The first table pulls from the smart list objects' "name" property while the second table's array controller gets its content from the first array controller's 'selection' key. The second table's column (that lists the names of the contained objects) pulls from the arrangedObjects.name ... since the second table's array controller gets its arranged objects from the first array controller's selected container object. Simple, straight-forward, and can be set up in less than five minutes by following the examples in the Core Data Programming Guide. But 'static list' or no, you still have to create the objects.*

I've done that already, but I want to add actual smart groups like in iTunes, where the end user will define the predicates to limit the criteria of objects. Those groups will be the items in the first table. In essence, my interface is set up like iTunes. The Source list is my categories list, and the songs are the items in the second tableview. I know what I'm describing is a little different now than the title, but I've shifted my idea since writing this originally.

*Then in that case, your smart list entity should have an attribute for storing the predicate(s). The exact mechanism of this will be determined by how you allow your users to define the rules. Directly storing the predicates might make it difficult to re-create the 'rule editor' if the user decides to edit the smart list later, so you might want to create a separate 'rule' entity and make your smart list entity have a to-many relationship to 'rules', which define the various aspects needed to recreate the predicate(s). You then create a transient attribute of your smart list like "filterPredicate" which constructs the predicate or compound predicate based on the -rules. Your second table's array controller's filterPredicate would be bound to your first table's array controller's selection's -filterPredicate.*