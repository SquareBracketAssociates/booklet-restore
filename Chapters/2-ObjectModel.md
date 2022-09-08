## Defining the Object Model

	^super reStoreDefinition
		define: #orderDate as: Date; 	"Class"
		define: #customer as: Customer;	"Class"
		define: #items as: (OrderedCollection of: CustomerOrderItem);	"Collection"
		define: #totalPrice as: (ScaledDecimal withPrecision: 8 scale: 2); 	"Parameterized Class"
		yourself
	define: #dateOfBirth as: Date;
	define: #salary as: Float;
	define: #isMarried as: Boolean; 
	define: #address as: Address;
	define: #spouse as: Person; 
	define: #friends as: (OrderedCollection of: Person);

define: #productQuantities as: (Dictionary of: Product -> Integer);

	^(self cityOffices at: aCompanyOffice city ifAbsentPut: [OrderedCollection new]) add: aCompanyOffice

	^self cityOffices at: aCompanyOffice city ifPresent: [ :offices | offices remove: aCompanyOffice]
	(Dictionary 
of: [ :office | office address city] -> (OrderedCollection of: CompanyOffice) 
owner: #company);
	define: #gender as: Gender;
	define: #gender as: Gender;
	Book			- author, title, isbn, publisher
	MusicProduct	- artist, title, catNo
		Vinyl		- vinylWeight
		CD
	Person		- firstName, surname, address
	Address 	- line1, postcode

	^ false

	^ false
	addClass: Customer;
	addClass: CustomerOrder;
	addClass: CustomerOrderItem
	addClassWithSubclasses: Product
	ReStore synchronizeAllClasses