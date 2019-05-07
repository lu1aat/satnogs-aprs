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
rtl_fm -f 144.93M -M fm -s 200000 -r 32000 -p 100 | direwolf -n 1 -r 32000 -b 16 -t 0 -
```

###  Lookup SatNOGS scripts and run kill/launch

SatNOGS seems to have to settings called:

* `SATNOGS_PRE_OBSERVATION_SCRIPT`
* `SATNOGS_POST_OBSERVATION_SCRIPT`

Let's create to `bash` scripts to kill and start `direwolf` in the home directory:

#### direwolf_stop.sh

```bash
#!/bin/sh
killall direwolf
```

#### direwolf_start.sh

```bash
#!/bin/sh
rtl_fm -f 144.93M -M fm -s 200000 -r 32000 -p 100 | direwolf -n 1 -r 32000 -b 16 -t 0 - &
```

Our home directory (`/home/pi/`) should have the files like:

```bash
$ ls
-rw-r--r-- 1 pi pi    0 May  7 00:28 direwolf_start.sh
-rwxr-xr-x 1 pi pi   29 May  7 00:34 direwolf_stop.sh
```

Grant scripts with execution permissions:

```bash
$ chmod +x /home/pi/direwolf*
```

Run `sudo satnogs-setup` and in the *Advance Settings* menu set full path to the scripts.

The `SATNOGS_PRE_OBSERVATION_SCRIPT` should have `/home/pi/direwolf_stop.sh` in order to stop `direwolf` before the SatNOGS observation starts.

When the observation ends, `direwolf` must be started again so `SATNOGS_POST_OBSERVATION_SCRIPT` value should be `/home/pi/direwolf_start.sh`.


## More

* Configuration examples for `direwolf`.
* Run `direwolf` at startup.
