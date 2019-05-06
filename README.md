# satnogs-igate

Documentation and scripts to test running both in the same station

Documentacion y scripts para probar correr SatNOGS y un IGATE al mismo tiempo [en espanol](#spanish)


## Why

When running a SatNOGS station there is a Raspberry Pi, probably with a VHF 2 meter antena connected to SDR and internet connection. All those components are the same needed for running an IGATE.

The ground station is not receiving full time, between satellite passes we can to take advantage of that idle time to upload APRS packets into internet.


## How

Installing `direwolf` and hooking up to some SatNOGS scripts that will run after and before each pass, killing and launching `direwolf`. Also, `direwolf` can run at start up.

* Install `direwolf` in your SatNOGS station
* Try to decode some packets 
* Lookup SatNOGS scripts and run kill/launch 
* Run `direwolf` on startup


### Install `direwolf`

SSH into your SatNOGS station in a similar way you done it when installing. THen run the following command to install `direwolf`:

```bash
sudo apt install direwolf
```

### Decode some packets

```bash
rtl_fm -M fm -f 144.930M -p 100 -s 48000 - | direwolf-r 48000 -D 1 -B 9600 -
```

...
