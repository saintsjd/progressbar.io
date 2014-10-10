progressbar.io
==============

Simple centralized logging for distributed processes.

Basic idea:

1.  Workers submit their status via HTTP POST to progressbar.io
2.  Curious observers poll progressbar.io via HTTP GET to see how things are going. (Webhooks will be available someday soon)

This helps keep workers working and simple. 

To start tracking something just describe it
----

```
# use plain text
curl --data "description=Making+Pizza+for+Order+111" http://progressbar.io/bars

# or use JSON
curl -H "Content-Type: application/json" -d '{"description":"Making Pizza for Order 111"}' http://progressbar.io/bars
```

You will get back a repsponse with a secret key to identify the 
```
```








