# Visualization and Server Design

## Introduction
Built a Demo or Visualization platform to show checkmate.
So the checkmate need a way to input command and output result.
And the I/O data should able to visit in Website/GUI/Mobile App.


## Resource
We need to design and implementation of each part communication.

There is two type of resource:

1. Services
```
    +----------+ Item checkmate runtime
    | RUNTIME  | 
    |      +---| In:  command, arguments
    |      | S | Out: results
    +----------+

    +----------+ Item CPU Statistics
    |CPU       | 
    |      +---| In:  process id
    |      | S | Out: CPU used
    +----------+

    +----------+ Item Timeout manager
    |TIMEOUT   | 
    |      +---| In:  communication
    |      | S | Out: communication timeout
    +----------+

    +----------+ Item Service Registry
    |Ser-Reg   | 
    |      +---| In:  application, exchange information
    |      | S | Out: origin, destination
    +----------+

    +----------+ Item Path Finder
    |PATH      | 
    |      +---| In:  Metrixs, list
    |      | S | Out: path
    +----------+

    +----------+ Item Path Finder
    |PATH      | 
    |      +---| In:  Metrixs, list
    |      | S | Out: path
    +----------+
```
2. Client
  Client can be a website/GUI/MobileApps/etc and even another Service.
```
    +----------+ Item Client
    |Client    | 
    |          | In:  Request
    |          | Out: Visualization Output
    +----------+
```
This flag:
```
   +---| 
   | S |
   ----+
```
It's a Server. Plase read it's information in Module part.

## Design
There are 3 kinds of design:

1. Simplest but high coupling
Item Server is a web server(like Flask, bootle and Django),
and each Services will has an web server instance with a port.
Use httprequest to send/get data.

Like this:
```
        +----------+ 
        |  Client  | 
        +----------+
             |
             | Http Get/Post
             |
             ▼
        +----------+
        | Services |
        +----------+
```

probelm:
- Each Services must has a port.
- The more services, Client need to connect more port.
- Such as runtime should connect TimeoutManager service, CPU service
  PATH service. 3 Ports.
- And website client can't build applications(runtime services) list.

2. Flexible server
Firstly, add a server:
```
    +----------+ Item Services Server
    |SS        | 
    |      +---| In:  command, args
    |      | S | Out: result
    +----------+
```
Each services will connect to this Services Server, and services
get data from Services Server.
SS is a custom-built Server, has a communication class Server.
The realization of a Server Module by using zmq/http server, etc.

```
                           +----+
                           | SS |
                           +----+
                              | 
                              | Request/Respond
                              | 
        --------------------------------------------
        |             |              |             |
        |             |              |             |
        |             |              |             |
 +----------+    +----------+   +----------+   +----------+
 | WebServer|    | Runtime  |   | CPU      |   | Others   |
 +----------+    +----------+   +----------+   +----------+
     ▲
     | Http Get/Post
     |
     ▼
 +----------+
 |  Client  |
 +----------+
```
So Services Server know which service are in provision.
Client can ask WebServer to get Runtime services list from SS.

Problem:
- No Load-balance.
- Dev the SS code ourselves.

3.zato server
Zato has Load-balance and load-balancer’s statistics.
Zato support many Outgoing connections, such as Plain HTTP, SQL, ZeroMQ.

```
                    +----------+
                    |  Caller  |
                    +----------+
                         ▲
                         | Http Get/Post
                         | /zmq request
                         | /sql, nosql request
                         ▼
                    +----------+
                    |   Zato   |
                    +----------+
                         |
                         |
     ---------------------------------------------
     |            |               |              |
     |            |               |              |
+---------+    +---------+   +---------+   +---------+
| Zato    |    | Zato    |   | Zato    |   | Zato    |
| Service |    | Service |   | Service |   | Service |
+---------+    +---------+   +---------+   +---------+
     ▲              ▲             ▲              ▲
     |              |             |              |
     ▼              ▼             ▼              ▼
+---------+    +---------+   +---------+    +---------+
| Cpu     |    | Path    |   | Runtime |    | Timeout |
| Server  |    | Server  |   | Server  |    | Server  |
+---------+    +---------+   +---------+    +---------+
```
Issue:
To get runtime list, should each Zato Service has only one Runtime
or One Zato Service has multi runtimes(or One Runtime Registry connect
to multi runtimes)?

Problem:
    -  Not familiar with Zato.io, need more time to research and development.
    -  Heavy, the Zato.io need so much dependence of Library, about 300Mb.
    -  Zato can only used in python 2.7, so can not Inherited from the 
    parent zato.server.service.Service Class in checkmate.
    (that mean we need to use socket to connect to Zato Service in 
    checkmate.Server or TimeoutManager.Server etc)


In my opinion, the second plan and third plan are very much alike.
The SS and WebServer-Client are the same as Zato Server and 
Zato Service-Caller.
And these two paln need to dev an Server to connect, and Encoder 
to explain.
I suggest Finish Server and Encoder part. Finish and test an simple SS.
Then connect to website. If everything is OK then move them to Zato.


## Module
To keep low coupling, the communication should be replaceable,
it can be a web server, zmq socket or event a sql, nosql reader/writer.

So the classes should be like that:
```
    +---| Item Server
    | S |
    ----+
```
``` python
    class Encoder():
        def encode(python_type):
            return process(python_type)

        def decode(command, args):
            func = explain(command, args)
            return func

    class Server():
        def initialize():
            encoder = Encoder()
            """"""

        def start():
            """"""
            loop:
                func = receive()
                send(func)

        def receive():
            return encoder.decode()

        def send():
            data = encoder.encode(func())
            connect.send(data)
```
We can have different Servers and different Encoder, so that support
different kind of Client.
