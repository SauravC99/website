---
title: Go HTTP Server
description: "Working HTTP Server written in GoLang."
publishDate: April 26 2024
tags: ["go", "GoLang", "server", "HTTP"]
draft: false
---

## go-http-server

go-http-server is a HTTP server written in GoLang. It is able to handle multiple requests concurrently using [Goroutines](https://go.dev/tour/concurrency/1). Data is exchanged with the server through GET and POST requests.

I have a 3D printer that currently lives in my garage. The location is fine for now, but having to go back and forth from my computer to the printer is getting old. One of my motivations for this project is to connect a raspberry pi or similar type of SBC to my 3D printer and to be able to start and monitor prints from my desk. I thought it would be an interesting project to create the server and client software myself and a great learning opportunity too!

Check it out on my [repo](https://github.com/SauravC99/go-http-server)!

## Installation

Clone the repository to your computer:

```bash
git clone https://github.com/SauravC99/go-http-server.git
```

Compile the program:

```bash
go build app/server.go
```

### Alternate Installation

You can clone the repository using the instructions above and run the `server.sh` script. The script will compile the program and run it.

## Usage

Run the server:

```shell
$ ./server
Server started
Listening on port 4221
```

Run with `-h` to see avaliable commands:

```bash
$ ./server -h
Usage of ./server:
  -directory string
        Directory for file hosting (download and upload)
  -port string
        Port to bind to (default "4221")
```
