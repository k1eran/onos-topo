# Command-Line Interface
The project provides a command-line facilities for remotely 
interacting with the topology subsystem.

The commands are available at run-time using the consolidated `onos` client hosted in 
the `onos-cli` repository, but their implementation is hosted and built here.

The documentation about building and deploying the consolidate `onos` client or its Docker container
is available in the `onos-cli` GitHub repository.

## Usage
```bash
> onos topo --help
...
```

### Global Flags
Since the `onos` command is a client, it requires the address of the server as well
as the paths to the key and the certificate to establish secure connection to the 
server.

These options are global to all commands and can be persisted to avoid having to
specify them for each command. For example, you can set the default server address
as follows:
```bash
> onos topo config set address onos-topo-server:5150
```

Subsequent usages of the `onos` command can then abstain from using the `--address` 
option to indicate the server address, resulting in easier usage.

## Example Commands

### Adding, Removing and Listing Devices
Until the full topology subsystem is available, there is a provisional 
administrative interface that allows devices to be added, removed and listed via gRPC.
A command has been provided to allow manipulating the device inventory from the command
line using this gRPC service.

To add a new device, specify the device information protobuf encoding as the value of the 
`addDevice` option. The `id`, `address` and `version` fields are required at the minimum.
For example:

```bash
> onos topo add device device-4 --address localhost:10164 --version 1.0.0
Added device device-4
```

_TODO: We will have to add `type` and `role` fields to the device._

In order to remove a device, specify its ID as follows:
```bash
> onos topo remove device device-2 
Removed device device-2
```

If you do not specify any options, the command will list all the devices currently in the inventory:
```bash
> onos topo get devices -v
NAME			ADDRESS			VERSION
localhost-3             localhost:10163         1.0.0
	USER		PASSWORD	TIMEOUT	PLAIN	INSECURE
	                                5       false	false

stratum-sim-1           localhost:50001         1.0.0
	USER		PASSWORD	TIMEOUT	PLAIN	INSECURE
	                                5       true	false

localhost-1             localhost:10161         1.0.0
	USER		PASSWORD	TIMEOUT	PLAIN	INSECURE
	devicesim       notused         5       false	false

localhost-2             localhost:10162         1.0.0
	USER		PASSWORD	TIMEOUT	PLAIN	INSECURE
	                                5       false	false
```
