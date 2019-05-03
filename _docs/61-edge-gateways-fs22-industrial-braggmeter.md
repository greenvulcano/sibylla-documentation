---
title: "Edge gateways - FS22 Industrial BraggMETER"
permalink: /docs/edge-gateways-fs22-industrial-braggmeter/
toc: true
---

# FS22 Industrial BraggMETER

[FS22 – Industrial BraggMETER DI](https://www.hbm.com/en/4604/fs22-industrial-braggmeter-optical-interrogator/)
interrogators are specifically designed to
interrogate Fiber Bragg Grating (FBG) based sensors in industrial environments.
BraggMETER interrogators employ proven continuous swept laser scanning
technology. They include a traceable wavelength reference that provides
continuous calibration to ensure system accuracy over long term operation. The
high dynamic range and output power allow high resolution to be attained even
for long fiber leads and lossy connections.

## How it works

Fiber Bragg Grating (FBG) techonology works by taking a standard single-mode optical fiber and irradiating it with two intense UV beams that originate from the same laser source. Because they come from the same laser source, these two beams are coherced so when they cross over they constructively and destructively interfere. The result is a grating sensor recorded into the fiber formed as a periodic variation of the refractive index of the fiber core. Now when you send light down the optical
fiber, a narrow wave band of light is reflected back whilst all other wavelengths are transmitted. The wavelength of this reflected light varies with the period of the grating which itself varies with both strain and temperature.

For more information refer to the [HBM webiste](https://www.hbm.com/it/).

## Fiber Bragg Grating Sensing System

A Fiber Bragg Grating Sensing System involves the following actors:

* FBG Sensors on a single fibre;
* Fibre optic cables connected to the FS22 Interrogator;
* FS22 Interrogator;
* A Processing Unit which converts wavelengths to measurands of interest.


![BraggMETER overview]({{ "/assets/images/fbg1.png" | relative_url }})


## Benefits

FBG Sensing Technology benefits include:

* Ultra Harsh Environment - Optical sensors are more telerant of extreme temperature,
radiation and vibration than electrical equivalents (fewer components,
no moving parts, no electronics);
* Multiplaxing capability - Dozens of multi-parameter sensors connect to a single
remote instrument with one fibreoptic cable, so reducing system connections,
penetrations and costs;
* Remote monitoring - Electronics can be tens of kilometers away from the
instrumented structure without amplification;
* Zero power - Zero power means immunity to EMI, and instrinsically safe
operation;
* Fatigue durability - Sensors proven over millions of high strain cycles,
often outlasting the host material;
* Sensor size - Miniature sensors allow dense instrumentation, e.g.,
embedment in composites.


## Sibylla FS22-EdgeGateway

GreenVulcano has developed an integration layer for Braggmeter FS22
to allow remote monitoring of infrastructures. This piece of software
works as a Processing Unit which stands between the FS22 Interrogator and the Sibylla IoT Platform.
Its role is to perform several mathematical operations
on FS22 data. These operations include:

* Computing Fourier transforms on accelerometer data;
* Computing temperatures from thermometer sensor's data;
* Computing strain, relative strain, purified microstrain and displacement from strain sensor's data;
* And so on...

This allows the EdgeGateway to capture events like:

* Strain events (e.g., structure or building deformation, velocity or weight of a vehicle);
* Accelerometer events (e.g., fan power on/off);
* Temperature events (e.g., temperature exceedes an alarm threshold);
* And so on...

The FS22-EdgeGateway can be configured to send events data to specific endpoints of the Sibylla IoT platform.
This data can later be retrived from the IoT platform for visualization and analysis.

The FS22-EdgeGateway can be run via command-line or through a graphical interface. It also includes a real-time
monitor for the visualization of real-time sensors data.


![EdgeGateway GUI]({{ "/assets/images/edgegateway1.png" | relative_url }})

![EdgeGateway Monitor]({{ "/assets/images/edgegateway2.png" | relative_url }})

