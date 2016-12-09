# JSON Security Marking Standard (JSON-SMS) --- DRAFT

## Introduction

Department of Defense (DoD) policy requires the identification and protection of national security information and controlled unclassified information (CUI).  Department of Defense Manual (DoDM) 5200.01, Volumes 2 and 4 (referenced below) describe how to appropriately mark classified information and CUI to facilitate information sharing.  These markings are used (along with other factors) to make access/dissemination decisions.

Exstensible Markup Language (XML) is widely used within the DoD to share information and a comprehensive marking standard called Information Security Marking Metadata (ISM or IC-ISM) has been made available by the Office of the Director of National Intelligence (referenced below).  We are not aware of any similar marking standards based on JavaScript Object Notation (JSON).  With the ever increasing popularity of REST APIs for sharing information between applications/systems, many now using JSON over XML, a standard for marking JSON documents has become necessary.  The JSON Security Marking Standard (JSON-SMS) aims to be that standard.

## References

* [DoDM 5200.01, Volume 2 - DoD Information Security Program: Marking of Classified Information](http://www.dtic.mil/whs/directives/corres/pdf/520001_vol2.pdf)
* [DoDM 5200.01, Volume 4 - DoD Information Security Program: Controlled Unclassified Information (CUI)](http://www.dtic.mil/whs/directives/corres/pdf/520001_vol4.pdf)
* [Office of the Director of National Intelligence - Information Security Marking Metadata (ISM)](https://www.dni.gov/index.php/about/organization/chief-information-officer/information-security-marking-metadata)

## Overview

JSON-SMS facilitates marking information based upon the concept of Banner Lines (Resource Level) and Portion Markings (Portion Level) as described in the DoDM 5200.01 volumes.  JSON-SMS provides the attributes necessary for consumers to construct Banner Lines and Portion Markings to be used when displaying the information.  In addition, these attributes can be used when making access/dissemination decisions.  Where possible, JSON-SMS attempts to align with the ISM standard.

JSON-SMS is only a marking standard.  It remains the responsibility of the data owner(s) to ensure information is appropriately marked and those in possession of classified information or CUI are responsible for its protection.

## JSMS Object

JSON-SMS attributes will be grouped together to form an object.  This object can be used at both the resource level (overall document) and the portion level (individual attribute).  

The object is identified at the resource level using the `jsms` key.  This key is placed at the root level of the JSON document.

** Marking at the resource level (equivalent to Banner Line for overall document classification) **
```json
{
  "jsms":{
    -- JSON-SMS Attributes --
  },
  "data":{
    ...
  }
}
```

The object is identified at the portion level using the attribute name plus the `_jsms` key suffix.  For example, if the key for the portion we wish to mark is `description`, the key for the JSMS object would be `description_jsms`.

** Marking at the portion level (equivalent to Portion Marking for identifying portions within a document) **
```json
{
  ...
  "data":{
    "programName": "Nonsensitive Program Name",
    "description": "Sensitive Description",
    "description_jsms": {
      -- JSON-SMS Attributes --
    },
    ...
  }
}
```

### Attributes

The following is a list of JSMS object attributes.  These attributes provide the information necessary to construct Banner Lines and Portion Markings.

#### JSON-SMS Version

The `version` attribute identifies the version of JSON-SMS being used.

#### Classification

The `classification` attribute (String) is used to identify the highest classification of information included within a document (Banner Line - Resource Level) or within a given portion of a document (Portion Marking - Portion Level).  This attribute is always used in conjunction with the `ownerProducer` attribute.  Taken together, these two attributes specify the classification category and type of classification (US, non-US, or Joint).

The possible values for `classification` are:

| Value | Description |
| ----- | ----------- |
| R | RESTRICTED |
| C | CONFIDENTIAL |
| S | SECRET |
| TS | TOP SECRET |
| U | UNCLASSIFIED |

** Example **
```json
  "classification": "U"
```

#### Owner Producer

The `ownerProducer` attribute (Array[String]) is used to identify one or more national governments or international organizations that have purview over the classification marking of a resource or portion therein. This attribute is always used in conjunction with the `ownerProducer` attribute.  Taken together, these two attributes specify the classification category and type of classification (US, non-US, or Joint).

** Example **
```json
  "ownerProducer": [
    "USA"
  ]
```

#### SCI Controls

The `sciControls` attribute (Array[String]) identifies one or more sensitive compartmented information control systems.

The possible patterns for `sciControls` are:

| Pattern | description |
| ------- | ----------- |
| KDK-BLFH-[A-Z0-9]{1,6} | KDK-BLFH-xxxxxx, xxxxxx represents up to 6 alphanumeric characters indicating a sub BLUEFISH compartment |
| KDK-IDIT-[A-Z0-9]{1,6} | KDK-IDIT-xxxxxx, xxxxxx represents up to 6 alphanumeric characters indicating a sub IDITAROD compartment |
| KDK-KAND-[A-Z0-9]{1,6} | KDK-KAND-xxxxxx, xxxxxx represents up to 6 alphanumeric characters indicating a sub KANDIK compartment |
| RSV-[A-Z0-9]{3} | RSV-XXX, XXX represents 3 alpha numeric characters to indicate sub Reserve compartments |
| SI-G-[A-Z]{4} | G-AAAA, AAAA represents 4 alpha characters to indicate sub Gamma compartments |
| SI-[A-Z]{3} | SPECIAL INTELLIGENCE compartment |
| SI-[A-Z]{3}-[A-Z]{4} | SPECIAL INTELLIGENCE sub-compartment |

The possible values for `sciControls` are:

| Value | Description |
| ----- | ----------- |
| EL | ENDSEAL |
| EL-EU | ECRU |
| EL-NK | NONBOOK |
| HCS | HCS |
| HCS-O | HCS-O |
| HCS-P | HCS-P |
| KDK | KLONDIKE |
| KDK-BLFH | KDK BLUEFISH |
| KDK-IDIT | KDK IDITAROD |
| KDK-KAND | KDK KANDIK |
| RSV | RESERVE |
| SI | SPECIAL INTELLIGENCE |
| SI-G | SI-GAMMA |
| TK | TALENT KEYHOLE |

** Example **
```json
  "sciControls": [
    "RSV-540"
  ]
```

#### SAR Identifiers

The `sarIdentifiers` attribute (Array[String]) identifies one or more defense or intelligence programs for which special access is required.

The possible patterns for `sarIdentifiers` are:

| Pattern | Description |
| ------- | ----------- |
| [A-Z\s0-9\-]{1,100} | SPECIAL ACCESS REQUIRED-XXX, the Digraph or Trigraph of the SAR is represented by the XXX |
| [A-Z]{2,} | SPECIAL ACCESS REQUIRED-XXX, the Digraph or Trigraph of the SAR is represented by the XXX |
| [A-Z]{2,}-[A-Z][A-Z0-9]+ | SPECIAL ACCESS REQUIRED-XXX, the Digraph or Trigraph of the SAR is represented by the XXX |
| [A-Z]{2,}-[A-Z][A-Z0-9]+-[A-Z0-9]{2,} | SPECIAL ACCESS REQUIRED-XXX, the Digraph or Trigraph of the SAR is represented by the XXX |

#### Atomic Energy Markings

The `atomicEnergyMarkings` attribute (Array[String]) identifies one or more Department of Energy (DoE) markings.

The possible patterns for `atomicEnergyMarkings` are:

| Pattern | Description |
| ------- | ----------- |
| RD-SG-((14)&#124;(15)&#124;(18)&#124;(20)) | RD-SIGMA-#, # represents the SIGMA number which may be 14, 15, 18, or 20 |
| FRD-SG-((14)&#124;(15)&#124;(18)&#124;(20)) | FRD-SIGMA-#, # represents the SIGMA number which may be 14, 15, 18, or 20 |

The possible values for `atomicEnergyMarkings` are:

| Value | Description |
| ----- | ----------- |
| RD | RESTRICTED DATA |
| RD-CNWDI | RD-CRITICAL NUCLEAR WEAPON DESIGN INFORMATION |
| FRD | FORMERLY RESTRICTED DATA |
| DCNI | DoD CONTROLLED NUCLEAR INFORMATION |
| UCNI | DoE CONTROLLED NUCLEAR INFORMATION |
| TFNI | TRANSCLASSIFIED FOREIGN NUCLEAR INFORMATION |

#### Dissemination Controls

The `disseminationControls` attribute (Array[String]) identifies one or more indicators for expanding or limiting the distribution of information.

The possible values for `disseminationControls` are:

| Value | Description |
| ----- | ----------- |
| RS | RISK SENSITIVE |
| FOUO | FOR OFFICIAL USE ONLY |
| OC | ORIGINATOR CONTROLLED |
| OC-USGOV | ORIGINATOR CONTROLLED US GOVERNMENT |
| IMC | CONTROLLED IMAGERY |
| NF | NOT RELEASABLE TO FOREIGN NATIONALS |
| PR | CAUTION-PROPRIETARY INFORMATION INVOLVED |
| REL | AUTHORIZED FOR RELEASE TO |
| RELIDO | RELEASABLE BY INFORMATION DISCLOSURE OFFICIAL |
| EYES | EYES ONLY |
| DSEN | DEA SENSITIVE |
| FISA | FOREIGN INTELLIGENCE SURVEILLANCE ACT |
| DISPLAYONLY | AUTHORIZED FOR DISPLAY BUT NOT RELEASE TO |

#### Non-IC Markings

The `nonICmarkings` attribute (Array[String]) identifies one or more indicators for expanding or limiting the distribution of information originating from non-intelligence components.

The possible patterns for `nonICmarkings` are:

| Pattern | Description |
| ------- | ----------- |
| ACCM-[A-Z0-9\-_]{1,61} | The name of the ALTERNATE COMPENSATORY CONTROL MEASURE, substituting "_" for a space |
| NNPI | NAVAL NUCLEAR PROPULSION INFORMATION |

The possible values for `nonICmarkings` are:

| Value | Description |
| ----- | ----------- |
| DS | LIMITED DISTRIBUTION |
| XD | EXCLUSIVE DISTRIBUTION |
| ND | NO DISTRIBUTION |
| SBU | SENSITIVE BUT UNCLASSIFIED |
| SBU-NF | SENSITIVE BUT UNCLASSIFIED NOFORN |
| LES | LAW ENFORCEMENT SENSITIVE |
| LES-NF | LAW ENFORCEMENT SENSITIVE NOFORN |
| SSI | SENSITIVE SECURITY INFORMATION |
