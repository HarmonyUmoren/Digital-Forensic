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
 A relative event timeline was reconstructed from the limited verifiable evidence available; however, an absolutely-dated timeline could not be established from the image alone, and specific additional evidence would be required to do so (Section 9 and Section 10).
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

# Chain of Custody
The forensic image cartel.img was derived from a USB storage device seized by EFCC officers on 2 August 2026 at 09:15 AM, pursuant to a search warrant. Examination of the image was conducted on 3 August 2026. Beyond the seizure date, seizing authority, and examination date stated above, no further chain-of-custody documentation (e.g., evidence transfer logs, storage location, or intermediate custodians) was provided as part of this exercise; this report makes no assumptions regarding those details.

# Evidence Verification
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
<img width="975" height="292" alt="image" src="https://github.com/user-attachments/assets/6b37dc1b-0407-4229-9d7e-179906aa4b82" />
<img width="975" height="719" alt="image" src="https://github.com/user-attachments/assets/13ac8705-97c6-44d1-8209-844b01aa8853" />
<img width="975" height="397" alt="image" src="https://github.com/user-attachments/assets/8f37ad20-3dd1-4b93-81af-0f9703163f1f" />

## Why These Preliminary Steps Are Necessary
Initial triage helps the examiner understand the structure and characteristics of the forensic image before performing a detailed investigation. It identifies the image format, file system, partition layout, and storage information, allowing the examiner to choose the correct forensic tools and examination methods. Performing this step reduces the risk of analysing the wrong data, misinterpreting the file system, or producing inaccurate results. It also provides a clear understanding of how the evidence is organized, making subsequent analysis and data recovery more effective.

# Evidence Discovery
Artifact 1: GUMBO1.TXT and GUMBO2.TXT (Allocated Files)

# Description
The FAT16 file system contained two allocated text files named GUMBO1.TXT and GUMBO2.TXT. These files contain recipes titled “Shrimp and Tasso Gumbo” and “Shrimp and Andouille Sausage Gumbo,” with file sizes of 2,815 bytes and 1,293 bytes respectively.

# Relevance
These were the only user-accessible files present on the USB drive when it was examined. Their presence does not support the suspect's statement that the USB contained only personal photographs, as both files are recipe documents rather than image files.

# Discovery Method
The files were identified by performing a recursive directory listing using fls, after which their contents and metadata were examined using icat and istat.

| **Forensic Command**     | **Key Output / Findings**                                                                                                                                                                                             |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`fls cartel.img`**     | **Allocated Files:**<br>• Directory Entry **4** – `GUMBO1.TXT`<br>• Directory Entry **6** – `GUMBO2.TXT`<br><br>**Virtual File System Entries:**<br>• `$MBR`<br>• `$FAT1`<br>• `$FAT2`<br>• `$OrphanFiles`            |
| **`istat cartel.img 4`** | **Directory Entry:** 4 (Allocated)<br>**File Name:** `GUMBO1.TXT`<br>**Attributes:** File, Archive<br>**File Size:** 2,815 bytes<br>**Created:** 2004-04-30 18:11:20 UTC<br>**Last Written:** 2004-04-30 18:11:20 UTC |
| **`istat cartel.img 6`** | **Directory Entry:** 6 (Allocated)<br>**File Name:** `GUMBO2.TXT`<br>**Attributes:** File, Archive<br>**File Size:** 1,293 bytes<br>**Created:** 2004-04-30 18:11:24 UTC<br>**Last Written:** 2004-04-30 18:11:24 UTC |
<img width="975" height="214" alt="image" src="https://github.com/user-attachments/assets/46535b84-92ef-4ad5-a8eb-346e8e095666" />
<img width="975" height="290" alt="image" src="https://github.com/user-attachments/assets/9595b0e7-4d26-4904-bab2-e7c521b5850f" />
<img width="975" height="283" alt="image" src="https://github.com/user-attachments/assets/f0fcbc77-2a01-40db-aef8-a545863d433b" />

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
<img width="975" height="189" alt="image" src="https://github.com/user-attachments/assets/6720979e-a9d7-4687-ab86-580f1e6a847c" />
<img width="975" height="222" alt="image" src="https://github.com/user-attachments/assets/2ad18c75-2e4b-4c83-9bc3-115cecab0f3d" />
<img width="975" height="256" alt="image" src="https://github.com/user-attachments/assets/7674aa54-2a6a-44c7-a70f-db86beeeca66" />

## Artifact 3: Recovered Image Files from an Unaffected Storage Region

# Description
A relatively small section of the forensic image, approximately 1.5 MB in size (offsets 53,277,184–54,735,360), remained unaffected by the large overwrite regions. Forensic carving of this area successfully recovered six JPEG images and two GIF images. This section represents one of the few portions of the storage media where image data remained intact.

# Relevance
The recovered image files constitute the only identifiable photographs obtained from the forensic image. Examination of their metadata revealed no camera-generated EXIF information, GPS coordinates, or device identification details. However, one image contained the embedded comment “copyright 2000 philg@mit.edu,” while another included the compression comment “LEAD Technologies Inc.” These characteristics suggest that the images are more likely to be generic or stock graphics than photographs created by the suspect.

# Discovery Method
The image files were identified by scanning unallocated space with Binwalk, which detected JPEG and GIF file signatures. The files were subsequently recovered using Foremost, which reconstructed the images by identifying their respective header and footer signatures.

| **Supporting Evidence**   | **Key Findings**                                                                                        |
| ------------------------- | ------------------------------------------------------------------------------------------------------- |
| **File Carving Command**  | `foremost -t jpg,gif,png,pdf,zip,doc,docx -i cartel.img -o carved_foremost`                             |
| **Total Files Recovered** | **10 files successfully extracted**                                                                     |
| **Recovered File 1**      | **00104057.jpg** — JPEG image, **93 KB**, Offset: **53,277,184 bytes**                                  |
| **Recovered File 2**      | **00104249.jpg** — JPEG image, **405 KB**, Offset: **53,375,488 bytes**                                 |
| **Recovered File 3**      | **00105065.jpg** — JPEG image, **401 KB**, Offset: **53,793,280 bytes**                                 |
| **Recovered File 4**      | **00105873.jpg** — JPEG image, **258 KB**, Offset: **54,206,976 bytes**                                 |
| **Recovered File 5**      | **00106393.jpg** — JPEG image, **6 KB**, Offset: **54,473,216 bytes**                                   |
| **Recovered File 6**      | **00106409.jpg** — JPEG image, **225 KB**, Offset: **54,481,408 bytes**                                 |
| **Recovered File 7**      | **00106865.gif** — GIF image, **11 KB**, Offset: **54,714,880 bytes**, Resolution: **290 × 246 pixels** |
| **Recovered File 8**      | **00106889.gif** — GIF image, **4 KB**, Offset: **54,727,168 bytes**, Resolution: **150 × 87 pixels**   |
| **Recovered File 9**      | **00335081.jpg** — JPEG image, **258 KB**, Offset: **171,561,472 bytes**                                |
| **Recovered File 10**     | **00335017.doc** — Microsoft Word document, **11 MB**, Offset: **171,528,704 bytes**                    |
<img width="975" height="458" alt="image" src="https://github.com/user-attachments/assets/64829c40-5063-45d3-8cc4-e81ae57dd0e2" />
<img width="975" height="169" alt="image" src="https://github.com/user-attachments/assets/22b0a34b-df73-498c-aa9e-c34aad04e408" />
<img width="313" height="469" alt="image" src="https://github.com/user-attachments/assets/96ea48a4-c13d-4732-903e-bf274b83c95f" />

## Artifact 4: Recovered Personal Text Fragment Referencing Prior Data Destruction

# Description
A second, smaller surviving island (offsets ~171,516,224 – 171,843,904) was found inside the CHARLIE-wiped region. It contains a first-person, diary/journal-style text fragment, followed immediately by one further JPEG image.

# Relevance
The recovered text is written in the first person and includes the passage transcribed in the supporting evidence below. This is the author's own contemporaneous account of destroying a prior drive and stating an intention to reformat the device now under examination — directly relevant to explaining the wiped condition of this volume (Artifact 2).

# Discovery Method
Identified as a printable-text run inside the otherwise CHARLIE-filled region during offset-by-offset scanning for non-pattern content.

# Supporting Evidence
Recovered text (offsets 171,531,840 - 171,538,118), verbatim excerpt:
"...Things are getting a little weird. I zapped the hard drive and
then threw it into the Mississippi River. I'm gonna reformat my USB
key after this entry, but try not to destroy the good stuff. I need
to change the password on the gnome account that Jeremy gave me. I
can probably just do that at Radio Shack."

## Artifact 5: Duplicate File Identified via Hash Comparison
Description
SHA-256 hashing of every recovered file (performed as a chain-of-custody step) showed that the JPEG recovered at offset 54,206,976 (00105873.jpg) and the JPEG recovered at offset 171,561,472 (00335081.jpg) — over 117 MB apart on the disk — are byte-for-byte identical (SHA-256: f92654d9ee17ab6b684b09de01cf0bc4076383c007964946d3f31577447596fb).
Relevance
Identical content surviving in two widely separated, otherwise-wiped regions of the disk indicates the same file was written to the device on at least two separate occasions, or was present before both the SORRY and CHARLIE wipe operations — useful corroborating detail for sequencing events in Section 9.
Discovery Method
Routine SHA-256 hashing of all carved/recovered files, cross-referenced for exact matches.

| **Supporting Evidence**             | **Key Findings**                                                                                                                                                                 |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **SHA-256 Hash Comparison Command** | `sha256sum 00105873.jpg 00335081.jpg`                                                                                                                                            |
| **Recovered File 1**                | **File:** `00105873.jpg`<br>**SHA-256:** `f92654d9ee17ab6b684b09de01cf0bc4076383c007964946d3f31577447596fb`<br>**File Offset:** 54,206,976 bytes                                 |
| **Recovered File 2**                | **File:** `00335081.jpg`<br>**SHA-256:** `f92654d9ee17ab6b684b09de01cf0bc4076383c007964946d3f31577447596fb`<br>**File Offset:** 171,561,472 bytes                                |
| **Comparison Result**               | Both recovered JPEG files produced **identical SHA-256 hash values**, indicating that the recovered file contents are identical.                                                 |
| **Location Difference**             | The two identical files were recovered from different locations within the forensic image at offsets **54,206,976** and **171,561,472** bytes, approximately **117.4 MB apart**. |

# Deleted Data Analysis
A total of nine deleted files were successfully recovered from unallocated space through file carving, exceeding the minimum requirement of three files. None of the recovered files retained their original FAT directory entries because the corresponding directory metadata had either been overwritten by the large pattern-fill overwrite or was no longer available. As a result, the recovery tools assigned filenames based on the files' starting sectors within the image. The three most significant recovered files are described below, while the remaining six are summarised in Table 8.1.

# Recovered File 1: 00104057.jpg
- Original Filename
Not recoverable. The original FAT directory entry was not present; therefore, the file was recovered using signature-based carving from unallocated space rather than through directory metadata.

- File Type
JPEG image (JFIF 1.01), resolution 528 × 792 pixels, file size 93 KB. The image contains the embedded comment “copyright 2000 philg@mit.edu.”

- Recovery Method
The file was recovered from unallocated space at approximately offset 53,277,184 using Foremost. Recovery was performed by identifying the JPEG Start of Image (SOI) marker (0xFFD8) and End of Image (EOI) marker (0xFFD9) after Binwalk detected the file signature.

- Evidence of Successful Recovery
The recovered file opened successfully without corruption and was verified as a valid JPEG image using ImageMagick (identify) and manual visual examination. The image header, footer, and compressed image data were intact, confirming that the carving process recovered the complete file.

- Significance
This recovered image is relevant to the investigation because it provides evidence regarding the suspect's statement that the USB contained only personal photographs. Although the file is a valid photographic image, the embedded copyright information indicates that it is more consistent with a generic or stock image than a personal photograph created or captured by the suspect.

<img width="406" height="305" alt="image" src="https://github.com/user-attachments/assets/d2b7046e-0757-40d0-aeea-35bf39d9ce99" />

## Recovered File 2: 00106865.gif
- Original Filename
Not recoverable — no surviving FAT directory entry (recovered via signature-based carving of unallocated space, not directory traversal).
- File Type
GIF image (GIF89a), 290×246, 11 KB.
- Recovery Method
Carved from unallocated space at offset 54,714,880 using Foremost, based on GIF header (“GIF89a”) / trailer (0x3B) signature matching.
- Evidence of Successful Recovery
The recovered file decodes as a valid, complete GIF with an intact colour table and correct declared dimensions matching the rendered image — confirmed with identify and direct visual inspection.
- Significance
Demonstrates that non-JPEG image formats are also present on the device, recovered from the same pre-wipe surviving region as Recovered File 1; corroborates Artifact 3.
<img width="313" height="266" alt="image" src="https://github.com/user-attachments/assets/63e3688c-f394-48cc-8518-6ef33537be73" />

## Recovered File 3: diary_fragment.txt
- Original Filename
Not recoverable. No corresponding FAT directory entry was available because the file was recovered directly from unallocated space through manual carving rather than from the file system directory structure.

- File Type
Plain text (ASCII/extended ASCII), 6,278 bytes in size.

- Recovery Method
The text fragment was manually recovered by locating a continuous sequence of readable ASCII characters within the CHARLIE overwrite region (offsets approximately 171,531,840–171,538,118). Since plain text files do not contain distinctive file signatures, conventional signature-based carving tools could not recover the file. Recovery was therefore performed through byte-level analysis of the image contents.

- Evidence of Successful Recovery
The recovered content consists of coherent, grammatically consistent first-person text with no obvious truncation or corruption at either the beginning or end of the recovered fragment. The readable text is bounded on both sides by the surrounding CHARLIE overwrite pattern, indicating that the entire recoverable section was successfully extracted.

- Significance
This recovered text represents one of the most important artefacts identified during the examination. Its contents reference the destruction of a previous hard drive and an intention to reformat the USB storage device. These statements are consistent with the large-scale overwrite pattern documented in Artifact 2 and provide contextual information that may assist in explaining the observed condition of the storage media. The significance is based solely on the recovered content and its relationship to other forensic findings.

## Remaining Recovered Files
| **Filename**   |                      **Type** | **Size** | **File Offset (Bytes)** | **Notes**                                                                           |
| -------------- | ----------------------------: | -------: | ----------------------: | ----------------------------------------------------------------------------------- |
| `00104249.jpg` |                          JPEG |   405 KB |              53,375,488 | Resolution: **1349 × 900**; no EXIF metadata present.                               |
| `00105065.jpg` |                          JPEG |   401 KB |              53,793,280 | Resolution: **1686 × 1122**; no EXIF metadata present.                              |
| `00105873.jpg` |                          JPEG |   258 KB |              54,206,976 | Duplicate of `00335081.jpg` (Artifact 5), confirmed by identical SHA-256 hash.      |
| `00106393.jpg` |                          JPEG |     6 KB |              54,473,216 | Thumbnail image (**169 × 228**); contains **"LEAD Technologies"** comment metadata. |
| `00106409.jpg` |                          JPEG |   225 KB |              54,481,408 | Resolution: **1024 × 685**; no EXIF metadata present.                               |
| `00106889.gif` |                           GIF |     4 KB |              54,727,168 | Resolution: **150 × 87** pixels.                                                    |
| `00335017.doc` | Microsoft Word Document (DOC) |    11 MB |             171,528,704 | Not further analysed within the scope of this examination.                          |

# Timeline Reconstruction
Reconstructing a complete timeline was significantly limited due to the extensive overwrite pattern identified in Artifact 2. Files recovered through signature-based carving from unallocated space no longer retained their original filesystem metadata, including file creation, modification, access, and deletion timestamps, because the associated directory entries had been overwritten or were no longer available. Consequently, the only verifiable timestamps present within the forensic image were the Created and Modified/Written timestamps associated with the two allocated files that remained on the volume.

The timeline presented below combines these verified filesystem timestamps with the relative sequence of events inferred from the physical disk layout, recovered file locations, and other forensic observations obtained during the examination.

| **Order** | **Event**                                                                                   | **Timestamp / Basis**                                                                                                                     | **Evidence Type**                                                        |
| --------: | ------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
|     **1** | `GUMBO1.TXT` written to the volume                                                          | **2004-04-30 18:11:20 UTC** — FAT Created/Written timestamp                                                                               | Filesystem metadata (hard timestamp)                                     |
|     **2** | `GUMBO2.TXT` written to the volume                                                          | **2004-04-30 18:11:24 UTC** — FAT Created/Written timestamp                                                                               | Filesystem metadata (hard timestamp)                                     |
|     **3** | Photographs (Artifact 3) written to the device                                              | **Undated** — inferred to predate the **SORRY** wipe because they occupy the cluster range immediately before it                          | Relative position / inference (no hard timestamp)                        |
|     **4** | First wipe pass (**"SORRY"** pattern) executed                                              | **Undated** — inferred to have occurred after Event 3 and before Event 5                                                                  | Content/pattern analysis (no hard timestamp)                             |
|     **5** | Diary text and duplicate JPEG (Artifacts 4–5) written to, or already present on, the device | **Undated** — the diary text describes a separate hard drive being destroyed and is not itself evidence of destruction of this USB device | Content analysis (self-referential text; no hard timestamp)              |
|     **6** | Second wipe pass (**"CHARLIE"** pattern) executed                                           | **Undated** — inferred to have occurred after Event 5 because it partially overwrote the region containing those artifacts                | Relative position / inference (no hard timestamp)                        |
|     **7** | USB storage device seized by EFCC                                                           | **2026-08-02, 09:15 AM**                                                                                                                  | Case background / Chain of custody (not derived from the forensic image) |

## Limitations and Additional Evidence Required
A complete, absolutely-dated timeline cannot be produced from this image alone, for the following reasons:
- The 2004-04-30 timestamps associated with the only remaining allocated files occurred more than 22 years before the device seizure date of 2 August 2026, making them inconsistent with the known investigation timeline. The most likely explanation is an inaccurate system clock on the computer that last accessed or modified the device. This may occur due to factors such as an incorrectly configured clock, depleted system battery, older hardware, or FAT metadata that was not updated after an initial manufacturing or testing process. However, this explanation cannot be conclusively verified from the forensic image alone.
- All files recovered from unallocated space (Artifacts 3 and 4, including the nine files recovered during Task 4) do not contain recoverable MAC timestamps (Modified, Accessed, and Created times). This is because the relevant metadata is stored within filesystem directory entries, which were either overwritten or no longer available. File carving allows recovery of file content but does not restore associated filesystem metadata.
- The two identified overwrite operations (the “SORRY” and “CHARLIE” regions) cannot be assigned exact dates based solely on the available evidence. However, their sequence can be determined from their physical arrangement within the image, with the SORRY overwrite region appearing before the CHARLIE overwrite region, and the recovered diary fragment being located within the CHARLIE overwrite area.
- Establishing a complete and accurately dated timeline would require additional evidence sources, including access to the suspect's host computer to examine operating system and USB connection artefacts (such as Windows SetupAPI logs, USB registry records, or Linux udev/syslog entries) that may indicate when the device was connected and the system clock status at that time. Additional relevant evidence may include backup records, cloud synchronization data, communication records referencing the device, and forensic examination of the hard drive mentioned in the recovered diary text as having been “thrown into the Mississippi River,” should that device be located.
No information beyond that described above was provided regarding chain-of-custody handling after seizure, the suspect's host computer, network or cloud accounts associated with the device, or any prior forensic reports concerning this evidence. These items are noted as outstanding rather than assumed.

### Conclusion
This examination was scoped to test the suspect's statement that the USB storage device “only contains personal photographs” against verifiable forensic evidence recovered from the forensic image cartel.img. Based solely on the evidence identified and documented in this report:
- The only allocated, user-accessible files on the volume at the time of examination were two recipe text files (Artifact 1), not photographs.
- More than 80% of the storage device had been overwritten with repetitive ASCII patterns, consistent with an intentional data-destruction effort rather than routine device use (Artifact 2).
- The images that were recovered came from a small surviving region of the disk, carried no camera or GPS metadata, and carried embedded comments consistent with generic or stock imagery rather than personal photographs (Artifact 3).
- A first-person text fragment recovered from the device describes prior destruction of a separate hard drive and a stated intention to reformat this USB device (Artifact 4).
- A duplicated file recovered from two widely separated locations on the disk provides corroborating detail for the sequence of events described above (Artifact 5).
Taken together, the evidence recovered from the forensic image does not support the suspect's statement that the device contained only personal photographs. This conclusion is limited strictly to what was directly recovered and verified from the image during this examination; this report does not draw, and should not be read as drawing, any conclusion regarding the guilt or innocence of any individual. As detailed in Section 10, a fully dated and corroborated account of events would require additional evidence not available within the scope of this examination.

End of Report.
