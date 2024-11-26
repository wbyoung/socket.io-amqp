# socket.io-amqp

A Socket.IO Adapter for use with RabbitMQ and other AMQP services.

[![Build Status](https://travis-ci.org/sensibill/socket.io-amqp.svg)](https://travis-ci.org/sensibill/socket.io-amqp)


[![NPM version](https://badge.fury.io/js/socket.io-amqp.svg)](http://badge.fury.io/js/socket.io-amqp)

## How to use

```js
var io = require('socket.io')(3000);
var amqp_adapter = require('socket.io-amqp');
io.adapter(amqp_adapter('amqp://localhost'));
```
## API

### adapter(uri[, opts], [onNamespaceInitializedCallback])

`uri` is a string like `amqp://localhost` which points to your AMQP / RabbitMQ server.
The amqp:// scheme is MANDATORY. If you need to use a username & password, they must
be embedded in the URI.

The following options are allowed:

- `prefix`: A prefix that will be applied to all queues, exchanges and messages created by socket.io-amqp.

- `queueName`: The name of the rabbitmq queue to use listen in on the exchange. Must be unique. Default value is '' which means rabbitmq will auto generate a queue name for you that is unique.

- `channelSeperator`: The delimiter between the prefix, the namespace name, and the room. The default is '#' for consistency with socket.io-emitter and backwards compatibility with older versions of `socket.io-amqp`. If you are using the library for the first time, **you should change it because # is a wildcard character in rabbitmq which means you may get cross chatter with other rooms**. If you have used the library previously and do not interact with the messages publshed to the exchanges created by `socket.io-amqp` based on routing key, you should also change the separator.

- `onNamespaceInitializedCallback`: This is a callback function that is called everytime sockets.io opens a new namespace. Because a new namespace requires new queues and exchanges, you can get a callback to indicate the success or failure here. This callback should be in the form of function(err, nsp), where err is the error, and nsp is the namespace. If your code needs to wait until sockets.io is fully set up and ready to go, you can use this.

- `useInputExchange` option: This configures the use of 2 exchanges 
    `socket.io` and `socket.io-input` where `socket.io-input` is a fanout exchange
    and `socket.io` is bound to it.
    
- `amqpConnectionOptions`
