# raid status

easy overview: `cat /proc/mdstat`

details: `mdadm --detail /dev/md1`

# force raid check

`echo check > /sys/block/md1/md/sync_action`

# replace broken disk

mark as failed: `mdadm --manage /dev/md1 --fail /dev/sda`

remove disk: `mdadm --manage /dev/md1 --remove /dev/sda`

add new disk: `mdadm --manage /dev/md1 --add /dev/sda`
