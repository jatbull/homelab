# homelab
code to change proxmox node name
execute through ssh
don't forget to change variables


mkdir -p /tmp/qemu ## make temp dir for moving VM config files
cp /etc/pve/nodes/pve/qemu-server/* /tmp/qemu/

hostnamectl set-hostname "ser8"
sed -i "s/pve/ser8/g" /etc/hosts

services=("pveproxy.service" "pvebanner.service" "pve-cluster.service" "pvestatd.service" "pvedaemon.service")
for service in "${services[@]}"
do
systemctl restart "$service"
done

rm -rf "/etc/pve/nodes/pve"
cp /tmp/qemu/* /etc/pve/nodes/ser8/qemu-server/
rm /tmp/qemu/*
