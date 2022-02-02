# Initializing a Local, Minimal Komodo Instance
----------
# Part 1 — Initialization

If you have already followed all the steps below, you can refer to this section to repeat the process.

## Prerequisites

Download and install all of the following:

- Docker Desktop — https://www.docker.com/products/docker-desktop
- MySQL Workbench — https://www.mysql.com/products/workbench/
- Node and NPM — https://docs.npmjs.com/downloading-and-installing-node-js-and-npm
## komodo-db

**Clone**
`git clone https://github.com/gelic-idealab/komodo-db.git`

**Configure**
Navigate to the newly cloned directory, `komodo-db`.

In a text editor, copy `.env.template` to a new file, `.env` , and open it. Do not commit `.env` to version control.

Fill out these fields in that file:

    MYSQL_USER=exampleUserName
    MYSQL_DATABASE=exampleDatabaseName
    MYSQL_PASSWORD=examplePassword
    MYSQL_ROOT_PASSWORD= examplePasswordWithLeadingSpace

These values can be whatever you want, but you will refer to them later.

In a command line, run `docker create network komodo_internal` [1].

In a text editor, open `docker-compose.yml`

Make sure the ports are mapped from the container to the host:

    ...
        networks:
          - komodo_internal
        ports:
          - 3306:3306
    
    networks:
    ...

**T****est**
In a command line, run `docker-compose up`.

If prompted to allow some features of the app through Windows Defender Firewall, do allow them.

If prompted to share the files for the `komodo-db/scripts` folder, give permission.

In MySQL Workbench, press (+) to create a new connection. Configure as follows: 

- Connection Name: (whatever you’d like)
- Connection Method: Standard TCP/IP 
- Under the Parameters tab…
    - Hostname: 0.0.0.0 or localhost
    - Port: 3306
    - Username: value of MYSQL_USER from above
    - Password: Press `Store in vault…` , then enter the value of MYSQL_PASSWORD from above

Press `Test Connection`. If the connection succeeds, press ok.

## komodo-relay

**Clone**
`git clone https://github.com/gelic-idealab/komodo-relay.git`

******Configure**
In a text editor, copy `config.template.js` to `config.js`. Do not commit `config.js` to version control.

Fill out the fields to match the configuration of `komodo-db`:

- `db.user`: value in MYSQL_USER
- `db.database`: value in MYSQL_DATABASE
- `db.password`: value in MYSQL_PASSWORD

Fill out the fields to match the computed values of `komodo-db`:

- `db.host`: `0.0.0.0` or `localhost`
- `db.port`: `3306`

Fill out other fields:

- `db.ssl.rejectUnauthorized` = `false`

******Test**
In a command line, run `npm install`. The last few lines of the output should say something like `added <num> packages`.

Run `node serve.js`.

If prompted to allow some features of the app through Windows Defender Firewall, do allow them.

Some log output should contain the following strings for a successful: 

    Database pool created: host: 0.0.0.0, database: komodo
    Komodo relay is running on :3000
    Database initialized with 14 tables

Depending on your configuration, the following values may be different: 

- `host`
- `database`
- port (`Komodo relay is running on :`)
- number of tables
## WebXR Client (Core or Module)

**Download**
Download one WebXR Client from [Illinois Box > Komodo > Public > Releases](https://uofi.box.com/s/gsrtdj8bfyxet3gssnefif8d30cpvpk6) and open the client folder.

For example, if you’ve downloaded Impress > v0.1.1, navigate to the v0.1.1 folder on your PC.
****
**Configure**
In a text editor, open `relay.js`. 

Edit the fields to match the configuration of `komodo-relay`: 

    var RELAY_BASE_URL = "localhost:3000"; // make sure the port matches
    var API_BASE_URL = "localhost:8999"; // this will not be used
    var VR_BASE_URL = "localhost:8888"; // this will not be used

**Run**
In a command line, run `npx serve .` — be sure to include the period.

If prompted to allow some features of the app through Windows Defender Firewall, do allow them.

**Test (Solo)**
Successful output looks like this: 
TODO

Copy the URL. Example: `localhost:12345`

In a text editor or the URL bar of a recommended browser, add URL query parameters to the URL. Example: `localhost:12345?session=123&client=456`

- Two clients connected to the same session will be in the same virtual environment
- Client numbers must be unique — duplicate connections will be terminated

In a recommended browser, connect to the session with the above URL.

In the People tab, a successful connection looks like this:

    Komodo Dev (IL) + /sync [server name]
    123 [session number]
    /sync#a8aa8a8a8aa8a8a8a8a8a8aa8 [socket ID]
    Pong: 123ms [ping/pong or errors]
    
    Client 456 [list of clients, including you]

**Test Capture (if available)**
In the Settings tab, open the Instructor Menu. Press Start Capture. 

Perform some actions, like moving around, then press End Capture.

In the file explorer, navigate to `komodo-relay/captures/<session number>/<timestamp>/data`. Open the file. You may rename the file to `data.json` if you wish.

Check that the format of the messages is correct.
TODO

**Test (Multiplayer on Host PC only)**
In a recommended browser, connect to the session with as many clients as you’d like.

**Test (Multiplayer on multiple devices / WAN)**
See the tunneling section below.

## VR Headset, Tethered to Host PC
## VR Headset, Standalone
## VR Headset, Tethered to Non-Host PC

See VR Headset, Standalone.
****
## Tunneling (optional)

**Configure**
**Test**

## Summary of Configuration
****# Part 2 — Deployment
## Server — Host PC

Ensure komodo-db, komodo-relay are running successfully. You can refer to Summary of Configuration in Part 1 to double-check the different component parts of Komodo.

## Clients — Host PC

**Tethered VR Mode**
**Spectator**

## Clients — Non-Host PC(s)

**Standalone VR Mode**
**Spectator Mode**

## VR Headset

This guide assumes you already have followed steps to set up your VR headset with a browser that supports the [WebXR samples 1 - Immersive VR Session](https://immersive-web.github.io/webxr-samples/) or that your VR headset has a WebXR browser and can run on the same network as your PC.

# Appendix
## komodo-db

**Troubleshooting**
In Docker Desktop, go to Containers / Apps.

1. Expand komodo-db and then select (the child) komodo-db. 
2. Select the Inspect tab. 
3. The various env variables should match those in `.env`
4. Check that the file mounts are set properly —  TODO
5. The port under 3306/TCP should match TODO

`network <name> declared as external, but could not be found` — This means you did not create the docker network in the command line.

Testing if the database was initialized properly — 

1. Double-click the connection in MySQL Workbench to open the connection. 
2. In the left sidebar, switch the tab from Administration to Schemas. 
3. Check that the database name matches MYSQL_DATABASE as you specified.
4. Expand that database.
5. Expand the connections table schema.
6. TODO — explain the schema

Port 3306 may be in use by another process in your machine. You will need to choose a different port in `docker-compose.yml` and then configure `komodo-relay` to use this port in its `config.js`. TODO 

**Footnotes** 
[1] We will not use this network, but the current docker-compose.yml configuration expects it.

## komodo-relay

**Troubleshooting**
`Error: Cannot find module` `'``<name>``'` — you must run `npm install` first.

**Troubleshoot komodo-relay setup**

1. In Docker Desktop, go to Containers / Apps.
2. Expand komodo-db and then select (the child) komodo-db. 
3. Select the Inspect tab. 
4. The port under 3306/TCP will tell you the hostname and port.

**Footnotes**

