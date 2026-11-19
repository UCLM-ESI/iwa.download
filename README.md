# IWA — IceGrid Web Admin

IceGrid administration from the browser: a web version of IceGridGUI for ZeroC Ice 3.7. IWA shows the running
deployment —registry, nodes, servers and IceBox services— and acts on it, and creates, edits and deploys IceGrid
applications. Whoever uses it needs nothing but a browser: it is installed on one machine and opened from the
others.

![The live deployment in IWA: on the left, the tree with the registry, the nodes and their servers with their state; on the right, the panel of the selected server, with its actions and its tabs for object adapters, properties, metrics, outputs and the event log.](img/live-deployment.png)

## What it does

- **Live deployment**: the state of the registry, its replicas, the nodes, the servers and the IceBox services,
  as it changes; start and stop them, enable and disable them, send them signals, and shut down nodes and
  registries.
- **What is going on**: stdout, stderr and log files, the Ice log, the runtime properties, the metrics with
  their graphs, and an event log of the deployment.
- **Applications**: a wizard to create an application and a tree editor to change it, with the same validation
  the registry applies and the XML in sight; they are opened from a file or from the registry itself, and
  deploying sends only the changes.
- **IceGrid demos** for teaching, each with its description, ready to deploy and run.

## Installing

IWA is distributed as a Debian package (Debian 13 or later, and derivatives).

### From the ARCO repository (recommended: this is how updates arrive)

```sh
curl -s https://uclm-arco.github.io/debian/get-sources-file.sh | sudo bash
sudo apt install iwa
```

### Downloading the package

The packages are in the ARCO package repository,
[UCLM-ARCO/debian](https://github.com/UCLM-ARCO/debian/tree/master/docs/pool/main/i/iwa). Download the newest
`iwa_<version>_all.deb` and install it:

```sh
sudo apt install ./iwa_*_all.deb
```

Either way, `apt` installs the ZeroC Ice packages that IWA needs as well.

## Running it

```sh
iwa
```

Open <http://localhost:8080>. From another machine, `http://<machine>:8080`.

Useful options (`iwa --help`, `man iwa`):

| Option | What for |
| --- | --- |
| `--demos 3` | Runs the IceGrid of the demos as well, with three nodes, while IWA is running; the connection screen finds it as `Demos`. With `--demos-host "*"` the other machines reach it too |
| `--router` | A Glacier2 router to reach registries that only listen on `tcp` (see below); `--router :4064` for the browsers of other machines |
| `--http-port PORT` | The port the page is served on (8080 by default) |

To keep it running, the package ships the systemd unit `iwa.service`, which installing does not enable:

```sh
sudo systemctl enable --now iwa
```

## Connecting to a registry

The browser speaks to IceGrid over WebSocket, so there are two ways in:

- **Directly**, if the registry has a `ws` endpoint among its client ones
  (`IceGrid.Registry.Client.Endpoints=tcp -p 4061:ws -p 4063`). In "New connection", the machine and the `ws`
  port.
- **Through IWA's router** (`iwa --router`), if the registry only listens on `tcp` and its configuration cannot
  be changed. The connection screen offers "Glacier2 router of IWA", and all that is missing is the `tcp`
  endpoint of the registry, the user name and the password, which the registry checks.

IWA is meant to be used inside a laboratory: it does not encrypt the connections nor check who connects beyond
whatever the registry itself does.

## Documentation

The manual pages `iwa` and `iwa-grid`, and the full guide that the package installs in
`/usr/share/doc/iwa/README.md`.

## License

GPL-2.0, like Ice for JavaScript.
