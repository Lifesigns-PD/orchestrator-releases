### orchestrator-releases
## To install the binary in linux
1. unzip CassiaInstallationsmainV1.1.4.zip
2. cd CassiaInstallationsmainV1.1.4/CassiaInstallationsmainV1.1.4
3. "ls" to view the files and folders present in the Folder
4. chmod +x DeviceManager_Install.sh  -> Making the Scripts executable
5. sudo ./DeviceManager_Install.sh    -> Run the scripts with Admin rights
6. DeviceManager_Install.sh will download and run Mosquitto service, create required .conf files, start the go binary excutable "go-ble-orchestrator" present in the folder
7. To view the logs "sudo journalctl -u go-ble-orchestrator.service -f"
8. The json_logs, database, app.log, stats_5s.log will be present in /opt/go-ble-orchestrator <cd /opt/go-ble-orchestrator>
9. To view the devices connected in table -> tail -f stats_5s.log
10. To view the errors recorded, change directory to /opt/go-ble-orchestrator then : grep -a "<required-keywords(case-sensititve)>" app.log
11. To check for Mosquitto MQTT : sudo systemctl status mosquitto.service
12. To check for mosquitto.conf : cd /etc/mosquitto/mosquitto.conf
## Uninstall services
1. In the directory of CassiaInstallationsmainV1.1.4
2. chmod +x DeviceManager_Uninstall.sh
3. sudo ./DeviceManager_Uninstall.sh

## DEBUG
# 1. mosquitto.conf should look like the below 
  View using : sudo cat /etc/mosquitto/mosquitto.conf or sudo nano /etc/mosquitto/mosquitto.conf
    mosquitto.conf : 
      #Unified BLE Orchestrator - Auto Generated Config
      pid_file /run/mosquitto/mosquitto.pid
      persistence true
      persistence_location /var/lib/mosquitto/
      log_dest file /var/log/mosquitto/mosquitto.log
      
      #Port 1883 for MQTT
      listener 1883
      allow_anonymous true
# 2. Check for MQTT data by listening:
    (if error comes then : sudo apt install mosquitto-clients)
    mosquitto_sub -t "<Topic>"
    mosquitto_sub -t "#" -> super topic subscribes to all the topics
    mosquitto_sub -t "ble/router/command"
    mosquittto_sub -t "ble/router/status"
# 3. If no mqtt data is seen, then update the firewall rules:
      sudo ufw allow 1883/tcp -> Mosquitto MQTT
      sudo ufw allow 8080/tcp -> ESP32 Dashboard
      sudo ufw allow 8083/tcp -> DeviceManager Dashboard
  # Refresh the rules
      sudo ufw reload
## Other Helpfull commands:
  sudo systemctl restart <service-name> 
  sudo systemctl stop <service-name>
  sudo systemctl start <service-name>
