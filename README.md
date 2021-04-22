# 🔑 Paket – A vault to packaging and encrypt/decrypt your files in golang!

[![go.pkg.dev](https://pkg.go.dev/badge/github.com/SeanTolstoyevski/paket.svg)](https://pkg.go.dev/github.com/SeanTolstoyevski/paket) | [![Go Report Card](https://goreportcard.com/badge/github.com/SeanTolstoyevski/paket)](https://goreportcard.com/report/github.com/SeanTolstoyevski/paket)

## Table of Contents

* [🔊 Informations](#-informations)

* [👩‍🏭👨‍🏭 What does it do](#what-does-it-do)

* [🏎 Installation](#installation)

* [Usage of cmd tool](#usage-of-cmd-tool)

	* [Create a package file with cmd tool](#create-a-package-file-with-cmd-tool)

* [Examples](#examples)

* [😋 If you like this](#-if-you-like-this)

* [🤔 FAQ](#-faq)

## 🔊 Informations

Hey,  
this is not for archiving your files like 7-zip.

We recommend that you take a look at the items below before using this module.  
The world of encryption and encryption is a **complex  topic**. It is important to know what you are doing and what this module actually does.

* Is it really secure? How secure is it?

Frankly the person who wants to get the data can crack anything if tries. Especially if the program you are distributing runs directly on the user's computer and all data is with the program. However, what AES and Package do is complex enough.  
Don't Remember, **every executable file is sensitive to disassembly**.  
You can pass your files through other complex processes before encrypting them. However, this causes your program to load files into memory slowly at run time.

* What encryption algorithm does it use?

AES CFB.  
If enough people write to add new algorithms, we will add new algorithms to the extent that golang supports it.

## 👩‍🏭👨‍🏭 What does it do?

Imagine you are producing a game. You will probably have carefully designed animations and sound effects. You do not want users to receive this data.  
If we think for this scenario; The package encrypts the files in the specified folder using AES with a key you specify. And it combines all encrypted files into a single file. Calculates the hash of the encrypted and unencrypted version of the file. Saves to a table. This is a little shield for people trying to deceive you.  
Then, you can easily retrieve the decoded or encrypted version of your file from the encrypted file.  
Normally you should create a system to securely encrypt and decrypt your files.  
This is a ready system 😎 .

## 🏎 Installation

This module consists of two parts:
1. CMD tool – command-line tool for encrypting and packaging files.
2. "pengine" (paket engine) – subfolder that provides low-level APIs (reading encrypted datas, verifications etc...).

To use Paket, you need to create a package file with the cmd tool.

You can install it like a normal golang module:  
`go get -u github.com/SeanTolstoyevski/paket`

## Usage of CMD Tool

Let's run the `-help` command of the Paket to see if it has been installed successfully:

```cmd
cmd>paket -help
Usage of paket:
  -f string
        Folder containing files to be encrypted. It is not recursive, Subfolders is not encrypted.
  -k string
        Key for encrypting files. It must be 16, 24 or 32 lenght in bytes. If this parameter is null, the tool generates one randomly byte  and prints value to the console.
  -o string
        The file to which your encrypted data will be written. If there is a file with the same name, you will be warned. (default "data.pack")
  -s    prints progress steps to the console. For example, which file is currently encrypting, etc. (default true)
  -t string
        The go file to be written for Paket to read. When compiling this file, you must import it into your program.
        It is created as "package main." (default "PaketTable.go")
```

**Warning**: If the key is null, the system randomly generates a key. You must then save this key. Paket does not write the randomly generated key.

### Create a package file with cmd tool

You can create a package with something like:

`paket -f=mydatas -o=data.dat`

Example output:

```cmd
Your random key: 092f8e0b25b0eeea32037e716dfcf2bc
3 files were found in mydatas folder.
Comedy of Errors (complete text) - Shakespeare.txt file is encrypting. Size: 0.9117 MB
George Orwell - Animal Farm.pdf file is encrypting. Size: 5.5276 MB
openal_soft_readme.md file is encrypting. Size: 0.0288 MB
```

If you don't want the details, you can pass  `-s=0` parameter.  
so:  
`paket -f=mydatas -o=data.dat -s=0`

Next, a go file like this is created.  
This is the table that keeps the information of your files.

The generated go code would look like this.  

```go
// **Do not edit this file**. It is generated automatically and contains sensitive data.

package main

import (
	paket "github.com/SeanTolstoyevski/paket/pengine"
)

// The map vault for datas.
var PaketData = map[string]paket.Values{
	"a.dat" : {StartPos : 0, EndPos : 15466, OriginalLenght : 15450, EncryptLenght : 15466, HashOriginal : "aa13995a2c75649044c45b65259f8fb0e99afaf4f6996e4a321d0f7617bd8ac7", HashEncrypt : "37bf49c3230a93b5390d4eb92a776f74082560e6aa4c76b889ecc345c5380958"},
	"aa.dat" : {StartPos : 15466, EndPos : 21398, OriginalLenght : 5916, EncryptLenght : 5932, HashOriginal : "1cf8da96b434c21b37bac3ab6347b19ada55b7be54af74d89776280f66c61be8", HashEncrypt : "d3abcf36550f769e6626a4d917980c02cb7fe75d78a786635808c0d50877b1d8"},
	"ab.dat" : {StartPos : 21398, EndPos : 31649, OriginalLenght : 10235, EncryptLenght : 10251, HashOriginal : "a7ebe908dfc0d3436b95eed2b71a1a90809e6bb395d3daf401acbfc085505359", HashEncrypt : "2fc9fddf838583350fff7271a3a41a1ee6dbf556025726d122ddffa2ac7ff6bb"},
	"ab.dat" : {StartPos : 31649, EndPos : 44423, OriginalLenght : 12758, EncryptLenght : 12774, HashOriginal : "24d38f462aaec4d0083cddbd05ce157bcd66d0615bbe45f446e11d7145f1e497", HashEncrypt : "780b14cd5b6c539161c5c7c27d7ed288aad1c480ea4cf7969a358d5fb05dfff1"},
	"ac.dat" : {StartPos : 44423, EndPos : 50651, OriginalLenght : 6212, EncryptLenght : 6228, HashOriginal : "283ffa3770005e67ad033e936043205845e5febe0bc9b4094dac056dc11bda2d", HashEncrypt : "880bf77551e64b2cb71b51a99ede683097333f27965cfa3a6c208e8a60a760a4"},
	"b.dat" : {StartPos : 50651, EndPos : 61172, OriginalLenght : 10505, EncryptLenght : 10521, HashOriginal : "8a59e7cf5649e5aca1d91a2c4f63044deb6719874b6ece8354d104aeff675e28", HashEncrypt : "6fd98cd58625eb28c00497361a291f2ab3376dc1bcfc0f474e0fc052fc52945a"},
	"c.dat" : {StartPos : 61172, EndPos : 89810, OriginalLenght : 28622, EncryptLenght : 28638, HashOriginal : "0e66ad90ccc43b9e5cbd06a351f5194d49ff3bb9abf014b2eeaf75c9ea6c0ed0", HashEncrypt : "815805956135c8bf8d6e68f0be2f8fa1f3a9cb0aab5ff7f2c4618da64ee5d4d2"},
}
```

**Great**, we created our first package. We're going to write some code now.  

## Examples

If you want you can examine the codes in the [examples folder](https://github.com/SeanTolstoyevski/paket/examples).


## 😋 If you like this

* 📝🖊 Please consider creating a PR or emailing me for grammatical errors and other language issues in documents. English is **not my native language**.
 - And we have a few not fixed issues right now.  [🤔 Any ideas or code for these](https://github.com/SeanTolstoyevski/paket/blob/master/developing_and_contribute.md).

* If you can test for Linux and Darwin, that would be a great  for me. I am a blind software developer. I cannot set up an environment in Linux that can develop and test these projects. Linux's accessibility is not as good as Windows. No mac. I'll try to test as much as possible with the built-in `go test` though.

* 💰🤑 If you don't want to do any of them and want to give financial support (like a cup of french press), you can send an e-mail

* 🌟⭐ And send a star. This is the greatest favor. I'm looking for a job. Employers are not so optimistic in Turkey against disabled users. But any good project with a few stars can win over employers' hearts.

## 🤔 FAQ

* Why name **Paket**?

> Package in English, "paket" in Turkish.  
I was looking for a module where I could package my data. This name came first to mind.

* Which Go versions are compatible?

> Tested with Go 15.3 64 bit on windows 10 64 bit.  
Probably compatible up to go1.12-13.
