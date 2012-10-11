
## yubigo

The Yubigo authentication Go package provides an easy way to integrate the Yubico Yubikey into your existing Go-based user authentication infrastructure. 

### Status and Roadmap

This package is very young and lacks automated tests and documentation. Development is high.

At this moment there is **no guarantee** regarding the stability of this package. Although this package is already being used in a production environment.
Everything is subject to change.

Development is co-ordinated via this github repository. If you made an improvement, please request for pull!

This project is licensed under a Simplified BSD license. Please read the LICENSE file.

### Installation

Installation is simple. Use go get:
`go get github.com/GeertJohan/yubigo`

### Usage

Make sure to import the package: `import "github.com/GeertJohan/yubigo"`

Basic OTP checking usage:
```go

// create a new yubiAuth instance with id and key
yubiAuth, err := yubigo.NewYubiAuth("1234", "fdsaffqaf4vrc2q3cds=")
if err != nil {
	// probably an invalid key was given
	log.Fatalln(err)
}

// verify an OTP value
result, ok, err := yubiAuth.Verify("ccccccbetgjevivbklihljgtbenbfrefccveiglnjfbc")
if err != nil {
	log.Fatalln(err)
}

if ok {
	// succes!! The OTP is valid!
	// lets get some data from the result
	sessioncounter := result.GetParameter("sessioncounter")
	log.Printf("This was the  %sth time the Yubikey was pluggin into a computer.\n", sessioncounter)
} else {
	// fail! The OTP is invalid or has been used before.
	log.Println("The given OTP is invalid!!!")
}
```

Do not verify HTTPS certificate:
```go
// Disable HTTPS cert verification. Use true to enable again.
yubiAuth.VerifyHttps(false)
```

HTTP instead of HTTPS:
```go
// Disable HTTPS. Use true to enable again.
yubiAuth.UseHttps(false)
```

Custom API server:
```go
// Set a list of n servers, each server as host + path. 
// Do not prepend with protocol
yubiAuth.SetApiServerList("api0.server.com/api/verify", "api1.server.com/api/verify", "otherserver.com/api/verify")
```

### Validation Protocol

The goal of this package is to implement a pure-Go Yubico OTP Validation Client which is following the Validation Protocol Version 2.0 as specified here: http://code.google.com/p/yubikey-val-server-php/wiki/ValidationProtocolV20