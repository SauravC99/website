---
title: go-dig
description: "Implementation of Ruby's hash and array dig methods in GoLang."
publishDate: October 14 2023
tags: ["go", "GoLang", "dig", "ruby"]
draft: false
---

## Introduction

[![Go Reference](https://pkg.go.dev/badge/github.com/sauravc99/go-dig.svg)](https://pkg.go.dev/github.com/sauravc99/go-dig)

I was working on creating a Go service that handled a lot of json data from vendors and needed an easier method of parsing it.

The commonly used method of handling json data in GoLang is to create a struct that contains the fields of the json and their types, and then to unmarshal the data into the struct. This method is completely fine, except for times where the vendor would change or update their json structure. I wanted a better way.

## Ruby

The Ruby programming language has some [dig methods](https://docs.ruby-lang.org/en/3.3/dig_methods_rdoc.html) which are very useful for accessing nested data. Instead of needing to create a struct with all of the fields, you can search through the data with a key or list of keys. Another benefit is that the dig methods will not raise an exception or error in cases where an item is not found; returning `nil` instead. I wanted to see if I could create something similar for Go.

## go-dig

I wanted to implement Ruby's dig methods in Go, complete with the ability to dig through nested hashes and arrays and proper error handling. I did that and also added more features like descriptive error messages. go-dig is easy to use and fast. The package is available for anyone to use.

## More

Check out the code and documentation with examples on my [repo](https://github.com/SauravC99/go-dig).

Check out the go package reference page:
[![Go Reference](https://pkg.go.dev/badge/github.com/sauravc99/go-dig.svg)](https://pkg.go.dev/github.com/sauravc99/go-dig)
