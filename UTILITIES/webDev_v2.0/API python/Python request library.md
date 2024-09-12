# Python Requests

(c) 2019, Joe James. MIT License.

Tutorial on using the [Requests](http://docs.python-requests.org/en/master/user/quickstart/) library to access HTTP requests, GET, POST, PUT, DELETE, HEAD, OPTIONS.  
This notebook also covers how to use the Python [JSON](https://docs.python.org/3/library/json.html) library to parse values out of a GET response.  
If you don't have the requests library installed you can run 'pip install requests' or some equivalent command for your system in the console window.
```python
import requests
import json

r = requests.get('https://api.github.com/events')
r = requests.post('https://httpbin.org/post', data = {'name':'Joe'})
r = requests.put('https://httpbin.org/put', data = {'name':'Joe'})
r = requests.delete('https://httpbin.org/delete')
r = requests.head('https://httpbin.org/get')
r = requests.options('https://httpbin.org/get')
```

### GET Requests - Passing Parameters in URLs

A URL that returns an HTTP response in JSON format is called an API endpoint.  
Here's an example, [https://httpbin.org/get](https://httpbin.org/get)

With GET requests we can add parameters onto the URL to retrieve specific data.  
We define the params as a dictionary, and add params=payload to the Request.  
The Requests library builds the whole URL for us.
```python
payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.get('https://httpbin.org/get', params=payload)
print(r.url)
```
https://httpbin.org/get?key1=value1&key2=value2

**Passing a List as a parameter**  
Still use key:value pairs, with the list as the value.  
You can see here all the different attributes included in an HTTP Request response.
```python
payload = {'key1': 'value1', 'key2': ['value2', 'value3']}
r = requests.get('https://httpbin.org/get', params=payload)

print('URL: ', r.url)
print('ENCODING: ', r.encoding)
print('STATUS_CODE: ', r.status_code)
print('HEADERS: ', r.headers)
print('TEXT: ', r.text)
print('CONTENT: ', r.content)
print('JSON: ', r.json)
```

### POST Requests

We can add parameters to a POST request in Dictionary format, but we use data=payload.  
POST requests are used to upload new records to the server.  
POST would typically be used to get data from a web form and submit it to the server.

```python
r = requests.post('https://httpbin.org/post', data = {'name':'Joe'})

payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.post("https://httpbin.org/post", data=payload)
print(r.text)
```

### Using Requests to GET Currency Exchange Data

Here's a handy endpoint where we can GET foreign currency exchange rates in JSON format, [https://api.exchangeratesapi.io/latest](https://api.exchangeratesapi.io/latest)
```python
url = 'https://api.exchangeratesapi.io/latest'
r = requests.get(url)
print(r.text)
```
**It looks like the default is base:EUR, but we want exchange rates for USD, so we can pass in a parameter for base.  
We can also put in any date we want.**
```python
url = 'https://api.exchangeratesapi.io/2018-01-15'
r = requests.get(url, params={'base':'USD'})
print(r.text)
```

### Decoding JSON data

Now we have the rates in JSON format. We need to convert that to usable data. The JSON library basically has two functions:

-   json.loads( ) converts a text string into Python dict/list objects.
-   json.dumps( ) converts dict/list objects into a string.

We need to decode the JSON data into a dictionary, then get the rate for GBP, convert it to a float, and do a conversion.
```python
rates_json = json.loads(r.text)['rates']
print(rates_json)
print(rates_json['GBP'])
gbp = float(rates_json['GBP'])
print('200USD = ', str(gbp * 200), 'GBP')
```

### Using Requests to GET Song Data

Every API has documentation on how to use it.  
You can find the docs for this Song Data API [here.](https://documenter.getpostman.com/view/3719697/RzfarXB4)

```python
url = 'https://musicdemons.com/api/v1/artist'
r = requests.get(url)
print(r.text[:700])

url = 'https://musicdemons.com/api/v1/artist/21'
r = requests.get(url)
print(r.text)

url = 'https://musicdemons.com/api/v1/artist/21'
headers = {'with': 'songs,members'}
r = requests.get(url, headers=headers)
print(r.text[:700])
```
>{"id":12,"name":"Beatles","year_started":1960,"year_quit":1970,"text":"Beatles"},{"id":14,"name":"Dario G","year_started":1997,"year_quit":null,"text":"Dario G"},{"id":16,"name":"Fleetwood Mac","year_started":1967,"year_quit":null,"text":"Fleetwood Mac"},{"id":17,"name":"Blink 182","year_started":1992,"year_quit":null,"text":"Blink 182"},{"id":18,"name":"Bloc Party","year_started":2002,"year_quit":null,"text":"Bloc Party"},{"id":19,"name":"The Temper Trap","year_started":2005,"year_quit":null,"text":"The Temper Trap"},{"id":20,"name":"MGMT","year_started":2002,"year_quit":null,"text":"MGMT"},{"id":21,"name":"Coldplay","year_started":1996,"year_quit":null,"text":"Coldplay"},{"id":22,"name":"

```python
url = 'https://musicdemons.com/api/v1/artist/21'
r = requests.get(url)
print(r.text)
```
>{"id":21,"name":"Coldplay","year_started":1996,"year_quit":null,"text":"Coldplay"}

```python
url = 'https://musicdemons.com/api/v1/artist/21'
headers = {'with': 'songs,members'}
r = requests.get(url, headers=headers)
print(r.text[:700])
```


### Tips on breaking down JSON

To get data out of a JSON object, which is a combination of lists and dictionaries,  
just remember for lists you need a numerical index, and for key-value pairs you need a text index.  
So if the object looks like this, {"cars":["id":1,"model":"Camry"... you can access the model of the first car with text['cars'][0]['model']

```python
import json
text_json = json.loads(r.text)
print(text_json['name'])
for song in text_json['songs']:
    print(song['title'])
```
