## Advanced Querying
allJohnSmiths := allSmiths satisfying: [ :each | each firstName = 'John'].
specificJohnSmith := allJohnSmiths detect: [ :each | each dateOfBirth = aDate]
	PrinterScanner	- scanSize, scanResolution
allSmithsOrJones addAll: (Customer storedInstances satisfying: [ :each | each surname = 'Jones']).

(Customer storedInstances satisfying: [ :each | each surname = 'Smith']) readStream
ukCustomers := allCustomers satisfying: [ :each | each address country = 'United Kingdom'].
ukCustomers collect: [ :each | each fullName || each dateOfBirth year]

allOrders := CustomerOrder storedInstances.
recentOrders := allOrders satisfying: [ :each | each orderDate year = Date today year].
recentOrders collect: [ :each | each customer fullName || each totalPrice]
recentOrders project: [ :each | each customer customerNo || each count].

"Total recent order value by customer"
recentOrders project: [ :each | each customer customerNo || each totalPrice sum].
[ :each | 
each customer customerNo || each count || each totalPrice sum || 
each totalPrice average || each totalPrice minimum || each totalPrice maximum]
Customer storedInstances select: [ :each | each fullName trimRight = ‘John Smith’]
	translateMessage: #plusPercent: 
	toFunction: '%1 * (1 + (%2 / 100))' asSQLFunction 
		translateMessage: #asPercentageString 
		toFunction: ('CAST((%1*100) as char) || "%%"' asSQLFunctionWithResultClass: String)
	-123 ifPositive: 'POS' ifNegative: 'NEG'
		translateMessage: #ifPositive:ifNegative: 
		toFunction: ('CASE WHEN %1>=0 THEN %2 ELSE %3 END' asSQLFunction resultParamIndex: 2)
template surname: 'Sm*' asWildcard.
matchingCustomers := template similarInstances.
matchingCustomers include: #customerOrders.
matchingCustomers := matchingCustomers asSortedCollection.
template surname: 'Sm*' asWildcard.
matchingCustomers := template similarInstances asSortedCollection.