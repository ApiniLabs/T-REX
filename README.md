# T-REX

## What Is `T-REX`?

> [!TIP]
> `T-REX` (Trivial Record Exchange) facilitates ad-hoc transfer of low-dimensional result data, eliminating the need for manual transcription from laboratory devices to paper or software programs. T-REX is enabled through the definition of a simple and lightweight text-based format for data exchange.
> 
> Here is an example tare weight (250 mg) and an environmental temperature (293.15 K), formatted using `T-REX`:
> ```
> TARE$MGM:2.5E2+ENV$KEL:293.15
> ```

## Introduction

`T-REX` serves as a versatile instrument for expedient ad-hoc transfer of low-dimensional result data within laboratory settings. Distinguished by its emphasis on simplicity over complexity, `T-REX` represents an initial step towards the automation of lab device data exchange. Notably, it facilitates uncomplicated transfers, such as weight values from a balance, while acknowledging its limitations for more intricate tasks like the transfer of Chromatography data sets, which are better suited for employment with [PAC-ID](https://github.com/ApiniLabs/PAC-ID), effectively directing users to the corresponding data record in their Chromatography Data System (CDS).

Laboratory devices typically endure lifecycles of up to 30 years. Despite this longevity, the adoption of standardized APIs remains a gradual process, with widespread implementation potentially spanning decades. However, a substantial number of devices already possess the capability to display QR codes, with ongoing firmware development ensuring their continued relevance. `T-REX` is strategically positioned to target this sizable installed base, offering immediate digitalization benefits within the current laboratory landscape.

Refined from the initial concepts of LabQR and ResultQR by the [SiLA 2 Core Working Group](https://sila-standard.com/standards/), `T-REX` represents the latest evolution in innovation.

## Specification

`T-REX`-formatted data consists of a string of ASCII characters, which is made up of individual `segment`s. These `segment`s are separated by `+`. Each `segment` comprises a `key` and a `value`, distinguished by a colon (`:`). Within the `key`, a `type` and a `unit` are combined using a dollar sign (`$`). The following railroad diagram illustrates this:

![T-REX Railroad Diagram](images/t-rex-railroad-diagram-simple.svg)

*A railroad diagram, showing the simplified EBNF grammar of the T-REX format.*

The railroad diagram above shows a simplified EBNF grammar of the T-REX format. The full grammar can be found further down. For easier readability, the following bullet list provides more details on the `T-REX` format.

- The `vlaue` can be `alphanumeric` or `numeric`:
  - `numeric`: A integer of decimal number. The decimal separator MUST always be `.`. Writing numbers using scientific notation is allowed. Allowed chars are `0-9`, `-`, `.` and `E`. Note that the `+` character is not allowed, as it is already used as separator.
  - `alphanumeric`: An arbitrary text. Allowed characters are `A-Z` (upper case only), numbers `0-9` and the dot `.`.
- The `type` is indicating “what the value refers to”. The best practice is to use a (well-known) English word or abbreviation. Allowed characters `A-Z` (upper case only), `0-9` and the dot `.`. If the type is a 2-4 digit numeric code, it can be assumed that the `type` denotes a [GS1 Application Identifier](https://ref.gs1.org/ai/). Some example `type`s are listed in the table below.
- The `unit` MUST either be a `Unit of Measure Common Code` or a hint to a data type as outlined in the table below. Note, the table only contains some common examples of units from the `Unit of Measure Common Code`; there are many more defined in the complete list[^1].

### Example `type`s

Any sequence of `alphanumeric` characters is allowed as `type`. Some example `type`s are listed in the table below. The best practice is to use a (well-known) English word or abbreviation. The `type` MAY also be a 2-4 digit numeric code based on a [GS1 Application Identifier](https://ref.gs1.org/ai/). The table lists two example GS1 AIs, however, there are many more available.

| `type` | Source | Description |
| :-- | :-- | :-- |
| `TARE` | T-REX (this specification) | A "tare" weight |
| `ENV` | T-REX (this specification) | Environmental (temperature) |
| `START` | T-REX (this specification) | Start (date or time) |
| `DURATION` | T-REX (this specification) | Duration |
| `MODE` | T-REX (this specification) | Mode of operation |
| ... | | |
| `10` | [GS1 AI](https://ref.gs1.org/ai/) | Batch or lot number |
| `21` | [GS1 AI](https://ref.gs1.org/ai/) | Serial number |
| ... | | |

### Valid `unit`s

The `unit` MUST either a `Unit of Measure Common Code` or a hint to a data type as outlined in the table below. Note, the table only contains some common examples of units from the `Unit of Measure Common Code`; there are many more defined in the complete list[^1].

| `unit` | `value` is | Source | Description |
| :-- | :-- | :-- | :-- | 
| `MGM` | `numeric` | Unit of Measure Common Code[^1] | For milligram [10⁻⁶ kg] |
| `CEL` | `numeric` | Unit of Measure Common Code[^1] | For degree celsius, Refer ISO 80000-5 (Quantities and units — Part 5: Thermodynamics) |
| `MLT` | `numeric` | Unit of Measure Common Code[^1] | For millilitre, [10⁻⁶ m³] |
| `GL` | `numeric` | Unit of Measure Common Code[^1] | For gram per litre [g/l] or [kg/m³] |
| `C34` | `numeric` | Unit of Measure Common Code[^1] | For mole [mol] |
| `D43` | `numeric` | Unit of Measure Common Code[^1] | For atomic mass unit [u] or [1,660 538 782 x 10⁻²⁷ kg] |
| `C62` | `numeric` | Unit of Measure Common Code[^1] | For unit-less numbers, use `C62` (unit “one“). |
| ... |  |  | ... |
| ... |  |  | ... many more units are defined in Unit of Measure Common Code[^1] ... |
| ... |  |  | ... |
| `T.D` | `alphanumeric` | T-REX (this specification) | For date and time followed by a `value` in ISO8601 Basic Format further limited to the following options:<ul><li> Date: YYYYMMDD, Example: `START$T.D:20231121`</li><li>Time: THHMM, Example: `START$T.D:T0846`, THHMMSS, Example: `START$T.D:T084659`, THHMMSS.SSS , Example: `START$T.D:T084659.956`</li><li> Timestamp: Any valid date format followed by any valid time format. Example: `START$T.D:20231121T0846`</li><li>Note: Relative time is represented by any suitable unit of measure instead of type `T.D`, Example: `DURATION$SEC:568`</li></ul> |
| `T.B` | `alphanumeric` | T-REX (this specification) | For Booleans followed by `T` (true) or `F` (false) as `value`. Example: `UNDERVACUUM$T.B:T` |
| `T.A` | `alphanumeric` | T-REX (this specification) | For alphanumeric strings of a variable length, limited to the character set `A-Z`, `0-9`, `.`. Example: `METHOD$T.A:HELLOWORLD` |
| `T.X` | `alphanumeric` | T-REX (this specification) | For arbitrary [Base36](https://en.wikipedia.org/wiki/Base36) encoded data. Allowed characters are `A-Z` and `0-9`. Use this as a last resort only. |
| `E` | `alphanumeric` | T-REX (this specification) | For error codes (alphanumeric). This type is meant to be used to indicate errors for expected `key`s, e.g. if a `TEMP$KEL` is not available because the corresponding sensor was unplugged, `TEMP$T.E:NC` could be used. |
| `X.` | `alphanumeric` | T-REX (this specification) | `X.`-prefixed codes are reserved for future extensions. |

[^1]: Unit of Measure Common Code as defined by UN/CEFACT in REC 20 ([https://unece.org/trade/uncefact/cl-recommendations](https://unece.org/trade/uncefact/cl-recommendations) > REC20 > Latest Revision > Column “CommonCode“ of Annexes I-III Excel File) 



## Examples

Example `T-REX`, containing a tare weight (250 mg) and an environmental temperature (293.15 K):
```
TARE$MGM:2.5E2+ENV$KEL:293.15
```

Example `T-REX`, containing a start date (22 FEB 2024 17:48), a duration (0 min) and a mode ("MANUAL"):
```
START$T.D:20240222T1748+DURATION$MIN:0+MODE$T.A:MANUAL
```

## Full EBNF Grammar of the T-REX Format

The following section contains the grammar for the T-REX format in [EBNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form) syntax:

```
trex         = segment , { "+" , segment };
segment      = key, ":", value ;
key          = type, "$", unit ;
value        = number | alphanumeric ;
type         = alphanumeric ;
unit         = alphanumeric ;
number       = decimal | scientific ;
decimal      = integer | ( ["-"] , {digit} , "." , digit , {digit} ) ;
scientific   = decimal , "E" , integer ;
integer      = ["-"] , digit, {digit} ;
digit        = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;
alphanumeric = { letter | digit | "." } ;
letter       = "A" | "B" | "C" | "D" | "E" | "F" | "G"
             | "H" | "I" | "J" | "K" | "L" | "M" | "N"
             | "O" | "P" | "Q" | "R" | "S" | "T" | "U"
             | "V" | "W" | "X" | "Y" | "Z" ;
```




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
