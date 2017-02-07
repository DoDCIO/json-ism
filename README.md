# JSON Information Security Marking Standard (JSON-ISM) --- DRAFT

## Introduction

Department of Defense (DoD) policy requires the identification and protection of national security information and controlled unclassified information (CUI).  Department of Defense Manual (DoDM) 5200.01, Volumes 2 and 4 (referenced below) describe how to appropriately mark classified information and CUI to facilitate information sharing.  These markings are used (along with other factors) to make access/dissemination decisions.

Exstensible Markup Language (XML) is widely used within the DoD to share information and a comprehensive marking standard called Information Security Marking Metadata (ISM or IC-ISM) has been made available by the Office of the Director of National Intelligence (referenced below).  We are not aware of any similar marking standards based on JavaScript Object Notation (JSON).  With the ever increasing popularity of REST APIs for sharing information between applications/systems, many now using JSON over XML, a standard for marking JSON documents has become necessary.  The JSON Information Security Marking Standard (JSON-ISM) aims to be that standard.

## References

* [DoDM 5200.01, Volume 2 - DoD Information Security Program: Marking of Classified Information](http://www.dtic.mil/whs/directives/corres/pdf/520001_vol2.pdf)
* [DoDM 5200.01, Volume 4 - DoD Information Security Program: Controlled Unclassified Information (CUI)](http://www.dtic.mil/whs/directives/corres/pdf/520001_vol4.pdf)
* [Office of the Director of National Intelligence - Information Security Marking Metadata (ISM)](https://www.dni.gov/index.php/about/organization/chief-information-officer/information-security-marking-metadata)

## Overview

JSON-ISM facilitates marking information based upon the concept of Banner Lines (Resource Level) and Portion Markings (Portion Level) as described in the DoDM 5200.01 volumes.  JSON-ISM provides the attributes necessary for consumers to construct Banner Lines and Portion Markings to be used when displaying the information.  In addition, these attributes can be used when making access/dissemination decisions.  Where possible, JSON-ISM attempts to align with the IC-ISM standard.

JSON-ISM is only a marking standard.  It remains the responsibility of the data owner(s) to ensure information is appropriately marked and those in possession of classified information or CUI are responsible for its protection.

## Distribution Notice

All data contained within this repo (including this file) is `UNCLASSIFIED`.  Classification markings are for illustration purposes only.

## ISM Object

ISM attributes will be grouped together to form an object.  This object can be used at both the resource level (overall document) and the portion level (individual attribute).  

The object is identified at the resource level using the `ism` key.  This key is placed at the root level of the JSON document.

**Marking at the resource level (equivalent to Banner Line for overall document classification)**
```json
{
  "ism":{
    "version": "1",
    "classification": "U",
    "ownerProducer": "USA"
  },
  "data":{
    ...
  }
}
```

The object is identified at the portion level using the attribute name plus the `Ism` key suffix.  For example, if the key for the portion we wish to mark is `description`, the key for the ISM object would be `descriptionIsm`.

**Marking at the portion level (equivalent to Portion Marking for identifying portions within a document)**
```json
{
  ...
  "data":{
    "programName": "Nonsensitive Program Name",
    "description": "Sensitive Description",
    "descriptionIsm": {
      "classification": "U",
      "ownerProducer": "USA"
    },
    ...
  }
}
```

## ISM Based on Classification

This sections describes which attributes of the ism object are required based on the sensitivity of the information.

### Unclassified Information

When dealing with unclassified information that has no limitations on dissemination, all ISM attributes are optional.  It is recommended however to include `version`, `classification`, and `ownerProducer` at the resource level.

### Classified Information

When dealing with classified information, the `classification` and `ownerProducer` attributes are required.  All other attributes are optional.

## Attributes

The following is a list of ISM object attributes.  These attributes provide the information necessary to construct Banner Lines and Portion Markings.

### JSON-ISM Version

The `version` attribute (String) identifies the version of JSON-SMS being used (for the entire document).  This attribute is only used at the resource level.

### Classification

The `classification` attribute (String) is used to identify the highest classification of information included within a document (Banner Line - Resource Level) or within a given portion of a document (Portion Marking - Portion Level).  This attribute is always used in conjunction with the `ownerProducer` attribute.  Taken together, these two attributes specify the classification category and type of classification (US, non-US, or Joint).

The possible values for `classification` are:

| Value | Description |
| ----- | ----------- |
| R | RESTRICTED |
| C | CONFIDENTIAL |
| S | SECRET |
| TS | TOP SECRET |
| U | UNCLASSIFIED |

**Example**
```json
  "classification": "U"
```

### Owner Producer

The `ownerProducer` attribute (Array[String]) is used to identify one or more national governments or international organizations that have purview over the classification marking of a resource or portion therein. This attribute is always used in conjunction with the `classification` attribute.  Taken together, these two attributes specify the classification category and type of classification (US, non-US, or Joint).

**Example**
```json
  "ownerProducer": [
    "USA"
  ]
```

### Joint

The `joint` attribute (Boolean), when true, is used to signify that multiple values in the `ownerProducer` attribute are JOINT owners of the data.

**Example**
```json
  "joint": true
```

### SCI Controls

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

**Example**
```json
  "sciControls": [
    "RSV-540"
  ]
```

### SAR Identifiers

The `sarIdentifiers` attribute (Array[String]) identifies one or more defense or intelligence programs for which special access is required.

The possible patterns for `sarIdentifiers` are:

| Pattern | Description |
| ------- | ----------- |
| [A-Z\s0-9\-]{1,100} | SPECIAL ACCESS REQUIRED-XXX, the Digraph or Trigraph of the SAR is represented by the XXX |
| [A-Z]{2,} | SPECIAL ACCESS REQUIRED-XXX, the Digraph or Trigraph of the SAR is represented by the XXX |
| [A-Z]{2,}-[A-Z][A-Z0-9]+ | SPECIAL ACCESS REQUIRED-XXX, the Digraph or Trigraph of the SAR is represented by the XXX |
| [A-Z]{2,}-[A-Z][A-Z0-9]+-[A-Z0-9]{2,} | SPECIAL ACCESS REQUIRED-XXX, the Digraph or Trigraph of the SAR is represented by the XXX |

### Atomic Energy Markings

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

### Dissemination Controls

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

### Display Only To

The `displayOnlyTo` attribute (Array[String]) identifies one or more countries and/or international organizations to which classified information may be displayed but NOT released based on the determination of an originator in accordance with established foreign disclosure procedures. This attribute is used in conjunction with the `DISPLAYONLY` value of the `disseminationControls` attribute.

The possible values for `displayOnlyTo` are:

| Value | Description |
| ----- | ----------- |

### FGI Source Open

The `fgiSourceOpen` attribute (Array[String]) identifies information which qualifies as foreign government information for which the source(s) of the information is not concealed. The attribute can indicate that the source of information of foreign origin is `UNKNOWN`.

The possible values for `fgiSourceOpen` are:

| Value | Description |
| ----- | ----------- |

### FGI Source Protected

The `fgiSourceProtected` attribute (Array[String]) has unique specific rules concerning its usage. A single indicator that information qualifies as foreign government information for which the source(s) of the information must be concealed. Within protected internal organizational spaces this element may be used to maintain a record of the one or more indicators identifying information which qualifies as foreign government information for which the source(s) of the information must be concealed. Measures must be taken prior to dissemination of the information to conceal the source(s) of the foreign government information. An indication that information qualifies as foreign government information according to CAPCO guidelines for which the source(s) of the information must be concealed when the information is disseminated in shared spaces This data element has a dual purpose. Within shared spaces, the data element serves only to indicate the presence of information which is categorized as foreign government information according to CAPCO guidelines for which the source(s) of the information is concealed, in which case, this data element's value will always be `FGI`. The data element may also be employed in this manner within protected internal organizational spaces. However, within protected internal organizational spaces this data element may alternatively be used to maintain a formal record of the foreign country or countries and/or registered international organization(s) that are the non-disclosable owner(s) and/or producer(s) of information which is categorized as foreign government information according to CAPCO guidelines for which the source(s) of the information must be concealed when the resource is disseminated to shared spaces. If the data element is employed in this manner, then additional measures must be taken prior to dissemination of the resource to shared spaces so that any indications of the non-disclosable owner(s) and/or producer(s) of information within the resource are eliminated. In all cases, the corresponding portion marking or banner marking should be compliant with CAPCO guidelines for FGI when the source must be concealed. In other words, even if the data element is being employed within protected internal organizational spaces to maintain a formal record of the non-disclosable owner(s) and/or producer(s), if the resource is rendered for display within the protected internal organizational spaces in any format by a stylesheet or as a result of any other transformation process, then the non-disclosable owner(s) and/or producer(s) should not be included in the corresponding portion marking or banner marking.

The possible values for `fgiSourceProtected` are:

| Value | Description |
| ----- | ----------- |

### Releasable To

The `releasableTo` attribute (Array[String]) identifies one or more countries and/or international organizations to which classified information may be released based on the determination of an originator in accordance with established foreign disclosure procedures.  This attribute is used in conjunction with the `disseminationControls` attribute.

The possible values for `releasableTo` are:

| Value | Description |
| ----- | ----------- |

### Non-IC Markings

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

### Classified By

The `classifiedBy` attribute (String) is used primarily at the resource level. The identity, by name or personal identifier, and position title of the original classification authority for a resource. It is manifested only in the 'Classified By' line of a resource's classification authority block.

### Compilation Reason

The `compilationReason` attribute (String) is a description of the reasons that the classification of this element is more restrictive than a simple roll-up of the sub elements would result in. This acts as an indicator to rule engines that there is not accidental over classification going on and to users that special care beyond what the portion marks reveal must be taken when using this data. Use of this mark does not replace the need for the compilation reason being defined in the prose in accordance with ISOO Directive 1. For example this would document why 3 Unclassified bullet items form a Secret List. Without this reason being noted the above described document would be considered to be miss-marked and overclassified.

### Derivatively Classified By

The `derivativelyClassifiedBy` attribute (String) is used primarily at the resource level. The identity, by name or personal identifier, of the derivative classification authority. It is manifested only in the 'Classified By' line of a resource's classification authority block.

### Classification Reason

The `classificationReason` attribute (String) is used primarily at the resource level. One or more reason indicators or explanatory text describing the basis for an original classification decision. It is manifested only in the 'Reason' line of a resource's classification authority block.

### Non-US Controls

The `nonUSControls` attribute (Array[String]) identifies one or more indicators for expanding or limiting the distribution of information originating from non-US components.

The possible values for `nonUSControls` are:

| Value | Description |
| ----- | ----------- |
| ATOMAL | NATO Atomal mark |
| BOHEMIA | NATO Bohemia mark |
| BALK | NATO Balk mark |

### Derived From

The `derivedFrom` attribute (String) is used primarily at the resource level. A citation of the authoritative source or reference to multiple sources of the classification markings used in a classified resource. It is manifested only in the 'Derived From' line of a document's classification authority block. ISOO's guidance is: Source of derivative classification. (1) The derivative classifier shall concisely identify the source document or the classification guide on the ‘‘Derived From’’ line, including the agency and, where available, the office of origin, and the date of the source or guide. An example might appear as: Derived From: Memo, ‘‘Funding Problems,’’ October 20, 2008, Office of Administration, Department of Good Works or Derived From: CG No. 1, Department of Good Works, dated October 20, 2008 (i) When a document is classified derivatively on the basis of more than one source document or classification guide, the ‘‘Derived From’’ line shall appear as: Derived From: Multiple Sources (ii) The derivative classifier shall include a listing of the source materials on, or attached to, each derivatively classified document.

### Declassification Date

The `declassDate` attribute (Date) is used primarily at the resource level. A specific year, month, and day upon which the information shall be automatically declassified if not properly exempted from automatic declassification. It is manifested in the 'Declassify On' line of a resource's classification authority block.

### Declassification Event

The `declassEvent` attribute (String) is used primarily at the resource level. A description of an event upon which the information shall be automatically declassified if not properly exempted from automatic declassification. It is manifested only in the 'Declassify On' line of a resource's classification authority block.

### Declassification Exception

The `declassException` attribute (String) is used primarily at the resource level. A single indicator describing an exemption to the nominal 25-year point for automatic declassification. This element is used in conjunction with the `declassDate` or `declassEvent`. It is manifested in the 'Declassify On' line of a resource's classification authority block. ISOO has stated it should be a SINGLE value giving the longest protection.
