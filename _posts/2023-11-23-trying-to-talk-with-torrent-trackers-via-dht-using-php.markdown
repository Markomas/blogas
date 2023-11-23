---
title: Trying to talk with torrent trackers via DHT using PHP
date: 2023-11-23 21:14:00 Z
---

Yo, I have decided to implement an interesting thing:
Why not try to create a DHT client in PHP, just for fun? Maybe I would be able to scrap the DHT network with PHP?

So, first thing first: I have no idea how DHT works 😞

From a quick Google search, I see that there is no working example of this in PHP 😞 😞 😞

But I found this:

    import bencode
    import random
    import socket
    
    
    # Generate a 160-bit (20-byte) random node ID.
    my_id = ''.join([chr(random.randint(0, 255)) for _ in range(20)])
    
    # Create ping query and bencode it.
    # "'y': 'q'" is for "query".
    # "'t': '0f'" is a transaction ID which will be echoed in the response.
    # "'q': 'ping'" is a query type.
    # "'a': {'id': my_id}" is arguments. In this case there is only one argument -
    # our node ID.
    ping_query = {'y': 'q',
                  't': '0f',
                  'q': 'ping',
                  'a': {'id': my_id}}
    ping_query_bencoded = bencode.bencode(ping_query)
    
    # Send a datagram to a server and recieve a response.
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    s.sendto(ping_query_bencoded,
             (socket.gethostbyname('router.bittorrent.com'), 6881))
    r = s.recvfrom(1024)
    
    ping_response = bencode.bdecode(r[0])
    
    print(ping_response)

So, I should be able to implement this in PHP, should be fun:D

### Bencode
A quick search, and I got this:
```
composer require rych/bencode
```
Somebody would call that cheating, I call it: **progress** 😈

### Final ping result

```
<?php

use Rych\Bencode\Bencode;

include_once __DIR__ . '/vendor/autoload.php';

$socket=@fsockopen("udp://router.bittorrent.com",6881,$err_no,$err_str,1);
if(!$socket) die('socket failed');

$myId = generateId();
print 'ping'.PHP_EOL;
fwrite($socket, Bencode::encode([
    'y' => 'q',
    't' => '0f',
    'q' => 'ping',
    'a' => [
        'id' => $myId
    ]]));
$packetReceived=fread($socket,1024);
var_dump(Bencode::decode($packetReceived));
```