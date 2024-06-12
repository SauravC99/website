---
title: Go HTTP Server
description: "Working HTTP Server written in GoLang."
publishDate: April 26 2024
tags: ["go", "GoLang", "server", "HTTP"]
draft: false
updatedDate: June 12 2024
---

## go-http-server

go-http-server is a HTTP server written in GoLang. It is able to handle multiple requests concurrently using [Goroutines](https://go.dev/tour/concurrency/1). Data is exchanged with the server through GET and POST requests.

I have a 3D printer that currently lives in my garage. The location is fine for now, but having to go back and forth from my computer to the printer is getting old. One of my motivations for this project is to connect a raspberry pi or similar type of SBC to my 3D printer and to be able to start and monitor prints from my desk. I thought it would be an interesting project to create the server and client software myself and a great learning opportunity too!

Check it out on my [repo](https://github.com/SauravC99/go-http-server)!

<details>
<summary><i>Update June 12 2024:</i></summary>

I have added support for compressing responses and handling the encoding header in requests. The compressing algorithm used is gzip and the correct encoding and header is included in response now. Also made sure the header parser is case-insensitive in case some clients send headers in lowercase.

```go title="server.go" ins={4-5, 16, 25, 34-48, 58-78} {7, 10, 13, 20, 23, 30, 52, 56, 80}
package main

import(
	"bytes"
	"compress/gzip"
	"flag"
	...
)

...

type RequestHeaders struct {
	...
	ContentType    string
	ContentLength  string
	AcceptEncoding string
	Body           string
}

...

func parseHeaders(connection net.Conn) (*RequestHeaders, error) {
	...
	contentLen := ""
	acceptEncoding := ""
	body := ""

	for _, line := range request {
		switch {
		...
		case strings.HasPrefix(strings.ToLower(line), "content-length:"):
			temp := strings.Split(line, " ")
			contentLen = temp[1]
		case strings.HasPrefix(strings.ToLower(line), "accept-encoding:"):
			temp := strings.Split(line, " ")
			// Handle multiple compression scheme values
			if len(temp) > 2 {
				encodings := temp[1:]
				for _, line := range encodings {
					line = strings.ReplaceAll(line, ",", "")
					// For the time being accept gzip compression
					if strings.ToLower(line) == "gzip" {
						acceptEncoding = "gzip"
					}
				}
			} else {
				acceptEncoding = temp[1]
			}
		}
	}

	...

}

...

func respond200Encode(connection net.Conn, content string, encode string) {
	//gzip encode
	var buffer bytes.Buffer
	writer := gzip.NewWriter(&buffer)
	writer.Write([]byte(content))
	writer.Close()
	encodedContent := buffer.String()

	response := STATUS_200_OK
	encoding := CONTENT_ENCODING + encode + END_HEADER_LINE
	cType := CONTENT_PLAIN
	length := CONTENT_LENGTH + strconv.Itoa(len(encodedContent)) + END_HEADER_BLOCK
	body := encodedContent

	response = response + encoding + cType + length + body
	_, err := connection.Write([]byte(response))
	if err != nil {
		fmt.Println("Error writing to connection: ", err.Error())
	}
	connection.Close()
}

...
```

</details>

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
