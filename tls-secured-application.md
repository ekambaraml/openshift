# How to create TLS Secured Applications


1. Create project

   **oc new-project development
   ```
     Now using project "development" on server "https://api.smartcare.cp.fyre.ibm.com:6443".

     You can add applications to this project with the 'new-app' command. For example, try:

        oc new-app rails-postgresql-example

     to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

        kubectl create deployment hello-node --image=k8s.gcr.io/serve_hostname
   ```


   **oc describe project development
   
```
   Name:			development
   Created:		14 seconds ago
   Labels:			kubernetes.io/metadata.name=development
   Annotations:		openshift.io/description=
		   	openshift.io/display-name=
		  	openshift.io/requester=kube:admin
		  	openshift.io/sa.scc.mcs=s0:c27,c14
		  	openshift.io/sa.scc.supplemental-groups=1000730000/10000
		  	openshift.io/sa.scc.uid-range=1000730000/10000
   Display Name:		<none>
   Description:		<none>
   Status:			Active
   Node Selector:		<none>
   Quota:			<none>
   Resource limits:	<none>
  
   ```
   
   
   
2. Generate TLS certificate using openssl

   mkdir openssl
   cd openssl
   
   openssl genrsa -des3 -out myCA.key 2048
   ```
   Generating RSA private key, 2048 bit long modulus (2 primes)
   .......................+++++
   ..........................................................+++++
   e is 65537 (0x010001)
   Enter pass phrase for myCA.key:
   Verifying - Enter pass phrase for myCA.key:
   ```
   
   openssl req -x509 -new -nodes -key myCA.key -sha256 -days 3650 -out myCA.pem
   
   ```
   Enter pass phrase for myCA.key:
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   What you are about to enter is what is called a Distinguished Name or a DN.
   There are quite a few fields but you can leave some blank
   For some fields there will be a default value,
   If you enter '.', the field will be left blank.
   -----
   Country Name (2 letter code) [XX]:US
   State or Province Name (full name) []:KS
   Locality Name (eg, city) [Default City]:Leawood
   Organization Name (eg, company) [Default Company Ltd]:IBM
   Organizational Unit Name (eg, section) []:Data and AI
   Common Name (eg, your name or your server's hostname) []:api.smartcare.cp.fyre.ibm.com
   Email Address []:
   ```

   openssl genrsa -out tls.key 2048
   
   ```
   Generating RSA private key, 2048 bit long modulus (2 primes)
   .......................................+++++
   .....................................................+++++
   e is 65537 (0x010001)
   ```
   
   openssl req -new -key tls.key -out tls.csr
   
   ```
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   What you are about to enter is what is called a Distinguished Name or a DN.
   There are quite a few fields but you can leave some blank
   For some fields there will be a default value,
   If you enter '.', the field will be left blank.
   -----
   Country Name (2 letter code) [XX]:US
   State or Province Name (full name) []:KS
   Locality Name (eg, city) [Default City]:Leawood
   Organization Name (eg, company) [Default Company Ltd]:IBM
   Organizational Unit Name (eg, section) []:Data and AI
   Common Name (eg, your name or your server's hostname) []:api.smartcare.cp.fyre.ibm.com
   Email Address []:

   Please enter the following 'extra' attributes
   to be sent with your certificate request
   A challenge password []:demo
   An optional company name []:IBM

   ```

   openssl x509 -req -in tls.csr -CA myCA.pem -CAkey myCA.key -CAcreateserial -out tls.crt -days 1650 -sha256

   ```
   Signature ok
   subject=C = US, ST = KS, L = Leawood, O = IBM, OU = Data and AI, CN = api.smartcare.cp.fyre.ibm.com
   Getting CA Private Key
   Enter pass phrase for myCA.key:
   ```


3. Create Application




 
