# Practice Mode

Most often when using the Gizmo it will be away from a competition
during practice or development.  For this purpose, the `gizmo` tool
provides a dedicated practice interface that auto-configures field
services for a single Gizmo to connect and receive control data.

To use practice mode, open a terminal or PowerShell window as
appropriate for your operating system and invoke the `gizmo` tool with
`field practice <number>` as the arguments.  Your command will look
similar to the one below, where 1234 is the team number of the Gizmo
that should bind to this field.

```
$ gizmo field practice 1234
2024-03-16T03:05:09.341-0500 [INFO]  field: Log level: level=info
2024-03-16T03:05:09.341-0500 [INFO]  field.mqtt: MQTT is starting
2024-03-16T03:05:09.341-0500 [INFO]  field.web: HTTP is starting
2024-03-16T03:05:10.343-0500 [INFO]  field.pusher: Connected to broker
2024-03-16T03:05:10.343-0500 [INFO]  field.tlm: Connected to broker
2024-03-16T03:05:10.343-0500 [INFO]  field.metrics: Connected to broker
2024-03-16T03:05:10.343-0500 [INFO]  field.pusher: Subscribed to topics
2024-03-16T03:05:10.343-0500 [INFO]  field.metrics: Subscribed to topics
2024-03-16T03:05:10.351-0500 [INFO]  field: Startup Complete!
2024-03-16T03:05:15.395-0500 [INFO]  field.pusher.gamepad-controller: Successfully bound controller: jsid=0
```

Once you see startup complete and the message that a joystick has been
successfully bound, the practice mode field is ready to use.
