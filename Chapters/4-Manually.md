## Storing Objects Manually
ReStore evaluateAsTransaction: 
[(Customer new firstName: 'John'; surname: 'Smith'; yourself) store]. 

"Storing a new object - without transaction:"
(Customer new firstName: 'John'; surname: 'Smith'; yourself) store.

"Updating an existing object - with transaction:"
ReStore evaluateAsTransaction: [johnSmith middleName: 'David'.]. 

"Updating an existing object - without transaction:"
johnSmith middleName: 'David'; store.
ReStore evaluateAsTransaction: [johnSmith unstore]. 

"Deleting an object - without transaction:"
johnSmith unstore.

johnSmith middleName: 'David'.
johnSmith hasChanged. "true"

johnSmith store.
johnSmith hasChanged. "false"
define: #customerOrders as: (OrderedCollection of: CustomerOrder dependent owner: #customer);