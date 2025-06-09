# `dvmhost` Digital Radio Host Application

.. contents:: Table of Contents
    :depth: 3


## Command-line parameters

Typically, you'll run dvmhost as a systemd service with only the `-c` command-line parameter.

| Parameter | Description |
|-----------|-------------|
| -v        | show version information                |
| -h        | show the help screen                    |
| -d        | force modem debug                       |
| -f        | foreground mode                         |
| --syslog  | force logging to syslog                 |
| --setup   | setup and calibration mode              |
| -c <file> | specifies the configuration file to use |
| --remote  | remote modem mode                       |
| -a        | remote modem command address            |
| -p        | remote modem command port               |
| --        |stop handling options                    |

## Setup/Calibration

Run `dvmhost -c <config file> --setup` to get to the setup TUI for calibration.

## config.yml reference


# Digital Voice Modem - Host Software Configuration

This document describes each setting in the DVM host software configuration file. The descriptions are derived directly from inline comments in the YAML configuration.

---


# Digital Voice Modem - Host Software Configuration

This document describes each setting in the DVM host software configuration file. The descriptions are derived directly from inline comments in the YAML configuration.

---

## Top-Level Settings

### `daemon`

* **Description**: Flag indicating whether the host runs as a background (daemon) or foreground task.
* **Type**: boolean
* **Default**: `true`

---

## Logging Configuration

### Logging Levels

* **1**: Debug
* **2**: Message
* **3**: Informational
* **4**: Warning
* **5**: Error
* **6**: Fatal

### `log`

| Field                          | Description                                              |
| ------------------------------ | -------------------------------------------------------- |
| `disableNonAuthoritiveLogging` | Log NON-AUTHORITATIVE errors                             |
| `displayLevel`                 | Console display log level (only used in foreground mode) |
| `fileLevel`                    | Log file verbosity level                                 |
| `useSyslog`                    | Send file logs to syslog instead of writing to files     |
| `filePath`                     | Path for general log file storage                        |
| `activityFilePath`             | Path for activity log file storage                       |
| `fileRoot`                     | Log filename prefix                                      |

---

## Network Configuration

### `network`

| Field                     | Description                                                      |
| ------------------------- | ---------------------------------------------------------------- |
| `enable`                  | Enables host networking                                          |
| `id`                      | Network Peer ID                                                  |
| `address`                 | FNE server address                                               |
| `port`                    | FNE server port                                                  |
| `password`                | FNE access password                                              |
| `encrypted`               | Enables encryption for endpoint networking                       |
| `presharedKey`            | AES-256 (32-byte) key for encryption (must be 64 hex characters) |
| `jitter`                  | Max allowable DMR network jitter (ms)                            |
| `slot1`, `slot2`          | Enable DMR slot 1 or 2 traffic                                   |
| `updateLookups`           | Sync RID/TGID tables from the network                            |
| `saveLookups`             | Save updated lookup tables locally (helpful in site trunking)    |
| `allowActivityTransfer`   | Send activity logs to network                                    |
| `allowDiagnosticTransfer` | Send diagnostic logs to network                                  |
| `allowStatusTransfer`     | Send status updates to network                                   |
| `debug`                   | Enable verbose debug logging for network stack                   |

#### RPC Settings

| Field         | Description                                                 |
| ------------- | ----------------------------------------------------------- |
| `rpcAddress`  | IP to listen for RPC (use `0.0.0.0` to bind all interfaces) |
| `rpcPort`     | Listening port for RPC calls                                |
| `rpcPassword` | Password for RPC access                                     |
| `rpcDebug`    | Enable debug logging for RPC                                |

#### REST API Settings

| Field                | Description                        |
| -------------------- | ---------------------------------- |
| `restEnable`         | Enables the REST API interface     |
| `restAddress`        | Address to bind for REST API       |
| `restPort`           | Port for REST API access           |
| `restSsl`            | Enable SSL for REST API            |
| `restSslCertificate` | Certificate file path for REST API |
| `restSslKey`         | Key file path for REST API         |
| `restPassword`       | REST API access password           |
| `restDebug`          | Enable debug logging for REST API  |

---

## Digital Protocol Configuration

### DMR (Digital Mobile Radio)

#### Top-Level Fields

| Field                    | Description                                              |
| ------------------------ | -------------------------------------------------------- |
| `enable`                 | Enables the DMR protocol stack                           |
| `disableNetworkGrant`    | Prevents network-originated calls from generating grants |
| `ignoreAffiliationCheck` | Disables TGID affiliation checks before granting         |
| `convNetGrantDemand`     | Forces network grant demand in conventional mode         |
| `embeddedLCOnly`         | Transmits only RF-embedded LC data                       |
| `dumpTAData`             | Log Talker Alias (TA) data                               |
| `dumpDataPacket`         | Enable verbose DMR data packet dump                      |
| `repeatDataPacket`       | Repeat incoming DMR data traffic                         |
| `dumpCsbkData`           | Enable verbose CSBK data dump                            |
| `verifyReg`              | Perform unit registration verification                   |
| `disableUnitRegTimeout`  | Disable 12-hour idle registration timeout                |
| `nRandWait`, `backOff`   | Subscriber TDMA contention parameters (do not modify)    |
| `callHang`, `txHang`     | Hang timers for voice calls and transmission tail        |
| `silenceThreshold`       | BER threshold for muting voice                           |
| `frameLossThreshold`     | Max lost frames before ending call                       |
| `queueSize`              | Internal frame buffer size                               |
| `verbose`                | Enable general verbose logging                           |
| `debug`                  | Enable protocol-specific debug logging                   |

#### Beacons

| Field      | Description                        |
| ---------- | ---------------------------------- |
| `enable`   | Enable roaming beacon transmission |
| `interval` | Seconds between beacons            |
| `duration` | Beacon transmission duration       |

#### Timeslot Control Channel (TSCC)

| Field                       | Description                                      |
| --------------------------- | ------------------------------------------------ |
| `dedicated`                 | Whether this TSCC is a dedicated control channel |
| `enable`                    | Enable TSCC behavior                             |
| `slot`                      | Timeslot for TSCC traffic                        |
| `disableGrantSourceIdCheck` | Skip grant source ID validation                  |

---

### P25 (Project 25)

#### Top-Level Fields

| Field                                                | Description                                          |
| ---------------------------------------------------- | ---------------------------------------------------- |
| `enable`                                             | Enables the P25 protocol                             |
| `tduPreambleCount`                                   | Number of TDUs sent before a call                    |
| `controlOnly`                                        | Prevent voice repetition on CC                       |
| `disableNetworkHDU`                                  | Skip HDU from network call origin                    |
| `disableNetworkGrant`                                | Disable automatic grants for network calls           |
| `ignoreAffiliationCheck`                             | Disable TGID affiliation checks                      |
| `inhibitUnauthorized`                                | Block unauthorized RIDs                              |
| `legacyGroupGrnt`, `legacyGroupReg`                  | Fallback TGID handling for legacy radios             |
| `convNetGrantDemand`                                 | Force grant demand for conventional traffic          |
| `demandUnitRegForRefusedAff`                         | Require registration after denied affiliation        |
| `verifyAff`, `verifyReg`                             | Enable verification for affiliation and registration |
| `disableUnitRegTimeout`                              | Disable idle registration timeout                    |
| `requireLLAForReg`                                   | Require LLA check before unit registration           |
| `dumpDataPacket`, `repeatDataPacket`, `dumpTsbkData` | Logging and traffic repetition options               |
| `callHang`                                           | Hang time after a voice call ends                    |
| `noStatusAck`, `noMessageAck`                        | Suppress ACKs for status/message packets             |
| `unitToUnitAvailCheck`                               | Perform availability checks before private calls     |
| `allowExplicitSourceId`                              | Enable support for explicit source IDs               |
| `sndcpSupport`                                       | Enable SNDCP data support                            |
| `silenceThreshold`                                   | BER threshold for silencing                          |
| `frameLossThreshold`                                 | Frame loss threshold for call termination            |
| `queueSize`                                          | Frame queue size                                     |
| `verbose`, `debug`                                   | Logging toggles                                      |

#### Control Channel

| Field                                          | Description                                      |
| ---------------------------------------------- | ------------------------------------------------ |
| `enable`                                       | Enable TSBK control data support                 |
| `ackRequests`                                  | ACK incoming TSBK requests                       |
| `dedicated`                                    | Set as dedicated CC                              |
| `broadcast`                                    | Broadcast CC data                                |
| `interval`, `duration`                         | Broadcast interval/duration for non-dedicated CC |
| `notifyActiveTG`                               | Notify VCs of active TGIDs                       |
| `disableTSDUMBF`                               | Transmit single-block TSDUs                      |
| `enableTimeDateAnn`                            | Enable time/date TSBK broadcast                  |
| `disableGrantSourceIdCheck`                    | Skip source ID grant validation                  |
| `disableAdjSiteBroadcast`                      | Suppress adjacent site announcements             |
| `redundantImmediate`, `redundantGrantTransmit` | Redundancy flags for immediate/grant messages    |

---

### NXDN (Next Gen Digital Narrowband)

#### Top-Level Fields

| Field                    | Description                                       |
| ------------------------ | ------------------------------------------------- |
| `enable`                 | Enable NXDN protocol                              |
| `ignoreAffiliationCheck` | Disable TGID affiliation checks                   |
| `verifyAff`, `verifyReg` | Perform verification for affiliation/registration |
| `disableUnitRegTimeout`  | Disable idle timeout                              |
| `dumpRcchData`           | Enable verbose logging of RCCH data               |
| `callHang`               | Hang time after a voice call                      |
| `silenceThreshold`       | BER threshold for silencing                       |
| `frameLossThreshold`     | Lost frame limit before ending call               |
| `queueSize`              | Frame buffer size                                 |
| `verbose`, `debug`       | Logging options                                   |

#### Control Channel

| Field                       | Description                              |
| --------------------------- | ---------------------------------------- |
| `enable`                    | Enable CC traffic support                |
| `dedicated`                 | Set as dedicated CC                      |
| `broadcast`                 | Enable CC data broadcasts                |
| `interval`, `duration`      | Timing parameters for broadcast messages |
| `disableGrantSourceIdCheck` | Skip source ID validation during grant   |

---

## System Configuration

### `system`

| Field                              | Description                                                 |
| ---------------------------------- | ----------------------------------------------------------- |
| `identity`                         | Textual label of this host                                  |
| `timeout`                          | Maximum total call transmit time (in seconds)               |
| `duplex`                           | Enables full-duplex (simultaneous Rx/Tx) operation          |
| `simplexSameFrequency`             | Use same frequency for simplex operation                    |
| `modeHang`                         | Hang time on digital mode in multi-mode operation (seconds) |
| `fixedMode`                        | Operate in a single digital mode only                       |
| `rfTalkgroupHang`                  | Hang time before allowing other TGID traffic                |
| `activeTickDelay`, `idleTickDelay` | Processing loop delays (ms)                                 |
| `localTimeOffset`                  | Timezone offset from GMT                                    |
| `disableWatchdogOverflow`          | Disable watchdog overflow check                             |

#### `info`

| Field                   | Description                                   |
| ----------------------- | --------------------------------------------- |
| `latitude`, `longitude` | Geographic coordinates for reporting location |
| `height`                | Site elevation in meters                      |
| `power`                 | Transmit power in watts                       |
| `location`              | Free-form textual site location               |

---

## CW ID Configuration

### `cwId`

| Field      | Description                            |
| ---------- | -------------------------------------- |
| `enable`   | Enable CW ID beaconing                 |
| `time`     | Time interval between CW IDs (minutes) |
| `callsign` | Callsign transmitted as CW ID          |

---

## Identity & Site Configuration

### `config`

| Field                                            | Description                                         |
| ------------------------------------------------ | --------------------------------------------------- |
| `authoritative`                                  | Host automatically grants channels for TGID ops     |
| `supervisor`                                     | Host grants VC TGID permissions                     |
| `channelId`, `channelNo`                         | Channel identity/number for TX frequency resolution |
| `colorCode`, `nac`, `ran`                        | DMR, P25, NXDN channel parameters                   |
| `pSuperGroup`, `announcementGroup`               | TGID groups for patching/announcements              |
| `dmrNetId`, `netId`, `sysId`, `rfssId`, `siteId` | Network and site identity values                    |

#### `controlCh`

| Field                                  | Description                                  |
| -------------------------------------- | -------------------------------------------- |
| `rpcAddress`, `rpcPort`, `rpcPassword` | RPC access for control channel notifications |
| `notifyEnable`                         | Whether to notify CC of VC traffic status    |
| `presence`                             | Network presence interval (seconds)          |

#### `voiceChNo`

| Field                                  | Description                          |
| -------------------------------------- | ------------------------------------ |
| `channelId`, `channelNo`               | Identity and number of voice channel |
| `rpcAddress`, `rpcPort`, `rpcPassword` | RPC config for each voice channel    |

#### `secure`

| Field | Description                                        |
| ----- | -------------------------------------------------- |
| `key` | AES-128 key for LLA encryption (32 hex characters) |

---

## Modem Configuration

### `modem.protocol`

| Field  | Description                                           |
| ------ | ----------------------------------------------------- |
| `type` | Modem port type: `null` (loopback) or `uart` (serial) |
| `mode` | Interface mode: `air` (radio) or `dfsi` (TIA-102)     |

#### `uart`

| Field   | Description                      |
| ------- | -------------------------------- |
| `port`  | Serial device path               |
| `speed` | UART baud rate (default: 115200) |

### Signal Processing

| Field                               | Description                                    |
| ----------------------------------- | ---------------------------------------------- |
| `rxInvert`, `txInvert`, `pttInvert` | Invert RX, TX, or PTT polarity                 |
| `dcBlocker`                         | Enable DC blocking filter                      |
| `cosLockout`                        | Prevent TX while COS is active                 |
| `fdmaPreamble`                      | Number of preambles before FDMA TX             |
| `dmrRxDelay`                        | Delay before DMR TX after RX signal detected   |
| `p25CorrCount`                      | Required packet correlations before P25 decode |

### FIFO Buffers

| Buffer           | Default | Description                       |
| ---------------- | ------- | --------------------------------- |
| `dmrFifoLength`  | 560     | DMR transmit buffer size (bytes)  |
| `p25FifoLength`  | 522     | P25 transmit buffer size (bytes)  |
| `nxdnFifoLength` | 538     | NXDN transmit buffer size (bytes) |

### `hotspot`

| Field                                                                                            | Description                                          |
| ------------------------------------------------------------------------------------------------ | ---------------------------------------------------- |
| `dmrDiscBWAdj`, `dmrPostBWAdj`, `p25DiscBWAdj`, `p25PostBWAdj`, `nxdnDiscBWAdj`, `nxdnPostBWAdj` | Demodulator tuning (range -128 to 128)               |
| `adfGainMode`                                                                                    | ADF7021 gain (0: Auto, 1: High Lin, 2: Low, 3: High) |
| `afcEnable`                                                                                      | Enable ADF7021 automatic frequency correction        |
| `afcKI`, `afcKP`, `afcRange`                                                                     | AFC loop tuning parameters                           |
| `txTuning`, `rxTuning`                                                                           | Manual TX/RX frequency offset (Hz)                   |

### `repeater`

| Field                              | Description          |
| ---------------------------------- | -------------------- |
| `dmrSymLvl3Adj`, `dmrSymLvl1Adj`   | DMR TX level tuning  |
| `p25SymLvl3Adj`, `p25SymLvl1Adj`   | P25 TX level tuning  |
| `nxdnSymLvl3Adj`, `nxdnSymLvl1Adj` | NXDN TX level tuning |

### `softpot`

| Field                                                                | Description                            |
| -------------------------------------------------------------------- | -------------------------------------- |
| `rxCoarse`, `rxFine`, `txCoarse`, `txFine`, `rssiCoarse`, `rssiFine` | Digital potentiometer settings (0–255) |

### `dfsi`

| Field                              | Description                            |
| ---------------------------------- | -------------------------------------- |
| `rtrt`, `diu`                      | Protocol flags                         |
| `jitter`                           | Buffer length (ms)                     |
| `callTimeout`                      | Reset timer (ms) if no frames received |
| `fsc`, `fscHeartbeat`, `initiator` | FSC protocol options                   |
| `dfsiTIAMode`                      | Format DFSI frames in TIA-102 standard |

### General Signal Levels

| Field                                                                             | Description                        |
| --------------------------------------------------------------------------------- | ---------------------------------- |
| `rxDCOffset`, `txDCOffset`                                                        | DC offset correction (-128 to 128) |
| `rxLevel`, `txLevel`                                                              | RX/TX modulation levels            |
| `rssiMappingFile`                                                                 | Path to RSSI mapping data          |
| `packetPlayoutTime`                                                               | Buffer playout time (deprecated)   |
| `disableOFlowReset`, `ignoreModemConfigArea`, `dumpModemStatus`, `trace`, `debug` | Advanced modem behavior flags      |

---

## ACL and Table Configurations

### `iden_table`

| Field  | Description                         |
| ------ | ----------------------------------- |
| `file` | Path to channel identity table file |
| `time` | Update interval in minutes          |

### `radio_id`

| Field  | Description                                               |
| ------ | --------------------------------------------------------- |
| `file` | Path to RID ACL file                                      |
| `time` | Update interval (set to 0 to avoid overwriting FNE rules) |
| `acl`  | Enable/disable enforcement of RID ACL                     |

### `talkgroup_id`

| Field  | Description                                               |
| ------ | --------------------------------------------------------- |
| `file` | Path to talkgroup rules file                              |
| `time` | Update interval (set to 0 to avoid overwriting FNE rules) |
| `acl`  | Enable/disable enforcement of TGID ACL                    |

