## Getting Started
ReStore connection: (SSWSQLite3Connection on: (Smalltalk imageDirectory / 'test.db') fullName)
ReStore connection: (SSWP3Connection new url: 'psql://user:pwd@192.168.1.234:5432/database')
ReStore connection: 
(SSWMySQLConnection new 
	connectionSpec: 
		(MySQLDriverSpec new 
			db: 'database'; host: '192.168.1.234'; port: 3306; 
			user: 'user'; password: 'pwd'; 
		yourself); 
yourself)
ReStore disconnect.