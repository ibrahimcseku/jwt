############################################
Workspace directory: D:\Ibrahim\Self_Learning\Microservice\workspace
Eclipse Directory: D:\Software\My_Tools\eclipse\eclipse-java-2020-12-R-win32-x86_64\eclipse

############MySql Properties:###############
port:3306
x port:33060
User&pass: 
root/Admin@12345
admin/Admin@12345

################db-service###################
	quotations add service:
		http://localhost:8181/rest/db/add
		{
		"userName": "Sara",
		"quotations": ["GOOG","TSLA","AAPLE"]
		}

	Get quotations: http://localhost:8181/rest/db/sara
	
###################stock-service##############
		http://localhost:8282/rest/stock/{username}
		http://localhost:8282/rest/stock/sara
		[{"quote":"GOOG","price":2129.08},{"quote":"TSLA","price":788.6214}]

###################consul-service###############
Centralize configuration using spring cloud consul:
	Consul Installation:
		1. Go to the consul directory (D:\Ibrahim\Self_Learning\Microservice\consul_1.9.3_windows_amd64\) and open command prompt
		2. Run command to check IP and bind the consul service and run the consul agent service: 
			> ipconfig
			> consul agent -server -bootstrap-expect=1 -data-dir=consul-data -ui -bind=YOUR_IP_ADDRESS
			
			Example for my local machine:
				consul agent -server -bootstrap-expect=1 -data-dir=consul-data -ui -bind=172.31.36.69
	
		3. Check in browser:
			http://localhost:8500/
		
		Note: Registered Service are:
				1. db-service
				2. stock-service

################### APIGateway using zuul proxy #################
	For db service Get quotations:
		Url:http://localhost:8383/api/db-service/rest/db/{userName}
		example:http://localhost:8383/api/db-service/rest/db/Sara
		
	For quotations add service:
		Url:http://localhost:8383/api/db-service/rest/db/add
		Content-Type: application/json
		Body: { "userName": "Soma", "quotations": ["AAPLE"] }
		
	For stock service:
		Url:http://localhost:8383/api/stock-service/rest/stock/{username}
		Example:http://localhost:8383/api/stock-service/rest/stock/sara
		[{"quote":"GOOG","price":2129.08},{"quote":"TSLA","price":788.6214}]
		
################## Including JWT Authentication in APIGateway ###################
Ref Url:
https://bezkoder.com/spring-boot-jwt-authentication/
1. Get Token:
	Request Url:http://localhost:8383/authenticate
	Refresh Request:
	Request Body:{"username":"ebu","password":"Admin@12345"}
	In Response: 
	{
		"token": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJpbjI4bWludXRlcyIsImV4cCI6MTYxNDQyMjUyNiwiaWF0IjoxNjEzODE3NzI2fQ.7rQncqo28rdYJ2rKtHMSy3RAVbiLnnsXg7m9l0ypXYGduSS5IrYd8S5bhJtgpjC7gAc9HjexjCK3M8kDe_2vJw"
	}
2. Above token will be used to call all other API. So,we need to this token in request header.
	Authorization:Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJlYnUiLCJleHAiOjE2MTQ0Mjc1NzEsImlhdCI6MTYxMzgyMjc3MX0.PJLVezyKo9NmrlA9olRBmOh7b5mEBmPRA64AGyUW2QP8gltPHhbmpU4RHI4EW5jt3bImnAwxI8UIOq73At71yg


Working flow:
JWTWebSecurityConfig > configureGlobal() > passwordEncoderBean() > authenticationManagerBean() > configure(HttpSecurity httpSecurity) > 
configure(WebSecurity webSecurity)



JwtTokenAuthorizationOncePerRequestFilter > doFilterInternal() > 
JwtAuthenticationRestController > createAuthenticationToken() > authenticate() > JwtInMemoryUserDetailsService > loadUserByUsername()> JwtUserDetails > JwtUserDetails() > JwtInMemoryUserDetailsService > loadUserByUsername() >
JwtTokenUtil > generateToken() > doGenerateToken() HS512, secret > JwtTokenResponse > JwtTokenResponse() > getToken()

--- without token req --
JwtTokenAuthorizationOncePerRequestFilter >  doFilterInternal() 
if Authorization token is not set
	>  JwtUnAuthorizedResponseAuthenticationEntryPoint >  commence () > through  Exception()
token is set
	JwtTokenUtil >  getClaimFromToken() > getAllClaimsFromToken() > JwtInMemoryUserDetailsService > loadUserByUsername() > JwtUserDetails > JwtUserDetails() > JwtTokenUtil > validateToken() > getClaimFromToken() > getAllClaimsFromToken() > isTokenExpired() > getClaimFromToken() > getAllClaimsFromToken()
	
------------
Note for documentation:
https://developer.equinix.com/blog/microservices-registration-and-discovery-using-consul
https://thenewstack.io/implementing-service-discovery-of-microservices-with-consul/
https://www.digitalocean.com/community/tutorials/an-introduction-to-using-consul-a-service-discovery-system-on-ubuntu-14-04
https://www.infoq.com/articles/Microservices-SpringBoot/
https://howtodoinjava.com/spring-cloud/consul-service-registration-discovery/	

---------------
#git init -b main
git init
git add .
git status
git commit -m "First commit of source code"
git log --oneline
#Checking out a file from an earlier commit
#git checkout <second commit's number> index.html

#Resetting the Git repository
#git reset HEAD index.html
#git checkout -- index.html

#git remote add origin <repository URL>
git remote add origin https://github.com/ibrahimcseku/microservice.git
#git remote set-url origin https://github.com/ibrahimcseku/microservice.git
git push -u origin master
#git push -u origin main

#git clone https://github.com/ibrahimcseku/microservice.git

#git clone https://github.com/ibrahimcseku/jwt.git

![Test Image 4](https://github.com/ibrahimcseku/microservice/blob/master/microservice_image.png)
		
		