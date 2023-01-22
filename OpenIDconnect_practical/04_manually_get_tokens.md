
json to picture
	first https://jsonformatter.curiousconcept.com
	then https://onlinejsontools.com/convert-json-to-image
		size: 25px
		color scheme: liquibyte

jwt
	https://jwt.io/

---


export issuer_url=https://10.20.116.209.nip.io/realms/master
export token_endpoint="${issuer_url}/protocol/openid-connect/token"
export authorization_endpoint="${issuer_url}/protocol/openid-connect/auth"


a) request webfinger endpoint for discovery of all supported modes and endpoints
see: OIDC discovery specification for details

curl --insecure ${issuer_url}/.well-known/openid-configuration

open in browser:
    https://10.20.116.209.nip.io/realms/master/.well-known/openid-configuration

important:
    all the endpoint -> token, authorization, userinfo
    grant_types_supported
    response_types_supported
    jwks_uri -> has the certificates, which are used to verify the token
    scopes -> openid scope we use
    and also the crypto algorithms supported
    -> THIS webfinger page is for all clients, not only for the kubernetes one

b) Simulate "Direct Access Grant"
see also: https://github.com/akoserwal/keycloak-integrations/blob/master/curl-post-request/keycloak-curl.sh
Direct Access Grant -> A client is using user-credentials to get an access token directly
    User credentials are those of the testuser

The grant type is "password", which means we want to do direct access grant

THIS WILL NOT WORK
	because:
		- we enabled client authentication
		- this is now also needed for "Direct Access Grant" 
		  although we have the user credentials
	curl -X POST "${token_endpoint}" --insecure \
	 -H "Content-Type: application/x-www-form-urlencoded" \
	 -d "username=testuser" \
	 -d "password=testpass" \
	 -d 'grant_type=password' \
	 -d "client_id=kubernetes"
	-> {"error":"unauthorized_client","error_description":"Invalid client or Invalid client credentials"}

this will work
	- here we also add client credentials
	curl -X POST "${token_endpoint}" --insecure \
	 -H "Content-Type: application/x-www-form-urlencoded" \
	 -d "username=testuser" \
	 -d "password=testpass" \
	 -d 'grant_type=password' \
	 -d "client_id=kubernetes" \
	 -d "client_secret=FPltK3k7eA5agTbnTmwc0ounthRVVNaW"
	 -> {"access_token":"eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUMXBjTVBlbzd4QmItdkZPR1NGUFl0OXJlZDNWdTdFOWJrRkFoN203cjRRIn0.eyJleHAiOjE2NzQzMzc5NzksImlhdCI6MTY3NDMzNzkxOSwianRpIjoiODc1ZjIwMzItOTY1Yy00MzMyLWFiZjYtYTgxMjFkNzM2NjI4IiwiaXNzIjoiaHR0cHM6Ly8xMC4yMC4xMTYuMjA5Lm5pcC5pby9yZWFsbXMvbWFzdGVyIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjAxODcxZWFkLTdiOTUtNDEwYS04YjA5LTEwOGQ2ZGExYTNlOCIsInR5cCI6IkJlYXJlciIsImF6cCI6Imt1YmVybmV0ZXMiLCJzZXNzaW9uX3N0YXRlIjoiODQzYjRmNDEtNjllZi00ZDQwLWExYzUtNWI1ODdjYThjMzI3IiwiYWNyIjoiMSIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLW1hc3RlciIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJrdWJlcm5ldGVzIjp7InJvbGVzIjpbImt1YmVybmV0ZXNfcm9sZSJdfSwiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJwcm9maWxlIGVtYWlsIiwic2lkIjoiODQzYjRmNDEtNjllZi00ZDQwLWExYzUtNWI1ODdjYThjMzI3IiwiZW1haWxfdmVyaWZpZWQiOnRydWUsIm5hbWUiOiJmaXJzdG5hbWUgbGFzdG5hbWUiLCJncm91cHMiOlsia3ViZXJuZXRlc19yb2xlIiwibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJ0ZXN0dXNlciIsImdpdmVuX25hbWUiOiJmaXJzdG5hbWUiLCJmYW1pbHlfbmFtZSI6Imxhc3RuYW1lIiwiZW1haWwiOiJ0ZXN0dXNlckB0ZXN0dXNlci5kZSJ9.Jv3q8UQcyRPyq0uGBqzwzgTdFmSZzSw0vJi2uYp9RulzQ7Ih7OnoOVbFZY8E4benaQ7JVH6Q0MTh8nebKKANW4qb5m-mTfQCqSozzK-20ehn1ZXXOr8TV-P59qKa0EFOpYsHoz06Q3tftbkPKfrFGCTeqxRdCvddPoFCia_AHV0b96L0FaNKVp_o70Q_YIaahYYmNJBJlSE5t6wYj_Yfrj-uywBQnvZX_pCDCwwYnxZfxPsJMBjmDqJXBgid41ww812m2PgTM4frAGffXl5gjxx4O2Wnvt_GugDwW8-WPd2ZOweoc0DsmwKsoMywPbVvXItUj2kJCsMseIEic6eIFw","expires_in":60,"refresh_expires_in":1800,"refresh_token":"eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJhNTBmYmI4MS1hYTA0LTQ0NzItYWMxYi00OTBhMmFlMzA1M2UifQ.eyJleHAiOjE2NzQzMzk3MTksImlhdCI6MTY3NDMzNzkxOSwianRpIjoiYmJlOGY0ZTAtMmY4OS00ZDhlLThhOTItZWI1NjJkYzI2MWY3IiwiaXNzIjoiaHR0cHM6Ly8xMC4yMC4xMTYuMjA5Lm5pcC5pby9yZWFsbXMvbWFzdGVyIiwiYXVkIjoiaHR0cHM6Ly8xMC4yMC4xMTYuMjA5Lm5pcC5pby9yZWFsbXMvbWFzdGVyIiwic3ViIjoiMDE4NzFlYWQtN2I5NS00MTBhLThiMDktMTA4ZDZkYTFhM2U4IiwidHlwIjoiUmVmcmVzaCIsImF6cCI6Imt1YmVybmV0ZXMiLCJzZXNzaW9uX3N0YXRlIjoiODQzYjRmNDEtNjllZi00ZDQwLWExYzUtNWI1ODdjYThjMzI3Iiwic2NvcGUiOiJwcm9maWxlIGVtYWlsIiwic2lkIjoiODQzYjRmNDEtNjllZi00ZDQwLWExYzUtNWI1ODdjYThjMzI3In0.gh-oOjqaRwGQa9t-MI6wArjdwrnLzCIgENCnSFht4zM","token_type":"Bearer","not-before-policy":0,"session_state":"843b4f41-69ef-4d40-a1c5-5b587ca8c327","scope":"profile email"}

	 interesting
	 	- "expires_in":60,
	 	- we also get a refresh_token









c) Simulate "Authorization Code Flow"

sources
	https://openid.net/specs/openid-connect-core-1_0.html#AuthRequest
	https://darutk.medium.com/diagrams-of-all-the-openid-connect-flows-6968e3990660
	https://fireflysemantics.medium.com/oauth-authorization-code-grant-flow-demonstrated-with-curl-71543ba6c3f7
	https://auth0.com/docs/get-started/authentication-and-authorization-flow/call-your-api-using-the-authorization-code-flow

there was a problem
        k get pods
		k logs my-release-nginx-ingress-vwc64 -f
			2023/01/19 02:25:10 [error] 73#73: *3284 upstream sent too big header while reading response header from upstream, client: 10.20.116.209, server: 10.20.116.209.nip.io, request: "GET /realms/master/protocol/openid-connect/auth?response_type=code&scope=openid&grant_type=authorization_code&client_id=kubernetes&client_secret=aDeeUt5uKeLmGTrkPsARGTET8FOcBRZx&state=12345&redirect_uri=http:%2F%2Flocalhost:8000 HTTP/1.1", upstream: "http://192.168.50.205:8080/realms/master/protocol/openid-connect/auth?response_type=code&scope=openid&grant_type=authorization_code&client_id=kubernetes&client_secret=aDeeUt5uKeLmGTrkPsARGTET8FOcBRZx&state=12345&redirect_uri=http:%2F%2Flocalhost:8000", host: "10.20.116.209.nip.io"
			10.20.116.209 - - [19/Jan/2023:02:25:10 +0000] "GET /realms/master/protocol/openid-connect/auth?response_type=code&scope=openid&grant_type=authorization_code&client_id=kubernetes&client_secret=aDeeUt5uKeLmGTrkPsARGTET8FOcBRZx&state=12345&redirect_uri=http:%2F%2Flocalhost:8000 HTTP/1.1" 502 157 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/109.0" "-"

    Those annotations in the ingress fixed the error:
        annotations:
          nginx.org/proxy-buffer-size: 128k
          nginx.org/proxy-buffers: 4 256k
          nginx.org/proxy-busy_buffers-size: 256k

#### lets start with the authorization code flow

we do: response_type=code + scope=openid + grant_type=authorization_code
response_type=code <- which tokens we get as a result
    see https://darutk.medium.com/diagrams-of-all-the-openid-connect-flows-6968e3990660
scope=openid <- has to be like that by specification for OIDC
grant_type <- authorization_code
state is only a security feature and shall come back identical from the server

1. Authentication Request

we need to open the browser and send a request to the authorization endpoint
We can send GET or POST
but we need to log out of keycloak, so we can be testuser instead of admin

http request has a header and a body
the body is specified with the -d parameters
by setting content type to urlencoded we get something like this in the body
MyVariableOne=ValueOne&MyVariableTwo=ValueTwo

if the content MIME type would be application/json is would look like this:
{"test":"test"}

we could issue this curl request:
curl -X POST "${authorization_endpoint}" --insecure \
 -H "Content-Type: application/x-www-form-urlencoded" \
 -d 'response_type=code' \
 -d "scope=openid" \
 -d 'grant_type=authorization_code' \
 -d 'redirect_uri=http://localhost:8000' \
 -d "client_id=kubernetes" \
 -d "client_secret=FPltK3k7eA5agTbnTmwc0ounthRVVNaW" \
 -d 'state=12345'

but the user needs to login, therefore we open this url in the browser:
https://10.20.116.209.nip.io/realms/master/protocol/openid-connect/auth?response_type=code&scope=openid&grant_type=authorization_code&client_id=kubernetes&client_secret=FPltK3k7eA5agTbnTmwc0ounthRVVNaW&state=12345&redirect_uri=http://localhost:8000

2. Authentication Request Validation
- performed by keycloak internally

3. Authorization Server Authenticates End-User
- the user logs in

4. Successful Authentication Response
- the browser is redirected to the configured redirect URI:
- and the authorization code is appended to this url as parameter
http://localhost:8000/?state=12345&session_state=76e4b042-7c28-45a9-bf1c-db033a4e22e5&code=771c8765-caf7-43f7-b478-7bbd8f5398e9.76e4b042-7c28-45a9-bf1c-db033a4e22e5.5cc99d26-aa42-4952-bd19-2b1240c04d63

-> we now have the authorization code
-> debug tip: this code will be invalidate after each try

5. Token Request
via token endpoint
curl --request POST --insecure \
  --url "${token_endpoint}" \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data grant_type=authorization_code \
  --data 'client_id=kubernetes' \
  --data 'client_secret=FPltK3k7eA5agTbnTmwc0ounthRVVNaW' \
  --data 'code=771c8765-caf7-43f7-b478-7bbd8f5398e9.76e4b042-7c28-45a9-bf1c-db033a4e22e5.5cc99d26-aa42-4952-bd19-2b1240c04d63' \
  --data 'redirect_uri=http://localhost:8000'

finally the tokens:
	{"access_token":"eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUMXBjTVBlbzd4QmItdkZPR1NGUFl0OXJlZDNWdTdFOWJrRkFoN203cjRRIn0.eyJleHAiOjE2NzQzMzk1MDIsImlhdCI6MTY3NDMzOTQ0MiwiYXV0aF90aW1lIjoxNjc0MzM4OTkxLCJqdGkiOiI0YjczYTAxYy1kYzNiLTRmNDEtOWI0NS01Y2Y0ZjU0MjVmZDIiLCJpc3MiOiJodHRwczovLzEwLjIwLjExNi4yMDkubmlwLmlvL3JlYWxtcy9tYXN0ZXIiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMDE4NzFlYWQtN2I5NS00MTBhLThiMDktMTA4ZDZkYTFhM2U4IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoia3ViZXJuZXRlcyIsInNlc3Npb25fc3RhdGUiOiI3NmU0YjA0Mi03YzI4LTQ1YTktYmYxYy1kYjAzM2E0ZTIyZTUiLCJhY3IiOiIwIiwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbImRlZmF1bHQtcm9sZXMtbWFzdGVyIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7Imt1YmVybmV0ZXMiOnsicm9sZXMiOlsia3ViZXJuZXRlc19yb2xlIl19LCJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6Im9wZW5pZCBwcm9maWxlIGVtYWlsIiwic2lkIjoiNzZlNGIwNDItN2MyOC00NWE5LWJmMWMtZGIwMzNhNGUyMmU1IiwiZW1haWxfdmVyaWZpZWQiOnRydWUsIm5hbWUiOiJmaXJzdG5hbWUgbGFzdG5hbWUiLCJncm91cHMiOlsia3ViZXJuZXRlc19yb2xlIiwibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJ0ZXN0dXNlciIsImdpdmVuX25hbWUiOiJmaXJzdG5hbWUiLCJmYW1pbHlfbmFtZSI6Imxhc3RuYW1lIiwiZW1haWwiOiJ0ZXN0dXNlckB0ZXN0dXNlci5kZSJ9.qXogQA8Pb7c4ADALt5HYpdTB87LfRMhyhzQHjae9wrROCOq3dl8lIi_jE5vi-2muWOlFQW_nrlPfYrfAs1ER8bHBsAiGe3zZiW4C_hMKPaFxk0Bt2QNCAGcWNfOy83qacD8cy-kdo5kk-P06UgjB0--IXxs8wlacQysBBc-cTiQBrzZDz9hfziuODr5_Va5VgDG9J8fyJEv1jxsIhZI8jayjwf7swtGlG849jXH6s-QiyG-24dswzA2NwtK8x6mnCXHLWAVCmx0fxNfKMrzVH8HMaw98NUWcdw2N4ScWQbg4FKoP2RmViG1HVD13D1AlpOF9KxvhJxTqByceyReWZA","expires_in":60,"refresh_expires_in":1800,"refresh_token":"eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJhNTBmYmI4MS1hYTA0LTQ0NzItYWMxYi00OTBhMmFlMzA1M2UifQ.eyJleHAiOjE2NzQzNDEyNDIsImlhdCI6MTY3NDMzOTQ0MiwianRpIjoiYjc1NzY1ODktMzFlNy00MWQ0LTg5NmItYWU0OWY2NzY1ZmI0IiwiaXNzIjoiaHR0cHM6Ly8xMC4yMC4xMTYuMjA5Lm5pcC5pby9yZWFsbXMvbWFzdGVyIiwiYXVkIjoiaHR0cHM6Ly8xMC4yMC4xMTYuMjA5Lm5pcC5pby9yZWFsbXMvbWFzdGVyIiwic3ViIjoiMDE4NzFlYWQtN2I5NS00MTBhLThiMDktMTA4ZDZkYTFhM2U4IiwidHlwIjoiUmVmcmVzaCIsImF6cCI6Imt1YmVybmV0ZXMiLCJzZXNzaW9uX3N0YXRlIjoiNzZlNGIwNDItN2MyOC00NWE5LWJmMWMtZGIwMzNhNGUyMmU1Iiwic2NvcGUiOiJvcGVuaWQgcHJvZmlsZSBlbWFpbCIsInNpZCI6Ijc2ZTRiMDQyLTdjMjgtNDVhOS1iZjFjLWRiMDMzYTRlMjJlNSJ9.Yb79YV_JpzFcKq0ahRvDYLFu6ojH5C4dpVSbhVv_1VY","token_type":"Bearer","id_token":"eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUMXBjTVBlbzd4QmItdkZPR1NGUFl0OXJlZDNWdTdFOWJrRkFoN203cjRRIn0.eyJleHAiOjE2NzQzMzk1MDIsImlhdCI6MTY3NDMzOTQ0MiwiYXV0aF90aW1lIjoxNjc0MzM4OTkxLCJqdGkiOiJhYjA3OGZiMi1mMmE2LTQxNGUtYmIzMS02NjA4OTEwODliZTEiLCJpc3MiOiJodHRwczovLzEwLjIwLjExNi4yMDkubmlwLmlvL3JlYWxtcy9tYXN0ZXIiLCJhdWQiOiJrdWJlcm5ldGVzIiwic3ViIjoiMDE4NzFlYWQtN2I5NS00MTBhLThiMDktMTA4ZDZkYTFhM2U4IiwidHlwIjoiSUQiLCJhenAiOiJrdWJlcm5ldGVzIiwic2Vzc2lvbl9zdGF0ZSI6Ijc2ZTRiMDQyLTdjMjgtNDVhOS1iZjFjLWRiMDMzYTRlMjJlNSIsImF0X2hhc2giOiJxMU1LTXpWVGJQVUtFTml5dmw1R2FnIiwiYWNyIjoiMCIsInNpZCI6Ijc2ZTRiMDQyLTdjMjgtNDVhOS1iZjFjLWRiMDMzYTRlMjJlNSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJuYW1lIjoiZmlyc3RuYW1lIGxhc3RuYW1lIiwiZ3JvdXBzIjpbImt1YmVybmV0ZXNfcm9sZSIsIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXSwicHJlZmVycmVkX3VzZXJuYW1lIjoidGVzdHVzZXIiLCJnaXZlbl9uYW1lIjoiZmlyc3RuYW1lIiwiZmFtaWx5X25hbWUiOiJsYXN0bmFtZSIsImVtYWlsIjoidGVzdHVzZXJAdGVzdHVzZXIuZGUifQ.uHfBYriCd1vBssOzVU87OQkhV8ZU5RXkmhcX1eGy1x_fYKw8cUNVYLrjW7sOgiZPbzpRw4Fci1v43lFg_WbS0ahLgdjXrY4wCNm0M0trVjtD5Gg8AqbftVxeDzuJrGjHRbSejxOtqMMZVqteYbw5lNWDVT1SKXdaSCTryLWIjHTSWKYjGw_PEd_4HFIUtch6vTe0KgO0jzbI3qIB6hf6BG0bkd-BNHdfbtfLcUk2VCknU1ZY0drtqseCPcvoL4Q3glGTI98XzIUSbFfMbYekwF1U81RQ8JMA59ZGk9PjWa8CmE6MBJ3EC0rNmWiDi16T_--uKXhIlem4mE8NetSFnw","not-before-policy":0,"session_state":"76e4b042-7c28-45a9-bf1c-db033a4e22e5","scope":"openid profile email"}


d) Simulate using refresh_token
source: https://auth0.com/docs/authenticate/login/oidc-conformant-authentication/oidc-adoption-refresh-tokens

curl --request POST --insecure \
  --url "${token_endpoint}" \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data grant_type=refresh_token \
  --data 'scope=openid' \
  --data 'client_id=kubernetes' \
  --data 'client_secret=FPltK3k7eA5agTbnTmwc0ounthRVVNaW' \
  --data 'refresh_token=eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJhNTBmYmI4MS1hYTA0LTQ0NzItYWMxYi00OTBhMmFlMzA1M2UifQ.eyJleHAiOjE2NzQzNDEyNDIsImlhdCI6MTY3NDMzOTQ0MiwianRpIjoiYjc1NzY1ODktMzFlNy00MWQ0LTg5NmItYWU0OWY2NzY1ZmI0IiwiaXNzIjoiaHR0cHM6Ly8xMC4yMC4xMTYuMjA5Lm5pcC5pby9yZWFsbXMvbWFzdGVyIiwiYXVkIjoiaHR0cHM6Ly8xMC4yMC4xMTYuMjA5Lm5pcC5pby9yZWFsbXMvbWFzdGVyIiwic3ViIjoiMDE4NzFlYWQtN2I5NS00MTBhLThiMDktMTA4ZDZkYTFhM2U4IiwidHlwIjoiUmVmcmVzaCIsImF6cCI6Imt1YmVybmV0ZXMiLCJzZXNzaW9uX3N0YXRlIjoiNzZlNGIwNDItN2MyOC00NWE5LWJmMWMtZGIwMzNhNGUyMmU1Iiwic2NvcGUiOiJvcGVuaWQgcHJvZmlsZSBlbWFpbCIsInNpZCI6Ijc2ZTRiMDQyLTdjMjgtNDVhOS1iZjFjLWRiMDMzYTRlMjJlNSJ9.Yb79YV_JpzFcKq0ahRvDYLFu6ojH5C4dpVSbhVv_1VY'

-> works
{"access_token":"eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUMXBjTVBlbzd4QmItdkZPR1NGUFl0OXJlZDNWdTdFOWJrRkFoN203cjRRIn0.eyJleHAiOjE2NzQzMzk2NDIsImlhdCI6MTY3NDMzOTU4MiwiYXV0aF90aW1lIjoxNjc0MzM4OTkxLCJqdGkiOiI4ZjkyM2FjZC1mMzhlLTQ1YmEtOTNjOC0xZjk3NzYxYmUxNjMiLCJpc3MiOiJodHRwczovLzEwLjIwLjExNi4yMDkubmlwLmlvL3JlYWxtcy9tYXN0ZXIiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMDE4NzFlYWQtN2I5NS00MTBhLThiMDktMTA4ZDZkYTFhM2U4IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoia3ViZXJuZXRlcyIsInNlc3Npb25fc3RhdGUiOiI3NmU0YjA0Mi03YzI4LTQ1YTktYmYxYy1kYjAzM2E0ZTIyZTUiLCJhY3IiOiIwIiwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbImRlZmF1bHQtcm9sZXMtbWFzdGVyIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7Imt1YmVybmV0ZXMiOnsicm9sZXMiOlsia3ViZXJuZXRlc19yb2xlIl19LCJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6Im9wZW5pZCBwcm9maWxlIGVtYWlsIiwic2lkIjoiNzZlNGIwNDItN2MyOC00NWE5LWJmMWMtZGIwMzNhNGUyMmU1IiwiZW1haWxfdmVyaWZpZWQiOnRydWUsIm5hbWUiOiJmaXJzdG5hbWUgbGFzdG5hbWUiLCJncm91cHMiOlsia3ViZXJuZXRlc19yb2xlIiwibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJ0ZXN0dXNlciIsImdpdmVuX25hbWUiOiJmaXJzdG5hbWUiLCJmYW1pbHlfbmFtZSI6Imxhc3RuYW1lIiwiZW1haWwiOiJ0ZXN0dXNlckB0ZXN0dXNlci5kZSJ9.moD7rar7RUxgzQcm-6knL6jwSyXwoeXNCoDaWmmQ80UqDOcBzlt87FgwoYj_hg-Onn5NEE0NqsSssjMghYu9z8FsWYlsTVbiIue7k2PI5g-kCOVWzFp87iXA1vrc15La4Jm5ehc5lQK9pg472altn97JUX1GJRIZ5_t23-boBZ78fa-u76TQyoUfci3bm33hCdNzlmWLCGx4s-YLVGsrKXtVrW4wiYNLnUPSTps1eDiH885BRwebMvI4NLLBpRlneP2uRCSgmjPY-Qrq61MzcidCQZsiF5B4P1_dBiEaW7kExvahQtD4ZJeybfpIo4IeGDhH32CE5gf2LW4LzPNxuw","expires_in":60,"refresh_expires_in":1800,"refresh_token":"eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJhNTBmYmI4MS1hYTA0LTQ0NzItYWMxYi00OTBhMmFlMzA1M2UifQ.eyJleHAiOjE2NzQzNDEzODIsImlhdCI6MTY3NDMzOTU4MiwianRpIjoiODk2NTA5MTctMzkzYi00YjViLWIxNTYtZmRmYmMyODNiZjdhIiwiaXNzIjoiaHR0cHM6Ly8xMC4yMC4xMTYuMjA5Lm5pcC5pby9yZWFsbXMvbWFzdGVyIiwiYXVkIjoiaHR0cHM6Ly8xMC4yMC4xMTYuMjA5Lm5pcC5pby9yZWFsbXMvbWFzdGVyIiwic3ViIjoiMDE4NzFlYWQtN2I5NS00MTBhLThiMDktMTA4ZDZkYTFhM2U4IiwidHlwIjoiUmVmcmVzaCIsImF6cCI6Imt1YmVybmV0ZXMiLCJzZXNzaW9uX3N0YXRlIjoiNzZlNGIwNDItN2MyOC00NWE5LWJmMWMtZGIwMzNhNGUyMmU1Iiwic2NvcGUiOiJvcGVuaWQgcHJvZmlsZSBlbWFpbCIsInNpZCI6Ijc2ZTRiMDQyLTdjMjgtNDVhOS1iZjFjLWRiMDMzYTRlMjJlNSJ9.OnRSXDlOztZICUyzOuxSYfSZJUF40shBPNRaKBXNcns","token_type":"Bearer","id_token":"eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUMXBjTVBlbzd4QmItdkZPR1NGUFl0OXJlZDNWdTdFOWJrRkFoN203cjRRIn0.eyJleHAiOjE2NzQzMzk2NDIsImlhdCI6MTY3NDMzOTU4MiwiYXV0aF90aW1lIjoxNjc0MzM4OTkxLCJqdGkiOiJkMGFlZmZhYS1jYmViLTQyMTktODM3Mi00OTMwNDRkZTNlYmUiLCJpc3MiOiJodHRwczovLzEwLjIwLjExNi4yMDkubmlwLmlvL3JlYWxtcy9tYXN0ZXIiLCJhdWQiOiJrdWJlcm5ldGVzIiwic3ViIjoiMDE4NzFlYWQtN2I5NS00MTBhLThiMDktMTA4ZDZkYTFhM2U4IiwidHlwIjoiSUQiLCJhenAiOiJrdWJlcm5ldGVzIiwic2Vzc2lvbl9zdGF0ZSI6Ijc2ZTRiMDQyLTdjMjgtNDVhOS1iZjFjLWRiMDMzYTRlMjJlNSIsImF0X2hhc2giOiI3eW5wbnlIRlctcGhuMGFOR1UyTWFBIiwiYWNyIjoiMCIsInNpZCI6Ijc2ZTRiMDQyLTdjMjgtNDVhOS1iZjFjLWRiMDMzYTRlMjJlNSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJuYW1lIjoiZmlyc3RuYW1lIGxhc3RuYW1lIiwiZ3JvdXBzIjpbImt1YmVybmV0ZXNfcm9sZSIsIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXSwicHJlZmVycmVkX3VzZXJuYW1lIjoidGVzdHVzZXIiLCJnaXZlbl9uYW1lIjoiZmlyc3RuYW1lIiwiZmFtaWx5X25hbWUiOiJsYXN0bmFtZSIsImVtYWlsIjoidGVzdHVzZXJAdGVzdHVzZXIuZGUifQ.Q_gnOscHhrW4fMk3pDoAWLSD6ezw67vWFOIenBuVPb6NbMA4mSU9BKuf3vduBwNXM_SZoKHtd5NgOR1NSNUJguppTAeaF-cDCUieoqktbnsHF0DwxCX_iBlWRuA1_g-0bG8pzG7gLmsBfFHW-CowWjZgj3i74ZrNYP2YJN2XDdy5Xgi9Fjjtf7I6iTS95rsqdob_Vshx4_b-6Zo4ZcuqhV_0xS-6Tj-iXrM_uKmz-krdX3BAe6Kye4tW5wQY1ZJlWk2tJIIwFAg6jfX3tlbd0H8dUShLVf6c1nKHuVtxZbNPDw8ozbR4T7dhk-Yydq67aAonbYHNSOOD19KlhxCTdg","not-before-policy":0,"session_state":"76e4b042-7c28-45a9-bf1c-db033a4e22e5","scope":"openid profile email"}
