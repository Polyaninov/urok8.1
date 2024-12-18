1. Поднять две виртуальные машины с любым Linux на борту в Virtualbox (или в том гипервизоре, который вы используете) и добавить к ним второй сетевой интерфейс


![адапптер 2](https://github.com/user-attachments/assets/fc0bc4f0-e6a9-4673-9a05-8f0771a0e5f3)

![Screenshot_1](https://github.com/user-attachments/assets/6f478c5f-a93f-47f0-b9d6-932f590e9512)

2. Задать hostname машинам
сервер-1
сервер-2
sudo hostnamectl set-hostname server-1
sudo nano /etc/hosts
sudo reboot
sudo netplan apply


3. Настроить статические IP-адреса на машинах из сети 172.16.1.0/24, проверить сетевое соединение между машинами с помощью ping

  network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3: # Имя вашего сетевого интерфейса может отличаться, проверьте командой ip a
      dhcp4: no
      addresses: [172.16.1.10/24]
      gateway4: 172.16.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4] 
        
4.На одной из машин настроить DHCP-сервер, который будет раздавать адреса из диапазона 172.16.1.15 — 172.16.1.25
sudo apt update && sudo apt install isc-dhcp-server
sudo nano /etc/dhcp/dhcpd.conf

Поднять еще одну виртуальную машину с именем хоста server-3 и зарезервировать для нее на DHCP-сервере IP-адрес 172.16.1.20
Настроить server-2 и server-3 так, чтобы они получали IP-адреса от настроенного DHCP-сервера
Проверить, что для ВМ server-3 выдается только зарезервированный ip
