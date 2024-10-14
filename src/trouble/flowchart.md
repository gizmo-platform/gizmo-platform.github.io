# Troubleshooting Guide

This page has a large format troubleshooting flowchart that you can
use to work out what you need to do to fix broken systems.

```d2
start : Start Here { shape: diamond }
everything_on : Everything Powers On { shape: diamond }

ds_battery: Check Driver's Station Battery/Power cables { shape: diamond }
ds_card: Check Driver's Station firwmare load { shape: diamond }
ds_battery_bad: Replace/Recharge DS battery { shape: square }
ds_card_bad: Reinstall DS firmware image { shape: square }
ds_check_logs: Connect a display and review logs { shape: square }

check_gizmo: What is the status of the gizmo lights? { shape: diamond }
gizmo_no_lights: No lights on the Gizmo { shape: diamond }
gizmo_reverse_polarity: Power cable must have correct polarity, flip wires { shape: square }
gizmo_bad_battery: Replace battery { shape: square }
gizmo_only_power: Reload Gizmo System Software { shape: square }
gizmo_bad_network: Re-bind gizmo to driver's station. { shape: square }
gizmo_color_wheel: Check gamepad connection, switch (D), and power. { shape: square }
gizmo_stuck_pair: Redo pairing procedure { shape: square }


start -> everything_on
everything_on -> ds_battery : Driver's Station won't power on
ds_battery -> ds_battery_bad : No charge indication on DS battery
ds_battery -> ds_card : DS battery tests good
ds_card -> ds_card_bad : No blinking lights on DS - Reload firmware
ds_card -> ds_check_logs : Blinking lights, but does not finish powering on


everything_on -> check_gizmo : Check the lights on the Gizmo
check_gizmo -> gizmo_no_lights : No lights on the Gizmo
gizmo_no_lights -> gizmo_reverse_polarity : Polarity on the power cable is wrong.
gizmo_no_lights -> gizmo_bad_battery : Battery reads less than 6.5v charge remaining.
gizmo_no_lights -> gizmo_only_power : Only power lights come on.
check_gizmo -> gizmo_bad_network : Network light is solid red
check_gizmo -> gizmo_stuck_pair: Network and field are flashing red or purple.
check_gizmo -> gizmo_color_wheel: Gizmo field light is cycling colors.
```
