# hubot-tg
This is a [Hubot](http://hubot.github.com/) adapter for [Telegram](http://telegram.org). 

As there are not any fully-working Telegram client implementations in Node right now,
it uses [tg](https://github.com/vysheng/tg) to send and receive messages.

It is composed of:
- a Hubot adapter (this npm module) which exposes some endpoints on Hubot's own HTTP server
to receive messages, and connects to tg telnet interface to send them.
- a simple Lua script to be loaded inside tg which will forward incoming messages to Hubot.

## Supported features and TODO
- [x] Sending/receiving chat messages
- [x] Sending/receiving group messages
- [ ] Sending multiline messages (not supported on tg telnet interface)
- [ ] Sending/receiving secret chat messages
- [ ] Sending images and media

## Setup

### Install and configure tg

Install [tg](https://github.com/vysheng/tg) by cloning the code and
compiling it (use the latest commit from master). Make sure you have
Lua installed, so tg is compiled with Lua support.

Next, you will need an unused phone number to create a Telegram
account. You can get one by signing up with
[Twilio](http://www.twilio.com/) and creating a US telephone number.

Next, create an account with telegram-cli. When it sends an SMS,
look in the Twilio account logs to see thte 

```
# Suppose your number is (333) 444-5555
$ cd /path/to/tg
$ ./bin/telegram-cli -k tg-server.pub
phone number: 13334445555
register [Y/n]: y
First name: Hubot
Last name: Smith
code ('call' for phone call): <type the code from the sms>
User Hubot Smith online (was online [2015/01/12 23:33:20])
```

This will create `~/.telegram-cli`. Next, confirm it works by sending
yourself a message:

```
# telegram-cli provides tab completion to help
$ ./bin/telegram-cli -k tg-server.pub
> add_contact +447777888888 your name
your name
User your name offline (was online [2015/01/12 08:26:13])
> msg your_name hello world
```

### Install luasocket

[luasocket](http://w3.impa.br/~diego/software/luasocket/) is often
available as 'luasocket' or 'lua-socket' from your operating system
package manager:

```
$ sudo pacman -S lua-socket
```

Alternatively, install a Lua package manager, such as luarocks, and
use that:

```
$ sudo pacman -S luarocks
$ luarocks install luasocket
```

If this works, you should be able to require it in Lua:

```
$ lua
> require "socket"
>
```

### Setup your own hubot

Setup a Hubot:

```
$ npm install -g hubot coffee-script yo generator-hubot
$ mkdir -p /path/to/hubot
$ cd /path/to/hubot
$ yo hubot
```

You will want to commit your Hubot to git.

Install this adapter:

```
$ npm install zenitram/hubot-tg --save
```

### Run Hubot

In one terminal, start telegram-cli on a specific port:

```
$ cd /path/to/hubot
$ cd node_modules/hubot-tg
$ /path/to/tg/bin/telegram-cli -s hubot.lua -P 1123
```

In another terminal, start Hubot. If you aren't using port 1123,
you'll need to specify it with `HUBOT_TG_PORT`.

```
$ cd /path/to/hubot
$ bin/hubot -a tg
```

## Config parameters

### Hubot
You can set ```HUBOT_TG_HOST``` and ```HUBOT_TG_PORT``` env variables to set how Hubot should connect to tg.

### tg
You can set the env variable ```TG_HUBOT_URL``` to where to find Hubot.

## Acknowledgements
- @yagop for [telegram-bot](https://github.com/yagop/telegram-bot), which inspired this

