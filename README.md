# Telegram Sender Utils

A set of simple scripts for sending messages to Telegram.

The `telegram-sender` script attempts to send a message based on the settings defined in the configuration file.  
If sending fails and queue saving is enabled, the message is saved into a queue file.

The `telegram-sender-queue` script processes queued messages by retrying to send them.  
If sending fails again, the messages remain in the queue for future attempts.

Typically, `telegram-sender-queue` is scheduled to run periodically via `cron`.  
See the sections below for configuration and usage examples.

---

## Configure

See the example configuration file:  
[`telegram-sender.conf.example`](./telegram-sender.conf.example)

You can copy it to your configuration directory and adjust settings as needed.

---

## Example of Use

Send messages from stdin and first param.

```bash
cat /proc/loadavg | telegram-sender
telegram-sender "Hello from $(hostname) CLI"
```

Cron job for send messages from all user queue

```
* * * * * find /var/tmp/ -maxdepth 1 -type f -name telegram-sender-queue\* -exec /usr/local/bin/telegram-sender-queue {} \;
```

Cron job for send messages from ordinary user queue

```
/usr/local/bin/telegram-sender-queue /var/tmp/telegram-sender-queue.myusername
```