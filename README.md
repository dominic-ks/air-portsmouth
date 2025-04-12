# Portsmouth Open Air Quality Project

A project to collect, combine, and present air quality data for Portsmouth in a way that is accessible, understandable, and useful to everyone.

Air quality data already exists — from government sensors, EU networks, and community projects — but it is often scattered, hard to interpret, or difficult to access in one place.

This project brings it together.

## Project Focus

- Aggregate real-time air quality data from multiple open sources.
- Visualise and share this data in a clear, user-friendly way.
- Provide open access to historical data and trends.
- Supplement this with our own DIY sensor devices where appropriate.
- Contribute to citizen science and open data platforms.

---

## Data Sources

We plan to gather data from:

| Source | Link | Notes |
|--------|------|-------|
| Portsmouth City Council AQ Data | [Portsmouth AQ Page](https://www.portsmouth.gov.uk/services/environmental-health/air-quality-and-pollution/air-quality-in-portsmouth/) | Static & report-based data, location-specific measurements. |
| OpenAQ API | [https://docs.openaq.org](https://docs.openaq.org/about/about) | Aggregates official monitoring stations & networks. |
| Sensor.Community (formerly Luftdaten) | [https://sensor.community/en/](https://sensor.community/en/) | Community-led sensor network — we aim to contribute here too. |
| AQICN / EU Networks | [https://aqicn.org/city/united-kingdom/portsmouth/](https://aqicn.org/city/united-kingdom/portsmouth/) | Existing real-time data visualisation. |

---

## Our Hardware

We're building our own sensors to fill in the gaps — to capture air quality data from the places people actually live, travel, and spend time. Existing monitoring stations provide valuable information, but they’re few and far between.

We will show our device designs and code here, so that anyone can build their own sensors. But, we will be creating a number of sensors ourselves, and could be open to taking donations to fund devices and also looking for people to contribute by hosting a sensor in their home or business.

### Sensors Used To Date

Our current prototype devices use affordable, readily available sensors. These allow us to collect indicative air quality data and experiment with different measurement techniques.

| Sensor    | Measures                | Notes                                            |
|-----------|-------------------------|--------------------------------------------------|
| PMS5003   | PM2.5 / PM10            | Particulate sensor — widely used, reliable for trends. |
| MICS6814  | CO, NO₂, NH₃            | Metal-oxide sensor — sensitive but requires calibration and suffers from cross-sensitivity. |
| MQ-135    | VOCs, CO₂ (rough), Benzene, Alcohol, Smoke | Very cheap sensor — highly dependent on calibration and environment. |
| DHT22 / Similar | Temperature / Humidity | Basic environmental data for context and sensor compensation. |

---

### Sensors Identified for Future Improvement (Achievable Cost)

We have identified more accurate or reliable sensors for future devices, while keeping costs reasonable.

| Sensor    | Measures                | Notes                                            |
|-----------|-------------------------|--------------------------------------------------|
| SCD41     | CO₂                     | Sensirion NDIR sensor — compact, highly accurate (~£40–50). |
| SGP30 or SGP40 | VOCs, eCO₂ estimate | Improved VOC measurement with built-in algorithms (~£12–20). |
| BME280 / BME680 | Temp / Humidity / Pressure | Better environmental sensing (~£5–10). |

---

### High-Accuracy Sensors Considered but Likely Too Expensive for Most Deployments

These professional-grade sensors offer excellent accuracy but are likely cost-prohibitive for most community deployments.

| Sensor              | Measures                | Notes                                           |
|--------------------|-------------------------|-------------------------------------------------|
| Alphasense A4 Series | NO₂, SO₂, CO, H₂S, O₃  | Electrochemical sensors — lab-quality accuracy but expensive and require analog front-end circuits. |
| Sensirion SEN55 / SEN54 | Multi-gas / PM        | All-in-one sensors — ideal but ~£100+ per unit. |
| PID-AH (Alphasense)  | VOCs (Photoionisation Detector) | Accurate VOC detection but not practical for this project. |

---

## Device Architecture

Our current devices use:

- A **Raspberry Pi Zero W** for network connectivity (Wi-Fi) and data handling.
- A **Raspberry Pi Pico** (or similar microcontroller) to interface with the sensors and handle readings — particularly useful for analog sensors and UART conflicts.

This setup provides flexibility:
- The Pi Zero manages communication with the backend and remote control.
- The Pico handles sensor logic, analog readings (for MQ-135), and passes data via USB or serial to the Pi Zero.

Power is currently supplied via mains electricity, using flat USB cables through windows for easy deployment in urban environments. Devices connect to the host's Wi-Fi network for data upload.

---

## Device Deployment Considerations

### Power
- Our initial deployments will use mains power where available.
- Flat USB cables through windows are a practical solution in urban environments.
- Battery/solar is not currently viable for continuous outdoor monitoring in the UK climate.

### Connectivity
- Devices will connect via the host's Wi-Fi network.
- Data will be sent to our backend platform for aggregation and visualisation.

---

## Mobile Device Development — Battery Powered with GPS & Mobile Data

In addition to our static air quality monitors, we are also exploring the development of a mobile, battery-powered device capable of tracking air quality along a route — for example, while cycling, walking, or running.

The goal is to capture location tracked air quality data that can be demonstrated using a map interface, similar to existing cycling or running apps. This would allow users to see their exposure to air pollution for a paticular activity or the time of day, as well as potentially contributing to our wider dataset.

This could provide individuals with a more personalised view of their exposure to air pollution during their day-to-day travel and activity.

### Hardware Overview

This mobile device is based on components already used in our existing devices, but adds battery management, GPS tracking, and mobile data connectivity:

| Component | Purpose | Notes |
|-----------|---------|-------|
| Raspberry Pi Zero 2 W | Core device | Low-power Linux platform with Wi-Fi for config & debugging. |
| Li-ion Battery HAT for Pi | Battery power management | Allows the device to run untethered from mains. Paired with 18650 cells. |
| Waveshare GSM/GPRS/GNSS HAT | GPS tracking & mobile data | Provides location data and connectivity via SIM card (2G/3G). |
| Existing Sensors | Air quality monitoring | Including PMS5003, MICS6814, MQ-135 — as used in our static devices. |

---

### Current Challenges

Battery life is the main technical challenge for this device:

- Early tests with different 18650 Li-ion cells have shown limited battery life — further testing is required with higher capacity cells and optimised firmware.
- Power consumption is currently high due to continuous sensor activity, mobile data transmission, and GPS polling.
  
Additionally, the current device setup does not yet support:
- Simultaneous use of the GPS feed and mobile data connection from the Waveshare HAT — this requires firmware development and serial port management to handle shared resources.
- Firmware updating or robust error handling for mobile data dropouts or GPS signal loss.

---

## Firmware

_This section will document the firmware running on our devices._

Details to include:
- Microcontroller platform (Raspberry Pi Pico)
- Sensor integration code
- Communication protocols (e.g. USB Serial, MQTT, REST)
- Update mechanisms
- Power-saving strategies

_(Details to be added.)_


---

## Backend

_This section will document the backend process data from our devices._

Details to include:
- MQTT broker setup
- Use of Firebase Functions / Firestore for current status
- BigQuery for historical data
- Data visualisation tools (e.g. Grafana, custom dashboards)

_(Details to be added.)_

---

## Existing Kits & Resources

Resources for building DIY sensors or learning more:

- Sensor.Community Kit: https://nettigo.eu/products/sensor-community-kit-sds011-bme280-english-language-harness-cable-edition  
- Sensirion Sensors: https://sensirion.com/products/catalog  
- Alphasense Sensors: https://www.alphasense.com/products/view-by-target-gas/co2-sensors  

---

## Get Involved

This is an open-source project — contributions welcome.

Whether you want to help with:
- Building or hosting a sensor
- Data visualisation
- Backend development
- Hardware testing
- Calibration techniques
- Local deployment
- Or just have ideas...

Feel free to get in touch or open an issue.

