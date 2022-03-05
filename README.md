# Library for tp-link HS100/HS110

Yet another tp-link HS100 library for golang


## Usage

Please make sure to have go-modules enabled.

Run within your go project folder: `go get -u github.com/johnnylin-a/golang-tplink-hs100`

### Usage example

use the following code as main and replace `YOUR_HS100_DEVICE` with the 
address of your HS100-device.

```golang
package main

import (
	"github.com/jaedle/golang-tplink-hs100/pkg/configuration"
	"github.com/jaedle/golang-tplink-hs100/pkg/hs100"
	"os"
)

func main() {
	h := hs100.NewHs100("YOUR_HS100_DEVICE", configuration.Default())

	name, err := h.GetName()
	if err != nil {
		println("Error on accessing device")
		os.Exit(1)
	}

	println("Name of device: " + name)
}
```

## Device discovery

It is possible to discover devices automatically.
Because this library uses tcp communication this requires to specify a subnet by CIDR notation.
For this example `192.168.2.0/24` all ips from `192.168.2.1` to `192.168.2.255` will be tried to reached.
By using `withTimeout(time.Duration)` a custom timeout can be specified instead of the default timeout.

*The discovery process will take at least the time of the default timeout.*

```golang
package main

import (
	"github.com/jaedle/golang-tplink-hs100/pkg/configuration"
	"github.com/jaedle/golang-tplink-hs100/pkg/hs100"
	"log"
	"time"
)

func main() {
	devices, err := hs100.Discover("192.168.2.0/24",
		configuration.Default().WithTimeout(time.Second),
	)

	if err != nil {
		panic(err)
	}

	log.Printf("Found devices: %d", len(devices))
	for _, d := range devices {
		name, _ := d.GetName()
		log.Printf("Device name: %s", name)
	}
}
```

## Acknowledgements

-   [tplink-smarthome-api](https://github.com/plasticrake/tplink-smarthome-api): 
    Thanks for the inspiration!

-   [tplink-smarthome-crypto](https://github.com/plasticrake/tplink-smarthome-crypto) 
    Thanks for the excellent documentation/test-cases for encrypting/decrypting 
    the communication

-   [tplink-smarthome-simulator](https://github.com/plasticrake/tplink-smarthome-simulator) 
    Thanks for providing a device simulator for integration tests!

-   [hs1xxplug](https://github.com/sausheong/hs1xxplug): 
    Thanks for the blueprint in golang!

## Development

## Project structure

This project tries to stick as close as possible to the [golang standard project layout](https://github.com/golang-standards/project-layout)

The public parts for this library are located in `/pkg`.


## License

[MIT](LICENSE)
