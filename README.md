# telegram.sh

## Notes

This fork adapts the [original](https://github.com/fabianonline/telegram.sh) to work on a simple shell `sh`.


## What does it do?

Telegram offers the feature of bots. A bot allows automated systems and
servers to send telegram messages to users.
Quite often it can be useful to send stuff to yourself. A classic
application of this would be receiving results of cronjob tasks via email.
Or maybe you want to grab a small file from your server, but downloading it
via SCP would be too much work or wouldn't work at all because firewall
stuff / filters / proxy servers / whatever.

telegram.sh allows you to send such things via telegram.

## Examples

```bash
# Send a message to yourself, using a bot token and a chat_id.
telegram -t 123456:AbcDefGhi-JklMnoPrw -c 12345 -m "Hello, World."

# You can define the token and chat_id in environment variables or config files.
# Then you can just use
telegram -m "Hello, World."

# Split them into multiple lines
telegram -m "Hello,"$'\n'"World."
echo -e "Hello\nWorld." | telegram -

# Or you send this one message to another chat:
telegram -c 6789 -m "Hello, Mars."

# Send stuff via stdin as monospace code:
ls -l | telegram -C -

# Use markdown in your message (HTML is available as well):
telegram -M -m "To *boldly* go, where _no man_ has gone before."

# Send a local file.
telegram -f results.txt -m "Here are the results."

# Or an image, giving you a preview and stuff.
telegram -i solar_system.png

# See all options:
telegram -h

# Use environment variables to tell curl to use a proxy server:
HTTPS_PROXY="socks5://127.0.0.1:1234" telegram -m "Hello, World."
```

## Requirements

Only a basic shell `sh` and `curl`.

## Installation

Grab the latest `telegram` file from this repository and put it somewhere.

### Create a Telegram Bot

To create a new bot on Telegram:

  * Search for the user `@botfather` and start to chat
  * Create a new bot sending `/newbot`
  * Keep the given token (and keep it secret!)
  * Send any message to your new bot to activate it
  * Find your chat_id running: `telegram -t <token> -l`

You now have your token and your chat_id, try it:
```
  telegram -t <token> -c <chat_id> -m "Hello there!"
```

### Configuration

 You can define <token> and <chat_id> by:

  1. Command line options: -t <token> -c <chat_id>
  2. Environment variables: TELEGRAM_TOKEN and TELEGRAM_CHAT
  3. User-based config file: ~/.telegram-sh.conf
  4. Global config file: /etc/telegram-sh.conf

The earliest take precedence, so you can define general <token> in
`~/telegram-sh.conf` and then overwrite it using `-t` on command line.

Config file contents should be like:

```
TOKEN=123456:AbcDefGhi-JlkMno
CHAT_ID=12345678
```
Please be aware that you should keep your token secret.
