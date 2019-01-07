## Socket Server Node JS

```Node JS
// Import net module.
var net = require('net');

// Create and return a net.Server object, the function will be invoked when client connect to this server.
var server = net.createServer(function(client) {

    console.log('Client connect. Client local address : ' + client.localAddress + ':' + client.localPort + '. client remote address : ' + client.remoteAddress + ':' + clien$

    client.setEncoding('utf-8');

    //client.setTimeout(1000);

    // When receive client data.
    client.on('data', function (data) {

        // Print received client data and length.
        console.log('Receive client send data : ' + data + ', data size : ' + client.bytesRead);

        // Server send data back to client use client net.Socket object.
        client.end('Server received data : ' + data + ', send back to client data size : ' + client.bytesWritten);
    //  client.write('Server received data : ' + data + ', send back to client data size : ' + client.bytesWritten);


        });

    // When client send data complete.
    client.on('end', function () {
        console.log('Client disconnect.');

        // Get current connections count.
        server.getConnections(function (err, count) {
            if(!err)
            {
                // Print current connection count in server console.
                console.log("There are %d connections now. ", count);
            }else
            {
                console.error(JSON.stringify(err));
            }

        });
    });

    // When client timeout.
    client.on('timeout', function () {
        console.log('Client request time out. ');
    })
});

// Make the server a TCP server listening on port 9999.
server.listen(9999, function () {

    // Get server address info.
    var serverInfo = server.address();

    var serverInfoJson = JSON.stringify(serverInfo);

    console.log('TCP server listen on address : ' + serverInfoJson);

    server.on('close', function () {
        console.log('TCP server socket is closed.');
    });

    server.on('error', function (error) {
        console.error(JSON.stringify(error));
    });

});

```

## Client Socket - Node Js [Test]
```Node JS
var net = require('net');
var client = new net.Socket();
client.connect(5000, '192.168.22.1', function() {
        console.log('Connected');
        client.write('Hello, server! Love, Client.');
});
var i = 0;
client.on('data', function(data) {
        console.log('Received: ' + data);
        i++;
        if(i==2)
                client.destroy(); 
});
client.on('close', function() {
        console.log('Connection closed');
});

client.on('error', function(error){
        console.log('error: ' + error);
});
```

