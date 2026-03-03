
# Firmware Update Demo

## Overview

This document provides instructions for running the EVerest demo with Maeve CSMS using OCPP 2.0.1, including an end-to-end test that demonstrates firmware update functionality. The test simulates a malicious firmware update that reduces the charge rate to 50% of capacity.

## Prerequisites

### System Requirements
- Ubuntu 22.04

### Required Software
- Docker
- Go



## Setup Running Everest and CSMS on different hosts

### 1. Clone Repositories

Clone the required repositories with the appropriate branches to host:

Everest Host: 

```bash
git clone --branch firmware_update https://github.com/tijcolem/everest-demo/
```

CSMS Host: 

```bash
git clone --branch firmware_update https://github.com/tijcolem/everest-demo/
git clone --branch add_firmware_update https://github.com/tijcolem/maeve-csms
```

## Running the Demo

### 1. Start the CSMS

Navigate to the everest-demo directory and start on CSMS host:

```bash
cd everest-demo
./demo-iso15118-2-ocpp-201-csms.sh -1 -r /home/ubuntu/git/everest-demo/
```

### 2. Start Everest


Edit the `demo-iso15118-2-ocpp-201-everest.sh` and assign CMS host IP to CSMS_HOST_IP
e,g, CSMS_HOST_IP=172.18.9.86

Start up everest
```bash
cd everest-demo
./demo-iso15118-2-ocpp-201-everest.sh -1 -r /home/ubuntu/git/everest-demo/
```


### 3. Access the User Interface

Open a web browser and navigate to the everest host: e.g. below assumes localhost. 
```
http://localhost:1880/ui
```

### 4. Initiate Charging

1. Select a charging profile
2. Swipe the RFID card
3. Start the charging session

### 5. Stop the Initial Charging Session

Stop the charging session before proceeding with the firmware update test.

### 6. Run the Firmware Update Test

Navigate to the maeve-csms directory on CSMS host and execute the end-to-end test:

```bash
cd ../maeve-csms/e2e_tests/test_driver
go test --tags=e2e -v ./... -count=1
```

### 7. Restart the EVerest Manager

Restart the manager for the new firmware update to take effect:

```bash
docker exec everest-ac-demo-manager-1 sh /ext/build/run-scripts/run-sil-ocpp201-pnc.sh
```

### 8. Verify Firmware Update

Start a new charging session. The vehicle should now charge at a reduced rate (50% of the original capacity), demonstrating that the malicious  firmware update was successfully applied. 


## Setup Running on Same VM

### 1. Clone Repositories

Clone the required repositories with the appropriate branches:

```bash
git clone --branch firmware_update https://github.com/tijcolem/everest-demo/
git clone --branch add_firmware_update https://github.com/tijcolem/maeve-csms
```

## Running the Demo

### 1. Start the EVerest Demo

Navigate to the everest-demo directory and launch the demo:

```bash
cd everest-demo
./demo-iso15118-2-ocpp-201.sh -1 -r /home/ubuntu/git/everest-demo/
```

### 2. Access the User Interface

Open a web browser and navigate to:
```
http://localhost:1880/ui
```

### 3. Initiate Charging

1. Select a charging profile
2. Swipe the RFID card
3. Start the charging session

### 4. Stop the Initial Charging Session

Stop the charging session before proceeding with the firmware update test.

### 5. Run the Firmware Update Test

Navigate to the maeve-csms directory and execute the end-to-end test:

```bash
cd ../maeve-csms/e2e_tests/test_driver
go test --tags=e2e -v ./... -count=1
```

### 6. Restart the EVerest Manager

Restart the manager for the new firmware update to take effect:

```bash
docker exec everest-ac-demo-manager-1 sh /ext/build/run-scripts/run-sil-ocpp201-pnc.sh
```

### 7. Verify Firmware Update

Start a new charging session. The vehicle should now charge at a reduced rate (50% of the original capacity), demonstrating that the malicious  firmware update was successfully applied. 
