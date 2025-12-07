# JK-BMS Monitoring with Grafana

Monitor your **JK-BMS** data using **Grafana**.

---
This project was tested on **Raspberry Pi 5 (Debian)** with the **JK_B2A8S20P** BMS board
- Hardware version: 11.XW
- Software version: 11.25
- Using built-in Bluetooth

## Installation

### 1. If you are using a non-Bluetooth connection, review the protocol recommendations:  
[mpp-solar Protocol Recommendations](https://github.com/jblance/mpp-solar/wiki/Protocol-Recommendations)

You may need to modify the `mppsolar` container and `mppsolar/jkbms.conf` accordingly.

### 2. Bluetooth Connection
Identify your BMS MAC address using:
```bash
bluetoothctl --timeout 10 scan on
```
, and setup its value in `mppsolar/jkbms.conf` - `port` field.

### 3. Start services:
```
docker compose up -d
```

### 4. Configure grafana
Open grafana http://<your host>:3000 (default creds admin/admin, will require to be changed after 1st login.)
Click Data sources -> Add new data source - Influx DB:
Query Language: Flux
URL: http://localhost:8086
Organization/Token/Default Bucket - use values from docker-compose.yaml.
Leave other options default/unchecked.
Click Dashboards -> New -> Import - select grafana/dashboard1.json file.

## Troubleshooting

Useful commands for Bluetooth BMS connection troubleshooting from Raspberry PI:
```
apt update && apt upgrade
apt install libglib2.0-dev
python3 -m venv venv  
source venv/bin/activate
pip install mppsolar[ble]

#Get firmware/hardware versions
jkbms -p c8:47:8c:ea:c4:4e -n "JK-B2A8S20P" -P JK02 -c getInfo

#Test geting cell data from host
jkbms -p c8:47:8c:ea:c4:4e -n "JK_B2A8S20P" -P JKV11 -c getCellData -D  
```
## Related resources
- [jkbms-influxdb-grafana](https://github.com/ch-reck/jkbms-influxdb-grafana)
- [Monitor your JK-BMS from anywhere](https://sysopstechnix.com/monitor-your-jkbms-from-anywhere-over-the-internet/)
- [mpp-solar](https://github.com/jblance/mpp-solar)
