# Как настроить iptables на сервере OpenVPN
*OC: Debian 8 Jessie*

Все хорошо описано [здесь](https://www.linode.com/docs/networking/vpn/set-up-a-hardened-openvpn-server)

Сначала сбросить все существующие правила `# iptables -F && sudo iptables -X`. 