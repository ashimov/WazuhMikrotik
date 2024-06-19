# WazuhMikrotik

This repository provides Wazuh decoders for Mikrotik and a script for monitoring Wireguard peers' login/logout activities.

**Tested on:**
- RouterOS 7.12
- Wazuh 4.7.1

## 🚀 Setup Instructions

### Step 1: Configure Wazuh Manager to Receive Syslog Messages

Follow the guide at [Wazuh Blog](https://wazuh.com/blog/how-to-configure-rsyslog-client-to-send-events-to-wazuh/) to configure your Wazuh manager to receive Syslog messages.

### Step 2: Deploy Mikrotik Decoders and Rules

1. Copy `1001-mikrotik_decoders.xml` to the Wazuh decoders directory:
    ```sh
    cp /path/to/1001-mikrotik_decoders.xml /var/ossec/etc/decoders/1001-mikrotik_decoders.xml
    ```
    If you are using Docker, run:
    ```sh
    docker cp /path/to/1001-mikrotik_decoders.xml single-node-wazuh.manager-1:/var/ossec/etc/decoders/1001-mikrotik_decoders.xml
    ```

2. Copy `local_rules.xml` to the Wazuh rules directory:
    ```sh
    cp /path/to/local_rules.xml /var/ossec/etc/rules/local_rules.xml
    ```
    If you are using Docker, run:
    ```sh
    docker cp /path/to/local_rules.xml single-node-wazuh.manager-1:/var/ossec/etc/rules/local_rules.xml
    ```

### Step 3: Restart Wazuh

Restart the Wazuh manager to apply the new configurations:
    ```sh
    systemctl restart wazuh-manager
    ```
    If you are using Docker, run:
    ```sh
    docker restart single-node-wazuh.manager-1
    ```

### Step 4: Configure Mikrotik to Send Logs to Syslog Server (Wazuh)

1. Configure the remote logging server:
    ```sh
    /system logging action add name=remote target=remote remote=YOUR_WAZUH_SERVER_IP
    ```

2. Add a logging rule to send all logs to the remote server:
    ```sh
    /system logging add action=remote topics=info
    ```

Make sure to replace `YOUR_WAZUH_SERVER_IP` with the IP address of your Wazuh server.

### Step 5: Monitor Wireguard Peers Activity

1. Copy the script `script.rsc` from the repository to your Mikrotik device.

2. Import and execute the script from the Mikrotik terminal:
    ```sh
    /import script.rsc
    ```

This script will log the connection status of Wireguard peers every 30 seconds, sending this information to the Wazuh Syslog server if properly configured.

## Author

👤 **Giuseppe Trifilio**

* Website: https://github.com/angolo40/WazuhMikrotik
* GitHub: [@angolo40](https://github.com/angolo40)
  
## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/angolo40/WazuhMikrotik).

## Show your support

Give a ⭐️ if this project helped you!

- **XMR**: `87LLkcvwm7JUZAVjusKsnwNRPfhegxe73X7X3mWXDPMnTBCb6JDFnspbN8qdKZA6StHXqnJxMp3VgRK7DcS2sgnW3wH7Xhw`
