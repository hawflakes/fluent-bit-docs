# Forward

_Forward_ is the protocol used by [Fluent Bit](http://fluentbit.io) and [Fluentd](http://www.fluentd.org) to route messages between peers. This plugin implements the input service to listen for Forward messages.

## Configuration Parameters

The plugin supports the following configuration parameters:

| Key         | Description       |
| ------------|-------------------|
| Listen      | Listener network interface, default: 0.0.0.0. |
| Port        | TCP port where listening for connections, default: 24224. |
| Buffer\_Size| Specify the maximum buffer size in KB to receive a Forward message. If not set, the default size will be the value of _Chunk\_Size_. |
| Chunk_Size  | By default the buffer to store the incoming Forward messages, do not allocate the maximum memory allowed, instead it allocate memory when is required. The rounds of allocations are set by _Chunk\_Size_ in KB. If not set, _Chunk\_Size_ is equal to 32 (32KB). |

## Getting Started

In order to receive Forward messages, you can run the plugin from the command line or through the configuration file:

### Command Line

From the command line you can let Fluent Bit listen for _Forward_ messages with the following options:

```bash
$ fluent-bit -i forward -o stdout
```

By default the service will listen an all interfaces (0.0.0.0) through TCP port 24224, optionally you can change this directly, e.g:

```bash
$ fluent-bit -i forward://192.168.3.2:9090 -o stdout
```

In the example the Forward messages will only arrive through network interface under 192.168.3.2 address and TCP Port 9090.

### Configuration File

In your main configuration file append the following _Input_ & _Output_ sections:

```python
[INPUT]
    Name        forward
    Listen      0.0.0.0
    Port        24224
    Chunk_Size  32
    Buffer_Size 64

[OUTPUT]
    Name   stdout
    Match  *
```

## Testing

Once Fluent Bit is running, you can send some messages using the _fluent-cat_ tool (this tool is provided by [Fluentd](http://www.fluentd.org):

```bash
$ echo '{"key 1": 123456789, "key 2": "abcdefg"}' | fluent-cat my_tag
```

In [Fluent Bit](http://fluentbit.io) we should see the following output:

```bash
$ bin/fluent-bit -i forward -o stdout
Fluent-Bit v0.9.0
Copyright (C) Treasure Data

[2016/10/07 21:49:40] [ info] [engine] started
[2016/10/07 21:49:40] [ info] [in_fw] binding 0.0.0.0:24224
[0] my_tag: [1475898594, {"key 1"=>123456789, "key 2"=>"abcdefg"}]
```
