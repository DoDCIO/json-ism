# JSON Security Marking Standard (JSON-SMS) --- DRAFT

## Introduction

Department of Defense (DoD) policy requires the identification and protection of national security information and controlled unclassified information (CUI).  Department of Defense Manual (DoDM) 5200.01, Volumes 2 and 4 (referenced below) describe how to appropriately mark classified information and CUI to facilitate information sharing.  These markings are used (along with other factors) to make access/dissemination decisions.

Exstensible Markup Language (XML) is widely used within the DoD to share information and a comprehensive marking standard called Information Security Marking Metadata (ISM or IC-ISM) has been made available by the Office of the Director of National Intelligence (referenced below).  We are not aware of any similar marking standards based on JavaScript Object Notation (JSON).  With the ever increasing popularity of REST APIs for sharing information between applications/systems, many now using JSON over XML, a standard for marking JSON documents has become necessary.  The JSON Security Marking Standard (JSON-SMS) aims to be that standard.

## References

* [DoDM 5200.01, Volume 2 - DoD Information Security Program: Marking of Classified Information](http://www.dtic.mil/whs/directives/corres/pdf/520001_vol2.pdf)
* [DoDM 5200.01, Volume 4 - DoD Information Security Program: Controlled Unclassified Information (CUI)](http://www.dtic.mil/whs/directives/corres/pdf/520001_vol4.pdf)
* [Office of the Director of National Intelligence - Information Security Marking Metadata (ISM)](https://www.dni.gov/index.php/about/organization/chief-information-officer/information-security-marking-metadata)

## Overview

JSON-SMS facilitates marking information based upon the concept of Banner Lines (Resource Level) and Portion Markings (Portion Level) as described in the DoDM 5200.01 volumes.  JSON-SMS provides the attributes necessary for consumers to construct Banner Lines and Portion Markings to be used when displaying the information.  In addition, these attributes can be used when making access/dissemination decision.

JSON-SMS is only a marking standard.  It remains the responsibility of the data owner(s) to ensure information is appropriately marked and those in possession of classified information or CUI are responsible for its protection.
