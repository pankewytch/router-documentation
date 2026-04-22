# Runbook: Promote Dynamic Address to Static Reservation

## When to Use
When a device that received a dynamic address needs a permanent 
reservation at that same address.

## Steps
1. Identify the current lease
   cat /var/lib/kea/dhcp4.leases | grep "DEVICE_IP"
   
2. Note the expiry time and MAC address

3. Stop Kea
   sudo systemctl stop kea-dhcp4-server

4. Remove the dynamic lease from the lease file
   - Edit /var/lib/kea/dhcp4.leases
   - Delete the entire lease block for that address

5. Add the reservation to kea.conf with the device's MAC

6. If shrinking pool, update pool boundaries

7. Start Kea
   sudo systemctl start kea-dhcp4-server

8. Verify the reservation is active
   sudo kea-shell --service dhcp4 lease4-get \
     '{"ip-address": "DEVICE_IP"}'