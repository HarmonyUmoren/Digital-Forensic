# DIGITAL FORENSIC EXAMINATION REPORT
Forensic Analysis of a USB Storage Device
Forensic Image Reference: cartel.img
CASE EXERCISE: CAPTURE THE FLAG

# Digital Forensics Examination Report

| **Field**                 | **Details**                                                            |
| ------------------------- | ---------------------------------------------------------------------- |
| **Prepared By**           | Harmony (Digital Forensic Examiner)                                    |
| **Module**                | Digital Forensics and Incident Response                                |
| **Date of Examination**   | 3 August 2026                                                          |
| **Evidence Seized**       | 2 August 2026, 09:15 AM, by EFCC officers pursuant to a search warrant |
| **Evidence Type**         | USB storage device (forensic image: `cartel.img`)                      |
| **Report Classification** | Confidential — For Authorized Recipients Only                          |

## 1. introduction 
This report presents the findings of a digital forensic examination conducted on a forensic image (cartel.img) acquired from a USB storage device recovered by the Economic and Financial Crimes Commission (EFCC) on 2 August 2026, pursuant to a search warrant. The device was seized as part of a broader investigation, and the suspect associated with the device stated that it “only contains personal photographs.”
The purpose of this examination was to test that claim against verifiable forensic evidence obtained from the image itself. This report documents the identification, preservation, examination, and analysis of the digital evidence contained within the image, and presents findings strictly on the basis of what was recovered and verified during the examination. No determination of guilt or innocence is made or implied by this report.
Key findings are summarized below and are described in full detail in the corresponding sections of this report:
- Integrity of the forensic image was verified using MD5 and SHA-256 hashing prior to examination (Section 5).
- Initial triage identified the image as a raw (dd-style), unpartitioned FAT16 volume, approximately 247.5 MB in size (Section 6).
- Two allocated text files (GUMBO1.TXT and GUMBO2.TXT), containing recipes, were the only user-accessible files present on the volume at the time of examination. Their presence does not support the suspect's statement that the device contained only personal photographs (Section 7, Artifact 1).
- Approximately 208 MB — more than 80% of the storage device — was found to contain two large regions of repetitive ASCII text (“SORRY” and “CHARLIE”), consistent with an intentional overwrite of previously stored data (Section 7, Artifact 2).
- A surviving, unaffected region of the image yielded six JPEG images and two GIF images through file carving. None of the recovered images contained camera-generated EXIF or GPS metadata; embedded comments in two images were consistent with generic or stock graphics rather than personal photographs (Section 7, Artifact 3).
- A recovered first-person text fragment, located within the CHARLIE-overwritten region, references the prior destruction of a hard drive and a stated intention to reformat the USB device under examination (Section 7, Artifact 4).
- Hash comparison identified two byte-for-byte identical JPEG files recovered from disk locations over 117 MB apart, indicating the same file existed on the device on more than one occasion or prior to both overwrite events (Section 7, Artifact 5).
- A total of nine deleted files were recovered from unallocated space through file carving (Section 8).
- A relative event timeline was reconstructed from the limited verifiable evidence available; however, an absolutely-dated timeline could not be established from the image alone, and specific additional evidence would be required to do so (Section 9 and Section 10).
Collectively, the evidence recovered from the forensic image is inconsistent with the suspect's statement that the device contained only personal photographs. This report does not draw any conclusion beyond what is directly supported by the recovered evidence, and clearly identifies areas where information was not available within the scope of this examination.

## Case Background and Scope
# 1. Case Information

| **Field**                   | **Details**                                                                   |
| --------------------------- | ----------------------------------------------------------------------------- |
| **Examiner**                | Harmony (Digital Forensic Examiner)                                           |
| **Module**                  | Digital Forensics and Incident Response                                       |
| **Date of Examination**     | 3 August 2026                                                                 |
| **Evidence Seized**         | 2 August 2026, 09:15 AM                                                       |
| **Seizing Authority**       | Economic and Financial Crimes Commission (EFCC), pursuant to a search warrant |
| **Evidence Description**    | USB storage device                                                            |
| **Forensic Image Analyzed** | `cartel.img`                                                                  |

# Objective
This examination was conducted to identify, preserve, examine, analyze, and document digital evidence contained within the forensic image (cartel.img) acquired from a USB storage device recovered during an EFCC search operation on 2 August 2026. The suspect claimed the device “only contains personal photographs.” The objective of this examination was to test this claim against verifiable forensic evidence, without making any determination of guilt or innocence.

# Scope
This examination was limited to the forensic image cartel.img as provided. No physical device, host computer, network evidence, or additional storage media was made available to, or examined by, this examiner. Any conclusions in this report are therefore limited to what can be directly established from the content of this single forensic image. Where information necessary to answer a specific question was not available within the scope of this examination, that limitation is explicitly stated rather than inferred or assumed.

# Methodology and Tools Used
The examination followed standard digital forensic principles of preservation, verification, and documentation, working from a forensic image rather than the original physical media, in order to avoid any modification of the source evidence. The following tools were used during this examination:
- md5sum — cryptographic hash verification (MD5)
- sha256sum — cryptographic hash verification (SHA-256)
- mmls — partition table / volume system analysis (The Sleuth Kit)
- img_stat — forensic image metadata analysis (The Sleuth Kit)
- fsstat — file system structure analysis (The Sleuth Kit)
- fls — recursive file and directory listing (The Sleuth Kit)
- icat / istat — file content and metadata extraction (The Sleuth Kit)
- blkls — unallocated space extraction (The Sleuth Kit)
- Python (byte-level scripting) — offset-level analysis of raw image content
- Binwalk — file signature detection within unallocated space
- Foremost — signature-based file carving
- ImageMagick (identify) — image file validation
- FTK Imager and Autopsy were listed as optional tools; their use in the production of the findings in this report is not evidenced and they are noted here for completeness only.
All commands and their outputs referenced throughout this report were captured directly from the examination and are reproduced in the relevant sections and supporting figures.

## Chain of Custody
The forensic image cartel.img was derived from a USB storage device seized by EFCC officers on 2 August 2026 at 09:15 AM, pursuant to a search warrant. Examination of the image was conducted on 3 August 2026. Beyond the seizure date, seizing authority, and examination date stated above, no further chain-of-custody documentation (e.g., evidence transfer logs, storage location, or intermediate custodians) was provided as part of this exercise; this report makes no assumptions regarding those details.

## Evidence Verification
- Purpose
Integrity verification was performed by calculating the MD5 and SHA-256 hash values of the forensic image (cartel.img) prior to forensic examination. The purpose of this process is to ensure that the forensic image has not been modified during storage or analysis. The calculated hash values provide a digital fingerprint of the evidence and serve as a reference for verifying its authenticity throughout the investigation.

## Hash Values Calculated
# Hash Values Calculated
| **File**     | **MD5 Hash**                       | **SHA-256 Hash**                                                   |
| ------------ | ---------------------------------- | ------------------------------------------------------------------ |
| `cartel.img` | `80348c58eec4c328ef1f7709adc56a54` | `ce550424200a997c61b413941c8ef4df9619a2f96579674952294a176a32be65` |

## Why Matching Hashes Matter
Matching hash values confirm that the forensic image has not been altered during the examination. This preserves the integrity and authenticity of the digital evidence and demonstrates that the analysis was conducted on an exact, unmodified copy of the original data. Any difference in hash values would indicate possible alteration or corruption of the evidence, requiring further investigation before the findings could be considered reliable.
The hash values recorded above represent the values calculated for the image as examined. No independent, previously-recorded reference hash (e.g., a value generated at the point of acquisition by the seizing officers) was provided for direct comparison; consequently, this report records the calculated values as a verification baseline for this examination rather than as a confirmed match against an external record.
<img width="750" height="209" alt="image" src="https://github.com/user-attachments/assets/940a1389-1c38-44c6-91b3-6e5973886164" />

# Initial Triage
| **Attribute**              | **Details**                                                                                                            |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Image Format**           | Raw (dd-style) disk image                                                                                              |
| **File System Type**       | FAT16                                                                                                                  |
| **Number of Partitions**   | 0 — no MBR partition table detected (unpartitioned “superfloppy” volume; the whole disk is a single FAT16 file system) |
| **Total Size**             | 259,506,176 bytes (≈ 247.5 MB)                                                                                         |
| **Sector Size**            | 512 bytes                                                                                                              |
| **Cluster Size**           | 4,096 bytes (8 sectors)                                                                                                |
| **OEM Name (Boot Sector)** | `mkdosfs` — indicates the volume was originally formatted with the Linux dosfstools utility                            |
| **Volume Serial Number**   | `0x4092D9D1`                                                                                                           |
| **Volume Label**           | (none set)                                                                                                             |

## Supporting Command Output
| **Forensic Command**                   | **Key Output / Findings**                                                                                                                                                                           |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`mmls cartel.img`**                  | **Result:** No output returned.<br>**Finding:** No MBR partition table detected. The forensic image is an unpartitioned ("superfloppy") FAT16 volume.                                               |
| **`img_stat cartel.img`**              | **Image Type:** Raw<br>**Image Size:** 259,506,176 bytes (≈247.5 MB)<br>**Sector Size:** 512 bytes                                                                                                  |
| **`fsstat cartel.img`**                | **File System Type:** FAT16<br>**OEM Name:** `mkdosfs`<br>**Volume ID:** `0x4092d9d1`<br>**File System Type Label:** FAT16                                                                          |
| **File System Layout (from `fsstat`)** | **Total Sector Range:** 0–506,847<br>**Reserved Area:** 0–0<br>**FAT 0:** 1–248<br>**FAT 1:** 249–496<br>**Data Area:** 497–506,847<br>**Root Directory:** 497–528<br>**Cluster Area:** 529–506,840 |

## Why These Preliminary Steps Are Necessary
Initial triage helps the examiner understand the structure and characteristics of the forensic image before performing a detailed investigation. It identifies the image format, file system, partition layout, and storage information, allowing the examiner to choose the correct forensic tools and examination methods. Performing this step reduces the risk of analysing the wrong data, misinterpreting the file system, or producing inaccurate results. It also provides a clear understanding of how the evidence is organized, making subsequent analysis and data recovery more effective.

# Evidence Discovery
Artifact 1: GUMBO1.TXT and GUMBO2.TXT (Allocated Files)

## Description
The FAT16 file system contained two allocated text files named GUMBO1.TXT and GUMBO2.TXT. These files contain recipes titled “Shrimp and Tasso Gumbo” and “Shrimp and Andouille Sausage Gumbo,” with file sizes of 2,815 bytes and 1,293 bytes respectively.

## Relevance
These were the only user-accessible files present on the USB drive when it was examined. Their presence does not support the suspect's statement that the USB contained only personal photographs, as both files are recipe documents rather than image files.

## Discovery Method
The files were identified by performing a recursive directory listing using fls, after which their contents and metadata were examined using icat and istat.

| **Forensic Command**     | **Key Output / Findings**                                                                                                                                                                                             |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`fls cartel.img`**     | **Allocated Files:**<br>• Directory Entry **4** – `GUMBO1.TXT`<br>• Directory Entry **6** – `GUMBO2.TXT`<br><br>**Virtual File System Entries:**<br>• `$MBR`<br>• `$FAT1`<br>• `$FAT2`<br>• `$OrphanFiles`            |
| **`istat cartel.img 4`** | **Directory Entry:** 4 (Allocated)<br>**File Name:** `GUMBO1.TXT`<br>**Attributes:** File, Archive<br>**File Size:** 2,815 bytes<br>**Created:** 2004-04-30 18:11:20 UTC<br>**Last Written:** 2004-04-30 18:11:20 UTC |
| **`istat cartel.img 6`** | **Directory Entry:** 6 (Allocated)<br>**File Name:** `GUMBO2.TXT`<br>**Attributes:** File, Archive<br>**File Size:** 1,293 bytes<br>**Created:** 2004-04-30 18:11:24 UTC<br>**Last Written:** 2004-04-30 18:11:24 UTC |

## Artifact 2: Extensive ASCII Overwrite Pattern

# Description
Examination of the forensic image revealed two consecutive regions occupying most of the storage medium that contained repetitive, human-readable ASCII strings. The first region, approximately 54.5 MB, consisted of repeated occurrences of the string “SORRY” (offsets approximately 273,920–54,735,350). This was immediately followed by a second region of approximately 148.5 MB containing repeated instances of the string “CHARLIE” (offsets approximately 54,735,360–203,186,168). Combined, these overwrite regions occupied roughly 208 MB, accounting for more than 80% of the 247.5 MB storage device.

# Relevance
Unlike standard file deletion or a quick format — which typically leaves previous file contents recoverable within unallocated space — the presence of large, continuous regions filled with repetitive ASCII text is unusual during normal device operation. The observed overwrite pattern indicates that a significant portion of the storage medium was intentionally overwritten, suggesting an effort to permanently remove or obscure previously stored data before the device was seized.

# Discovery Method
The overwrite pattern was identified through forensic examination of unallocated space using blkls output and direct byte-level analysis at specific offsets with Python scripts. A comprehensive scan of the forensic image confirmed the precise starting and ending offsets of both repetitive text regions, establishing their overall size and distribution across the storage medium.

# Supporting Evidence
| **Supporting Evidence**                    | **Key Findings**                                                                                                      |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| **Python Hex/Data Inspection**             | `data = open('cartel.img','rb').read()` was used to load the forensic image into memory for analysis.                 |
| **Data at Offset 2,200,544**               | `b'SORRY\nSORRY\nSORRY\nSORRY\nSORRY\nSO'` — confirms the presence of repeated **"SORRY"** strings within the image.  |
| **Data at Offset 54,800,000**              | `b'CHARLIE\nCHARLIE\nCHARLIE\nCHARLIE\n'` — confirms the presence of repeated **"CHARLIE"** strings within the image. |
| **Pattern Boundary Analysis (Regex Scan)** | **SORRY**<br>• Occurrences: **8,835,577**<br>• Offset Range: **273,663 – 54,735,350**                                 |
| **Pattern Boundary Analysis (Regex Scan)** | **CHARLIE**<br>• Occurrences: **18,372,608**<br>• Offset Range: **54,735,356 – 203,186,168**                          |


