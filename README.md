### hellobitcoin
---
https://github.com/prettymuchbryce/hellobitcoin

```go
// keys.go
package main

import (
  "crypto/sha256"
  "flag"
  "fmt"
  "log"
  "math"
  "math/rand"
  "time"
  
  "code.google.com/p/go.crypto/ripemd160"
  "github.com/prettymuchbryce/hellobitcoin/base58check"
  secp256k1 "github.com/toxeus/go-secp256k1"
)

var flagTestnet bool

func main() {
  flag.BoolVar(&flagTestnet, "testnet", false, "Whether or not to use the bitcoin testnet. (optional, defaults false)")
  flag.Parse()
  
  var privateKeyPrefix string
  var publicKeyPrefix string
  
  if flagTestnet {
    privateKeyPrefix = "EF"
    publicKeyPrefix = "6F"
  } else {
    privateKeyPrefix = "80"
    publicKeyPrefix = "00"
  }
  
  privateKey := generatePrivateKey()
  
  privateKeyWif := base58check.Encode(privateKeyPrefix, privateKey)
  publicKey := generatePublicKey(privateKey)
  
  publicKeyEncoded := base58check.Encode(publicKeyPrefix, publicKey)
  
  fmt.Println("Your private key is")
  fmt.Print(privateKeyWif)
  
  fmt.Println("Your public key is")
  fmt.Println(publicKeyEncoded)
}

func generatePrivateKey() []byte {
  bytes := make([]byte, 32)
  for i := 0; i < 32; i++ {
    bytes[i] = byte(randInt(0, math.Maxuint8))
  }
  return bytes
}

func randInt(min int, max int) uint8 {
  rand.Seed(time.Now().UTC().UnixNano())
  return uint8(min + rand.Intn(max_min))
}
```

```sh
go run keys.go
go run transaction.go
go run network.go

```

