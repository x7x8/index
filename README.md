# user-login-log GRPC service

## Function Introduction

##### User login device fingerprint storage
	When the user logs in successfully through SSO or other interfaces.Call this interface，
	The user UID,CID,IP,device fingerprint and other information are written in [device fingerprint records].
##### Dynamic start user login secondary authentication
	When the user needs to open a second authentication.
	Call the interface,The interface will query from table[USER_LOGIN_LOG] with the defined rules.
	Then returns whether to open a secondary authentication.

## How do build this service

##### turn to my-bazel directory,and then track this repository
```
cd my-bazel
sc track login-log-service
sc deps login-log-service
```

##### build server
> bazel build login-log-service/cmd:server

##### build client
>bazel build login-log-service/cmd:client

##### start skylb docker-compose
>docker-compose -f docker-compose/dev/skylb/docker-compose.yml up

##### start server
>./my-bazel/bazel-bin/login-log-service/cmd$ ./server --skylb-endpoints localhost:1900  --alsologtostderr -v=3

##### start client
>./my-bazel/bazel-bin/login-log-service/cmd$ ./client --skylb-endpoints localhost:1900  --alsologtostderr -v=3

## How to call this service

It can be called by SKYLB,This GRPC service is registered in SKYLB:vexpb.ServiceId_LOGIN_LOG_SERVICE

The GRPC client can pass this number,Create a GRPC client,such as:
skycli = skylb.NewServiceCli(vexpb.ServiceId_LOGIN_LOG_SERVICE)

For more details,see this file：user_login_log/cmd/client.go

## Data table(USER_LOGIN_LOG)
```
Column		type		length	key	remark

id		int		11	y	ID No,primary key,auto increment
uid		varchar		20	n	user UID
cid		varchar		20	n	user CID
ip		varchar		32	n	User login source IP
fingerContent	varchar		500	n	Fingerprint content,User-Agent,device ID,etc.
createTime	datetime	0	n	recording time
clientFlag	char		2	n	Client Identifier:1-web,2-app,3-pc
```
