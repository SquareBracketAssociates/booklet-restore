## Storing Objects – Transactions
ReStore beginTransaction.

"Create the object"
johnSmith := Customer new firstName: 'John'; surname: 'Smith'; yourself. 

"Ask ReStore to store johnSmith in the database – nothing is written to the database at this point"
johnSmith store.

"Commit the transaction – johnSmith will now be stored in the database:"
ReStore commitTransaction

johnSmith := Customer new firstName: 'John'; surname: 'Smith'; yourself. 
johnSmith address: (Address new line1: '123 Some Street'; yourself).
johnSmith store.

ReStore commitTransaction
ReStore beginTransaction. 

"Update the persistent object – ReStore will automatically add any changes to the active transaction”
johnSmith dateOfBith: (Date fromString: '01/02/1990').
johnSmith addOrder: CustomerOrder new.

"Commit – all changes made since the start of the transaction are automatically stored"
ReStore commitTransaction
ReStore evaluateAsTransaction: 
[johnSmith 
dateOfBith: (Date fromString: '01/02/1990');
addOrder: CustomerOrder new]
ReStore beginTransaction.
johnSmith surname: 'Smythe'.
(Object confirm: 'Save Changes?')
        ifTrue: [ReStore commitTransaction "johnSmith is now named 'John Smythe' in the database"]
        ifFalse: [ReStore rollbackTransaction "johnSmith's surname in the image is reverted to 'Smith' "] 
ReStore evaluateAsTransaction: [johnSmith unstore]. 
define: #orders as: (OrderedCollection of: CustomerOrder owner: #customer);