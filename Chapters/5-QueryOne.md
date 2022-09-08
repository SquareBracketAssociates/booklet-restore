## Query Introduction
allCustomers := Customer storedInstances. 

"Retrieve all Customers with the surname Smith"
allCustomers select: [ :each | each surname = 'Smith'].
 
"Find Customer number 12345"
allCustomers detect: [ :each | each customerNo = 12345].
 
"Do we have any Customers in London?"
allCustomers anySatisfy: [ :each | each address city = 'London'].

"Who has placed an order today?"
allCustomers select: [ :each | each customerOrders anySatisfy: [ :order | order date = Date today]]. 
allCustomers detect: [ :each | each customerNo = 12345].
allCustomers anySatisfy: [ :each | each address city = 'London'].
allCustomers select: [ :each | each customerOrders size > 10].
allCustomers select: [ :each | each customerOrders anySatisfy: [ :order | order date = Date today]]. 

	^self firstName, ' ', self surname
allCustomers anySatisfy: [ :each | each surname = 'Smith' and: [each address city = 'London']] 
	(p1 surname = p2 surname)
		ifTrue: [p1 firstName <= p2 firstName]
		ifFalse: [p1 surname < p2 surname]]
	(p1 surname < p2 surname) | 
		((p1 surname = p2 surname) & (p1 firstName <= p2 firstName))]
template surname: 'Smith'.
allSmiths := template similarInstances.

template address city: 'London'.
allSmithsInLondon := template similarInstances.
template surname: 'Smith*' asWildcard.
allSmithsEtc := template similarInstances.
template despatchDate: nil required.
allUnsentOrders := template similarInstances.