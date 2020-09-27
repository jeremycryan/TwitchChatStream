# TwitchChatStream
Stream incoming chat messages from a Twitch channel over IRC.

This tool uses an anonymous username and no authentication token, so it is only capable of reading messages, not sending them. Useful for basic Twitch integration for game development or streaming tools.

## Example usage

The following script will continuously print chat messages from the specified channel.

```
from twitch_chat_stream import Stream

# replace channel_name with your Twitch username
my_stream = Stream("channel_name")
while True:
    for message in my_stream.queue_flush():
        print(f"{message.user}: {message.text}")
```

## Stream object

### Stream.__init__(channel_name=None)
You can initialize the `Stream` object with no arguments or with a channel name. If you initialize it with no arguments, you must call `Stream.open` with a channel name before it can begin to stream incoming chat messages.

### Stream.open(channel_name)
Begins to listen to the channel's chat over IRC. Starts a new thread that continuously populates an internal queue with new messages.

### Stream.queue_flush()
Returns all messages in the queue, as a tuple, in the order they were sent. Clears the queue.

Messages returned in this way are of type Stream.Message, and have three attributes:
- `Stream.Message.user`, the username of the sender,
- `Stream.Message.text`, the body of the message, and
- `Stream.Message.creation_time`, the time the Message object was created, received using `time.time()`.

### Stream.queue.close()
Closes the IRC connection and deletes any remaining Messages in the queue.
