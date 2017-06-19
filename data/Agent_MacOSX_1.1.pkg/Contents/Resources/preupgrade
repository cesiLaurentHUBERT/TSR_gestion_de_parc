#!/bin/bash

if [ -e /Applications/OCSNG.app ]; then
	sudo /Applications/OCSNG.app/Contents/Resources/uninstaller.sh
fi

if [ -e /usr/local/sbin/ocs_mac_agent.php ]; then
echo 'Removing php agent'
	sudo rm -rf /private/etc/ocsinventory-client
	sudo rm /usr/local/sbin/ocs_mac_agent.php
	sudo rm -rf /Library/StartupItems/OCSInventory
fi

exit 0;