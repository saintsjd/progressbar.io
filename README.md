progressbar.io
==============

Simple centralized logging for distributed processes.

Basic idea:

1.  Workers submit their status via HTTP POST to progressbar.io
2.  Curious observers poll progressbar.io via HTTP GET to see how things are going. (Webhooks will be available someday soon)


To start tracking something, create a bar:
----

```
# use plain text
curl -X POST --data "name=Making+Pizza+for+Order+111" http://progressbar.io/bars

# or use JSON
curl -X POST -H "Content-Type: application/json" -d '{"name":"Making Pizza for Order 111"}' http://progressbar.io/bars
```

You will get back a response with an id and a secret key. The secret key can be used to write to the bar or delete it:
```
HTTP 201 CREATED
{
  id: "428f52f8-bd78-4c37-9071-10ebb84ccb2d",
  timestamp: "2014-10-12T21:22:13",
  secretKey: "36ec244b-731d-46c3-956a-7438d42a7798",
  name: "Making Pizza for Order 111"
}

```

Log some events to the bar
----

Make sure to use the secretKey in the HTTP auth header and the bar id in the URL.

```
# use plain text
curl -X POST -u 36ec244b-731d-46c3-956a-7438d42a7798: --data "message=Rolling+the+dough" http://progressbar.io/bars/428f52f8-bd78-4c37-9071-10ebb84ccb2d

# or use JSON
curl -X POST -u 36ec244b-731d-46c3-956a-7438d42a7798: -H "Content-Type: application/json" -d '{"message":"Adding Sauce"}' http://progressbar.io/bars/428f52f8-bd78-4c37-9071-10ebb84ccb2d

curl -X POST -u 36ec244b-731d-46c3-956a-7438d42a7798: -H "Content-Type: application/json" -d '{"message":"Spreading Toppings"}' http://progressbar.io/bars/428f52f8-bd78-4c37-9071-10ebb84ccb2d
```

Check the status of a bar
---- 

No secretKey needed for reading from the bar

```
curl -X GET -H "Content-Type: application/json" http://progressbar.io/bars/428f52f8-bd78-4c37-9071-10ebb84ccb2d/status
```

Returns the last message logged to the bar

```
HTTP 200 OK
{
  id: "02ebf474-e957-4337-9083-d6f1d57c29df",
  timestamp: "2014-10-12T21:40:32",
  message: "Spreading Toppings"
}
```



Check the full history of a bar
---- 

No secretKey needed for reading from the bar

```
curl -X GET -H "Content-Type: application/json" http://progressbar.io/bars/428f52f8-bd78-4c37-9071-10ebb84ccb2d/logs
```

Returns the last 100 messages logged to the bar in DESC order

```
HTTP 200 OK
[
  {
    id: "02ebf474-e957-4337-9083-d6f1d57c29df",
    timestamp: "2014-10-12T21:40:32",
    message: "Spreading Toppings"
  },
  {
    id: "4c4d031e-de41-4f52-8258-74e39362d258",
    timestamp: "2014-10-12T21:30:33",
    message: "Adding Sauce"
  },
  {
    id: "67e6698e-132a-49ec-9bc4-bb7253c6a70a",
    timestamp: "2014-10-12T21:20:22",
    message: "Tossing Dough"
  }
]
```



