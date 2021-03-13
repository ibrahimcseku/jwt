######### API List #######################
http://localhost:8080/api/authenticate/reg
Method:POST
Conten-Type: application/json
Body:
	{
		"username":"Sara",
		"password":"12345",
		"role":["admin"]
	}
	
	{
		"username":"Ebu",
		"password":"12345",
		"role":["user"]
	}
	
http://localhost:8080/api/authenticate/login
Method:POST
Conten-Type: application/json
Body:
	{
		"username":"Sara",
		"password":"12345"
	}
	
	{
		"username":"Ebu",
		"password":"12345"
	}
	
Method: GET
http://localhost:8080/api/resource/content
http://localhost:8080/api/resource/user
http://localhost:8080/api/resource/admin
