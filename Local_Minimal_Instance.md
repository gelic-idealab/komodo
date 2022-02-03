# Initializing a Local, Minimal Komodo Instance
----------
# Part 1 — Initialization

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

## WebXR Client (Core or Module) -- Host PC Only with Localhost

### Download
Download one WebXR Client from [Illinois Box > Komodo > Public > Releases](https://uofi.box.com/s/gsrtdj8bfyxet3gssnefif8d30cpvpk6) and open the client folder.

For example, if you’ve downloaded Impress > v0.1.1, navigate to the v0.1.1 folder on your PC.
****
### Configure

In a text editor, open `relay.js`. 

Edit the fields to match the configuration of `komodo-relay`: 

    var RELAY_BASE_URL = "localhost:3000"; // make sure the port matches
    var API_BASE_URL = "localhost:8999"; // this will not be used
    var VR_BASE_URL = "localhost:8888"; // this will not be used

### Run
In a command line, run `npx serve .` — be sure to include the period.

If prompted to allow some features of the app through Windows Defender Firewall, do allow them.

### Test (Solo)
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

### Test Capture (if available)
In the Settings tab, open the Instructor Menu. Press Start Capture. 

Perform some actions, like moving around, then press End Capture.

In the file explorer, navigate to `komodo-relay/captures/<session number>/<timestamp>/data`. Open the file. You may rename the file to `data.json` if you wish.

Check that the format of the messages is correct.
TODO

## WebXR Client (Core or Module) -- Host PC and Non-Host PC with Localtunnel

### Download
Download one WebXR Client from [Illinois Box > Komodo > Public > Releases](https://uofi.box.com/s/gsrtdj8bfyxet3gssnefif8d30cpvpk6) and open the client folder.

For example, if you’ve downloaded Impress > v0.1.1, navigate to the v0.1.1 folder on your PC.

### Copy 

Copy your existing build to reduce confusion

Rename it to, for example, `v0.1.1-localtunnel`.

### Tunnel the relay

Create a tunnel to the relay server port:

In a command line, in the `komodo-relay` directory, do `node serve.js`. 

In a different command line, do this:
```
npx localtunnel -p 3000
```

If prompted to allow some features of the app through Windows Defender Firewall, do allow them.

Successful output looks like this:
```
$ npx localtunnel -p 3000
npx: installed 22 in 2.534s
your url is: https://<adjective>-<noun>-<num>.loca.lt
```

DO NOT share this URL with anyone, and be sure to exit the command line when finished using Komodo.

Copy this url.

### Configure the build

Configure the build to use the tunneled relay server port:

In a text editor, in your WebXR Client build folder, open `relay.js`. 

Edit the fields to match the configuration of `komodo-relay`: 

    var RELAY_BASE_URL = "https://<adjective>-<noun>-<num>.loca.lt";
    var API_BASE_URL = "localhost:8999"; // this will not be used
    var VR_BASE_URL = "localhost:8888"; // this will not be used

You may wish to keep a copy of the build that doesn't require you to use localtunnel, so that you don't have to use the tunnel for Host-PC-only testing. I recommend annotating the build folder with `-localhost3000` and `-localtunnel` to keep them from getting mixed up.

### Serve the build
In a command line, in your WebXR Client (`-localtunnel`) build folder, run this:
```
npx serve .
``` 
Be sure to include the period.

Successful output looks like this:

```
$ npx serve .
INFO: Accepting connections at http://localhost:<port-of-build>
```

### Tunnel the build

In a different command line, do this:
```
npx localtunnel -p <port-of-build>
```

If prompted to allow some features of the app through Windows Defender Firewall, do allow them.

Successful output looks like this:
```
$ npx localtunnel -p <port-of-build>
npx: installed 22 in 2.534s
your url is: https://<adjective>-<noun>-<num>.loca.lt
```

DO NOT share this URL with the public, and be sure to exit the command line when finished using Komodo.

Copy this url.

### Agree to the warning

Agree to the Tunnel Warning / Reminder. Every device that you wish to connect to your Host PC's relay server, including the Host PC itself, must agree to the warning.

Visit the relay URL: 
`https://<adjective>-<noun>-<num>.loca.lt/`

Press `Click to continue` when prompted.

Copy the URL of the build. Example: `https://<adjective>-<noun>-<num>.loca.lt/`

In a text editor or the URL bar of a recommended browser, add URL query parameters to the URL. Example: `https://<adjective>-<noun>-<num>.loca.lt/?session=123&client=456`
- Two clients connected to the same session will be in the same virtual environment
- Client numbers must be unique — duplicate connections will be terminated

In a recommended browser, connect to the session with the modified build URL:
`https://<adjective>-<noun>-<num>.loca.lt/?session=123&client=456`

Press `Click to continue` when prompted.

In the People tab, a successful connection looks like this:

    Komodo Dev (IL) + /sync [server name]
    123 [session number]
    /sync#a8aa8a8a8aa8a8a8a8a8a8aa8 [socket ID]
    Pong: 123ms [ping/pong or errors]
    
    Client 456 [list of clients, including you]

You can give the URLs to any Host or Non-Host devices who want to connect. Just don't share them with the public. Make sure to close all your command lines when you are done using Komodo.

### Test Capture (if available)
In the Settings tab, open the Instructor Menu. Press Start Capture. 

Perform some actions, like moving around, then press End Capture.

In the file explorer, navigate to `komodo-relay/captures/<session number>/<timestamp>/data`. Open the file. You may rename the file to `data.json` if you wish.

Check that the format of the messages is correct.
TODO

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

### Troubleshooting `mbind: Operation not permitted` is printed repeatedly. 

TODO

### Troubleshooting `Cannot start Docker Compose application`

```
Cannot start Docker Compose application. Reason: Error invoking remote method 'compose-action': Error: Command failed: docker-compose --file "docker-compose.yml" --project-name "komodo-db" --project-directory "<path>...\komodo-db" up -d Some networks were defined but are not used by any service: proxy
```

TODO

### Troubleshooting `Ports are not available`

```
ERROR: for komodo-db 
Cannot start service db: Ports are not available: listen tcp 0.0.0.0:3306: bind: Only one usage of each socket address (protocol/network address/port) is normally permitted. 

ERROR: for db 
Cannot start service db: Ports are not available: listen tcp 0.0.0.0:3306: bind: Only one usage of each socket address (protocol/network address/port) is normally permitted. Encountered errors while bringing up the project.
```

A different application on your PC is using port 3306. Either change the port in the configuration (which means you will need to update `komodo-relay`'s configuration also), or find the program on that port and close it.

So `komodo-db/docker-compose.yml` looks like this: 

```
...
    networks:
      - komodo_internal
    ports: 
      - 3308:3306 # host_port (changeable) : container_port (controlled by mysql image)

networks:
...
```

Then configure `komodo-relay` to use this port in its `config.js`. 

### Troubleshooting -- Double-checking Inspect tab

1. In Docker Desktop, go to Containers / Apps.
2. Expand komodo-db and then select (the child) komodo-db. 
3. Select the Inspect tab. 
4. The various env variables should match those in `.env`
5. Check that the file mounts are set properly —  TODO
6. The port under 3306/TCP should match TODO

`network <name> declared as external, but could not be found` — This means you did not create the docker network in the command line.

## Troubleshooting -- Testing if the database was initialized properly

1. Double-click the connection in MySQL Workbench to open the connection. 
2. In the left sidebar, switch the tab from Administration to Schemas. 
3. Check that the database name matches MYSQL_DATABASE as you specified.
4. Expand that database.
5. Expand the connections table schema.
6. TODO — explain the schema

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

## Other

Check the WebXR Device API to see which browsers are supported.

* https://caniuse.com/webxr
* Some browsers may work with the WebVR polyfill.

Learn more about the Docker images used in these repos:

* mysql: https://hub.docker.com/_/mysql (komodo-db)
* node: https://hub.docker.com/_/node (komodo-relay)

Learn more about relevant Docker concepts:

* docker-compose CLI: https://docs.docker.com/compose/
  * Networks: https://docs.docker.com/compose/networking/
* Networking overview: https://docs.docker.com/network/
  * `docker network create`: https://docs.docker.com/engine/reference/commandline/network_create/

Learn more about other packages in these repos:

* socket.io: https://socket.io/docs/v2/ (WebXR Clients)

Read more about the NPM packages used in this guide:

* localtunnel: https://www.npmjs.com/package/localtunnel
* serve: https://www.npmjs.com/package/serve

Learn about relevant NodeJS and NPM concepts: 

* Running NodeJS scripts: https://nodejs.dev/learn/run-nodejs-scripts-from-the-command-line
* Node Package Manager (NPM): https://nodejs.dev/learn/an-introduction-to-the-npm-package-manager
* NPX (NodeJS Package Runner): https://nodejs.dev/learn/the-npx-nodejs-package-runner
  * https://docs.npmjs.com/cli/v7/commands/npx