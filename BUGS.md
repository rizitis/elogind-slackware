### Possible elogind bugs or local issues:

1. sleep not working in Plasma 6 in my custom zen kernel but works in stock kernel 6.12.48 (I must check if has to do with httpps://github.com/elogind/elogind/commit/cc8f50376a94f29b31eb8cb9a501277b3134007b or my 6.15.9-zen+ kernel not support it.)

what works is
```
loginctl lock-session           # Κλειδώνει την τρέχουσα session
loginctl unlock-session         # Ξεκλειδώνει την session
loginctl list-sessions          # Δείχνει ενεργές sessions
loginctl show-session <ID>      # Δείχνει λεπτομέρειες session
```
loginctl suspend wakeup in a sec but I dont care what is the one that break sleep <br>
```
cat /proc/acpi/wakeup | grep enabled
PEG1      S4    *enabled   pci:0000:00:01.0
PEG2      S4    *enabled   pci:0000:00:06.2
PEG0      S4    *enabled   pci:0000:00:06.0
PS2K      S3    *enabled   pnp:00:01
                *enabled   serio:serio0
RP05      S4    *enabled   pci:0000:00:1c.0
RP07      S4    *enabled   pci:0000:00:1c.6
RP08      S4    *enabled   pci:0000:00:1c.7
XHCI      S4    *enabled   pci:0000:00:14.0
TXHC      S4    *enabled   pci:0000:00:0d.0
AWAC      S4    *enabled   platform:ACPI000E:00
PWRB      S4    *enabled   platform:PNP0C0C:00

```
because in fact i dont have deep s3 sleep because:
```
cat /sys/power/mem_sleep
cat /sys/power/state
[s2idle]
freeze mem disk

```

Everything works in xfce x11 , xfce wayland had some issue with sleep, plasma 6 not working.<br>
It start killing wifi when suspend/sleep and then start it and refuse to sleep.

since loginctl lock-session works I m fine

---

2. 
