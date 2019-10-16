# Slowloris 

It is an application layer attack that uses partial HTTP requests to a web server 
and then it tries to keep them open as long as it can. This attack uses low 
bandwidth. It falls in the category of *low* and *slow* attacks.

The flow of the attack is the following:
1. The attacker opens multiple connection to the server by sending partial HTTP request headers.
2. The server receives and opens a thread to handle the requests. It will wait to the connection to succeed or timeout before freeing the thread to handle another
request.
3. To prevent the server to timing out the connections, the attacker sends 
partial request periodically to keep the connection alive.
4. Once all available threads in the server are in use, the server will be unable to respond to additional requests from regular traffic.

## Testing slowloris

To test this attack we will use the **slowhttptest** tool, for more information on how to install this tool, check the [http attacks tutorial](./HTTP_ATTACKS.md).

The **slowhttptest** cli allow us to use Slowloris with the -H option. We can set the server to attack with the -u option and setup the URL to attack. So our basic 
Slowloris attack will be `slowhttptest -H -u htttp://url.to.attack`.

Furthermore we can configure the attack with the following options:

- -c number, connections to be opened
- -i number, the interval between follow up data in seconds, per connection
- -l number, duration of the attack (how long it will keep attacking)
- -p number, seconds to wait for HTTP response on probe connection, to determine if the server is considered as inaccesible
- -t string, custom verb to use POST, GET...

- -v number, verbosity level of log 0-4
- -x number, max length of the follow up data in bytes

- -g generate statistics in CSV and HTML of the attack
- -o string, custom output file path and/or name of the generated statistics

- -f value of the Content-type header in the request
- -m value of the Accept header
