## Storing Objects – TransactionsYou have successfully completed your class specifications, connected ReStore, and constructed your tables. You are now ready to create and store persistent objects.One of the simplest ways to do this is to allow ReStore to keep track of your persistent objects by wrapping all changes in a **Transaction**. Transactions are a way of packaging together a batch of changes to persistent objects. During the transaction there is no actual interaction with the database, all changes are purely within your image. At the end of the transaction, you may **commit** any changes to the database, at which point they become persistent. Alternatively, changes may be **rolled back**, leaving the database unchanged and the objects in memory in their original state, prior to the start of the transaction. An important trait of transactions is that they are **atomic** – if the transaction is successfully committed, all changes made within the transaction will be persisted to the database. However if the commit fails, no changes are written to the database . This prevents related changes being only partly committed, which could leave the database in an inconsistent state.### Storing a New ObjectWhen you first create an object in your image it is non-persistent, i.e. it does not exist in the database. You need to explicitly ask ReStore to persist the new object – this is done via the message store```"Store a new object in the database – first begin a new transaction"
ReStore beginTransaction.

"Create the object"
johnSmith := Customer new firstName: 'John'; surname: 'Smith'; yourself. 

"Ask ReStore to store johnSmith in the database – nothing is written to the database at this point"
johnSmith store.

"Commit the transaction – johnSmith will now be stored in the database:"
ReStore commitTransaction```#### Automatic Storing of Referenced Objects - 1An important point to note is that, where a stored object references other non-persistent objects, these will automatically be stored along with the referring object. For example, if we modify the above example to also create an `Address` object for the new customer:```ReStore beginTransaction.

johnSmith := Customer new firstName: 'John'; surname: 'Smith'; yourself. 
johnSmith address: (Address new line1: '123 Some Street'; yourself).
johnSmith store.

ReStore commitTransaction```Since the new `Address` object is referenced from the stored `Customer` object, the `Address` object will automatically also be persisted -- there is no need to explicitly store this object. This applies whenever a stored object references other non-persistent objects. ### Updating a Persistent ObjectOnce you have a persistent object you may want to update its database representation with changes made in your image. Provided you make these changes within a transaction then there is nothing additional you need to do other than commit the transaction at the end – ReStore will automatically detect all changes made to persistent objects: ```"First being a new transaction"
ReStore beginTransaction. 

"Update the persistent object – ReStore will automatically add any changes to the active transaction”
johnSmith dateOfBith: (Date fromString: '01/02/1990').
johnSmith addOrder: CustomerOrder new.

"Commit – all changes made since the start of the transaction are automatically stored"
ReStore commitTransaction```#### Automatic Storing of Referenced Objects - 2Something to note in the above example is the addition of a new CustomerOrder:```johnSmith addOrder: CustomerOrder new.```Similar to the Address object in the previous section, the new `CustomerOrder` object will automatically be stored in the database when the transaction commits, since it is referenced from the already-persistent johnSmith object. Again, there is no need to explicitly store this object. #### Block TransactionsAs a more convenient alternative to a beginTransaction… commitTransaction sequence, it is possible to use the message evaluateAsTransaction: and make all your changes within a block:```"Equivalent to the above using a block"
ReStore evaluateAsTransaction: 
[johnSmith 
dateOfBith: (Date fromString: '01/02/1990');
addOrder: CustomerOrder new]```### Reverting ChangesReStore recognizes changes to a persistent object by storing a copy of the object prior to those changes being made. Providing those changes are made within a transaction then the changes can be automatically reverted by **rolling back** the transaction.This can be useful within application code by providing an easy way to implement “Undo” functionality:```"Selectively commit or rollback changes to a persistent object"
ReStore beginTransaction.
johnSmith surname: 'Smythe'.
(Object confirm: 'Save Changes?')
        ifTrue: [ReStore commitTransaction "johnSmith is now named 'John Smythe' in the database"]
        ifFalse: [ReStore rollbackTransaction "johnSmith's surname in the image is reverted to 'Smith' "] ```In a similar way it is also possible to easily implement a dialog with OK/Cancel functionality without any additional code – simply begin a transaction when opening the dialog, then either commit or rollback depending on whether the user clicks OK or Cancel. ### Deleting Persistent ObjectsFinally, you may also wish to delete persistent objects from the database. This is done with the message unstore:```"Delete an object from the database"
ReStore evaluateAsTransaction: [johnSmith unstore]. ```#### Automatic Deletion of Dependent ObjectsSimilar to the automatic storing of referenced objects discussed earlier, referenced objects may also automatically be unstored when their referring object is unstored. However this is subject to whether the `reStoreDefinition` of the referring object defines the relationship as dependent \(see section 2.7\).Assuming the address and orders definitions of `Customer` are dependent:```define: #address as: Address dependent;
define: #orders as: (OrderedCollection of: CustomerOrder owner: #customer);```… then the Address object and all `CustomerOrder` objects referred to by johnSmith will also be deleted when johnSmith is deleted. 