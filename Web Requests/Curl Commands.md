

|**Command**|**Description**|
|---|---|
|`curl -h`|cURL help menu|
|`curl inlanefreight.com`|Basic GET request|
|`curl -s -O inlanefreight.com/index.html`|Download file|
|`curl -k https://inlanefreight.com`|Skip HTTPS (SSL) certificate validation|
|`curl inlanefreight.com -v`|Print full HTTP request/response details|
|`curl -I https://www.inlanefreight.com`|Send HEAD request (only prints response headers)|
|`curl -i https://www.inlanefreight.com`|Print response headers and response body|
|`curl https://www.inlanefreight.com -A 'Mozilla/5.0'`|Set User-Agent header|
|`curl -u admin:admin http://<SERVER_IP>:<PORT>/`|Set HTTP basic authorization credentials|
|`curl http://admin:admin@<SERVER_IP>:<PORT>/`|Pass HTTP basic authorization credentials in the URL|
|`curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/`|Set request header|
|`curl 'http://<SERVER_IP>:<PORT>/search.php?search=le'`|Pass GET parameters|
|`curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/`|Send POST request with POST data|
|`curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/`|Set request cookies|
|`curl -X POST -d '{"search":"london"}' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php`|Send POST request with JSON data|






```
curl inlanefreight.com
```

```
curl -O inlanefreight.com/index.html
```

```
curl -s -O inlanefreight.com/index.html
```

```
curl https://inlanefreight.com
```

To skip the certificate check with cURL, we can use the `-k` flag:

```
curl -k https://inlanefreight.com
```

To view the full HTTP request and response, we can simply add the `-v` verbose flag to our earlier commands

```
curl inlanefreight.com -v
```

The difference between the two is that `-I` sends a `HEAD` request (as will see in the next section), while `-i` sends any request we specify and prints the headers as well.

```
curl -I https://www.inlanefreight.com
```

We can use the `-A` to set our `User-Agent`, as follows:

```
curl https://www.inlanefreight.com -A 'Mozilla/5.0'
```

```
curl -i http://<SERVER_IP>:<PORT>/
```

```
curl -u admin:admin http://<SERVER_IP>:<PORT>/
```

```
curl http://admin:admin@<SERVER_IP>:<PORT>/
```


We can set the header with the `-H` flag, and will use the same value from the above HTTP request. We can add the `-H` flag multiple times to specify multiple headers:

```
curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/
```

We will use the `-X POST` flag to send a `POST` request. Then, to add our POST data, we can use the `-d` flag and add the above data after it, as follows:

```
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/
```

We can use the `-v` or `-i` flags to view the response

```
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/ -i
```

```
curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```

```
curl -H 'Cookie: PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```

```
curl -X POST -d '{"search":"london"}' -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php
["London (UK)"]
```

```
curl http://<SERVER_IP>:<PORT>/api.php/city/london
```

We see that the result is sent as a JSON string
```
curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq
```



## Create

To add a new entry, we can use an HTTP POST request, which is quite similar to what we have performed in the previous section. We can simply POST our JSON data, and it will be added to the table. As this API is using JSON data, we will also set the `Content-Type` header to JSON, as follows:

```
curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```


## Update

Now that we know how to read and write entries through APIs, let's start discussing two other HTTP methods we have not used so far: `PUT` and `DELETE`. As mentioned at the beginning of the section, `PUT` is used to update API entries and modify their details, while `DELETE` is used to remove a specific entity.


```
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```

```
curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq
```

## DELETE

Finally, let's try to delete a city, which is as easy as reading a city. We simply specify the city name for the API and use the HTTP `DELETE` method, and it would delete the entry, as follows:

```
curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City
```

```
curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq
```



|**Command**|**Description**|
|---|---|
|`curl http://<SERVER_IP>:<PORT>/api.php/city/london`|Read entry|
|`curl -s http://<SERVER_IP>:<PORT>/api.php/city/ \| jq`|Read all entries|
|`curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'`|Create (add) entry|
|`curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'`|Update (modify) entry|
|`curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City`|Delete entry|

```
curl -X POST -F "submit:<value>" -F "<file-parameter>:@<path-to-file>" <site>
```

#curl #post #get #api