## Update Clashes

	| myStockChange mergedStockLevel |
	(aSymbol = #numberInStock) ifFalse: [^false].
	myStockChange := self stockLevel – oldVersion stockLevel.
	mergedStockLevel := newVersion stockLevel + myStockChange.

	^mergedStockLevel >= 0
		ifTrue: [self stockLevel: newStockLevel. true]
		ifFalse: [false] 