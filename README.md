# T-REX

## What Is `T-REX`?

> [!TIP]
> `T-REX` (Trivial Record Exchange) streamlines the process or even facilitates ad-hoc transfer of low-dimensional result data, eliminating the need for manual transcription from laboratory devices to paper or software programs.
> 
> Here is an example tare weight and a environmental temperature, formatted using `T-REX`:
> ```
> TARE$MGM:2.5E2+ENV$KEL:293.15
> ```

## Introduction

The `T-REX` serves as ad-hoc transfer for low dimensional result data. It is designed as low entry barrier first stepping stone towards automated lab device data exchange and therefore focuses on simplicity instead of feature-richness. It for example allows to easily transfer a weight value from a balance but by no means is made to transfer a Chromatography data set (for that use case, use a [PAC-ID](https://github.com/ApiniLabs/PAC-ID) that points to the data record in your CDS instead).

Lab Devices have a lifetime of up to 30 years. Once first devices with standardized API’s are sold, it will still likely take decades until most lab devices found in average labs have such capabilities. The installed base of devices that is capable of displaying QR codes and for which there is still ongoing firmware development however is already fairly large. That installed base is the main target of the T-REX as it brings digitalization benefits that are usable now into the lab as it is today.

The `T-REX` is the latest evolution of ideas previously known as LabQR and ResultQR from the SiLA 2 Core Working Group.

The goals for `T-REX` are:

- Simple and easy to understand, parse and transmit
- Ability to store into a small QR code due to compatibility with QR code alphanumeric encoding
- Direct usage as query part of a URL or an extension to a [PAC-ID](https://github.com/ApiniLabs/PAC-ID)

## Specification


## Terminology Used

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) "Key words for use in RFCs to Indicate Requirement Levels".

## FAQ

**Q: PAC-ID vs. T-REX. When to use which?**

**A:** In certain cases it’s not entirely clear what should go into a T-REX and what should be a segment of the PAC-ID itself. For example a GS1 code containing a manufacturing date, that date would be a better fit for T-REX than a ID segment.

## License

Shield: [![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa]

This work is licensed under a
[Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa].

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg
