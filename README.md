# ChatGPT IRC Bot
__ChatGPT IRC Bot__ is a simple IRC bot written in Python. It was initially forked from the [knrd1/chatgpt](https://github.com/knrd1/chatgpt) but turned into its own project. It connects to __OpenAI__ endpoints to answer questions or generate images and uses official bindings from __OpenAI__ to interact with the API through the HTTP requests. You can find more details about API [here](https://platform.openai.com/docs/api-reference).

## Prerequisites
1. Create an account and obtain your API key: https://platform.openai.com/account/api-keys
2. Install Python3 and the official Python bindings (__openai__: 0.28.0, __0.28.1__; __pyshorteners__)
   > Note that only version __0.28.x__ of __openai__ is supported, and the latest version is __0.28.1__.
   * Debian/Ubuntu
     ```
     apt install python3 python3-pip
     pip3 install openai==0.28.1 pyshorteners
     ```
   * RedHat/CentOS
     ```
     yum install python3 python3-pip
     pip3 install openai==0.28.1 pyshorteners
     ```
   * FreeBSD
     ```
     pkg install python311 py311-pip
     pip install openai==0.28.1 pyshorteners
     ```

## Installation
Clone the package using the below command. It will copy files into the __ChatGPT-IRC-Bot__ directory, which you can later rename.
```
git clone https://github.com/oiramNet/ChatGPT-IRC-Bot.git
```

## Configuration
ChatGPT IRC Bot uses a plaintext file as its configuration file. The package includes an example configuration file (__chat.conf.sample__) set to connect to IRCnet. You can copy and modify it.
```
cd ChatGPT-IRC-Bot
cp chat.conf.sample chat.conf
```
> Variable __context__ is optional, you can leave it blank or enter what you want the bot to know and how you want it to behave. This will only work with the models connecting to the endpoint __/v1/chat/completions__ (see below).

```
[openai]
api_key = sk-XXXXXXXXXXXXXXX

[chatcompletion]
model = gpt-4o-mini
role = user
context = 
temperature = 0.8
max_tokens = 1000
top_p = 1
frequency_penalty = 0
presence_penalty = 0
request_timeout = 60
context = You are a professor.
prompt_prefix = Precise answers only, nothing additional. Provide answers in the form of only 255 characters per line to avoid multiple line outputs as much as possible. If a list is requested, provide it in the form of 255 characters, or in the form of a conversation to avoid multiple line outputs.

[irc]
server = open.ircnet.net
port = 6667
ssl = false
channels = #oiram
nickname = SampleBot
ident = samplebot
realname = Sample Bot
password = 
```

### Model endpoint compatibility
ChatGPT IRC Bot can use one of three API models:
* Models that support endpoint __/v1/completions__ (Legacy)
  > gpt-3.5-turbo-instruct, babbage-002, davinci-002
* Models that support endpoint __/v1/chat/completions__
  > __gpt-4o-mini__, gpt-4o, gpt-4, gpt-4-turbo, gpt-4-turbo-preview, gpt-3.5-turbo
* Models that support the creation of an image using endpoint __/v1/images/generations__
  > dall-e-2, dall-e-3

We suggest starting experimenting with __gpt-4o-mini__ model. More details about models can be found [here](https://platform.openai.com/docs/models).

## Running bot
To start the bot, you can just run the command below.
* Debian/Ubuntu/RedHat/CentOS
  ```
  python3 chatgpt.py chat.conf
  ```
* FreeBSD
  ```
  python3.11 chatgpt.py chat.conf
  ```

You can use the __screen__ command to run it in the background and keep it running even after you log out of your session.
* Debian/Ubuntu/RedHat/CentOS
  ```
  screen python3 chatgpt.py chat.conf
  ```
* FreeBSD
  ```
  screen python3.11 chatgpt.py chat.conf
  ```

To detach from the __screen__ session (leaving your ChatGPT IRC Bot running in the background), press Ctrl + A followed by d (for "detach").
If you need to reattach to the screen session later, use the following command:
```
screen -r
```

## Interaction
ChatGPT IRC Bot will interact only if you direct your question using its nickname.
```
12:34:12 < user> SampleBot: how are you?
12:34:13 < SampleBot> I'm just a computer program, so I don't have feelings, but I'm here and ready to help you! How can I assist you today?
12:56:21 < user> SampleBot: do you like IRC?
12:56:22 < SampleBot> As an AI, I don't have personal preferences or feelings, but I can provide information about IRC! Internet Relay Chat (IRC) is a popular protocol for real-time communication and has been used for
decades. Many people appreciate it for its simplicity and the sense of community it fosters. If you have specific questions or topics about IRC, feel free to ask!
```

If you set the model to __dall-e-2__ or __dall-e-3__, the ChatGPT IRC Bot will return a shortened URL to the generated image.
```
13:14:05 < user> SampleBot: red apples on the table
13:14:35 < SampleBot> https://tinyurl.com/1a2b3c4d
```

## Docker
To build the Docker image, you can use the following command:
```
docker build -t my-chatgpt-app .
```
To run the Docker container, you can use the following command:
```
docker run -it my-chatgpt-app
```
To detach from a running Docker, press Ctrl + P. While holding down Ctrl, press Q.
To reattach to the container later, use the following command:
```
docker attach <container_id>
```
