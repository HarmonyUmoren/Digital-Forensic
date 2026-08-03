# Digital-Forensic

DIGITAL FORENSIC EXAMINATION REPORT
Forensic Analysis of a USB Storage Device
Forensic Image Reference: cartel.img
CASE EXERCISE: CAPTURE THE FLAG

Prepared By	Harmony (Digital Forensic Examiner)
Module	Digital Forensics and Incident Response
Date of Examination	3 August 2026
Evidence Seized	2 August 2026, 09:15 AM, by EFCC officers pursuant to a search warrant
Evidence Type	USB storage device (forensic image: cartel.img)
Report Classification	Confidential — For Authorized Recipients Only
 
1. Executive Summary
This report presents the findings of a digital forensic examination conducted on a forensic image (cartel.img) acquired from a USB storage device recovered by the Economic and Financial Crimes Commission (EFCC) on 2 August 2026, pursuant to a search warrant. The device was seized as part of a broader investigation, and the suspect associated with the device stated that it “only contains personal photographs.”
The purpose of this examination was to test that claim against verifiable forensic evidence obtained from the image itself. This report documents the identification, preservation, examination, and analysis of the digital evidence contained within the image, and presents findings strictly on the basis of what was recovered and verified during the examination. No determination of guilt or innocence is made or implied by this report.
Key findings are summarized below and are described in full detail in the corresponding sections of this report:
●	Integrity of the forensic image was verified using MD5 and SHA-256 hashing prior to examination (Section 5).
●	Initial triage identified the image as a raw (dd-style), unpartitioned FAT16 volume, approximately 247.5 MB in size (Section 6).
●	Two allocated text files (GUMBO1.TXT and GUMBO2.TXT), containing recipes, were the only user-accessible files present on the volume at the time of examination. Their presence does not support the suspect's statement that the device contained only personal photographs (Section 7, Artifact 1).
●	Approximately 208 MB — more than 80% of the storage device — was found to contain two large regions of repetitive ASCII text (“SORRY” and “CHARLIE”), consistent with an intentional overwrite of previously stored data (Section 7, Artifact 2).
●	A surviving, unaffected region of the image yielded six JPEG images and two GIF images through file carving. None of the recovered images contained camera-generated EXIF or GPS metadata; embedded comments in two images were consistent with generic or stock graphics rather than personal photographs (Section 7, Artifact 3).
●	A recovered first-person text fragment, located within the CHARLIE-overwritten region, references the prior destruction of a hard drive and a stated intention to reformat the USB device under examination (Section 7, Artifact 4).
●	Hash comparison identified two byte-for-byte identical JPEG files recovered from disk locations over 117 MB apart, indicating the same file existed on the device on more than one occasion or prior to both overwrite events (Section 7, Artifact 5).
●	A total of nine deleted files were recovered from unallocated space through file carving (Section 8).
●	A relative event timeline was reconstructed from the limited verifiable evidence available; however, an absolutely-dated timeline could not be established from the image alone, and specific additional evidence would be required to do so (Section 9 and Section 10).
Collectively, the evidence recovered from the forensic image is inconsistent with the suspect's statement that the device contained only personal photographs. This report does not draw any conclusion beyond what is directly supported by the recovered evidence, and clearly identifies areas where information was not available within the scope of this examination.
2. Case Background and Scope
2.1 Case Information
Examiner	Harmony (Digital Forensic Examiner)
Module	Digital Forensics and Incident Response
Date of Examination	3 August 2026
Evidence Seized	2 August 2026, 09:15 AM
Seizing Authority	Economic and Financial Crimes Commission (EFCC), pursuant to a search warrant
Evidence Description	USB storage device
Forensic Image Analyzed	cartel.img

2.2 Objective
This examination was conducted to identify, preserve, examine, analyze, and document digital evidence contained within the forensic image (cartel.img) acquired from a USB storage device recovered during an EFCC search operation on 2 August 2026. The suspect claimed the device “only contains personal photographs.” The objective of this examination was to test this claim against verifiable forensic evidence, without making any determination of guilt or innocence.
2.3 Scope
This examination was limited to the forensic image cartel.img as provided. No physical device, host computer, network evidence, or additional storage media was made available to, or examined by, this examiner. Any conclusions in this report are therefore limited to what can be directly established from the content of this single forensic image. Where information necessary to answer a specific question was not available within the scope of this examination, that limitation is explicitly stated rather than inferred or assumed.
3. Methodology and Tools Used
The examination followed standard digital forensic principles of preservation, verification, and documentation, working from a forensic image rather than the original physical media, in order to avoid any modification of the source evidence. The following tools were used during this examination:
●	md5sum — cryptographic hash verification (MD5)
●	sha256sum — cryptographic hash verification (SHA-256)
●	mmls — partition table / volume system analysis (The Sleuth Kit)
●	img_stat — forensic image metadata analysis (The Sleuth Kit)
●	fsstat — file system structure analysis (The Sleuth Kit)
●	fls — recursive file and directory listing (The Sleuth Kit)
●	icat / istat — file content and metadata extraction (The Sleuth Kit)
●	blkls — unallocated space extraction (The Sleuth Kit)
●	Python (byte-level scripting) — offset-level analysis of raw image content
●	Binwalk — file signature detection within unallocated space
●	Foremost — signature-based file carving
●	ImageMagick (identify) — image file validation
●	FTK Imager and Autopsy were listed as optional tools; their use in the production of the findings in this report is not evidenced and they are noted here for completeness only.
All commands and their outputs referenced throughout this report were captured directly from the examination and are reproduced in the relevant sections and supporting figures.
4. Chain of Custody
The forensic image cartel.img was derived from a USB storage device seized by EFCC officers on 2 August 2026 at 09:15 AM, pursuant to a search warrant. Examination of the image was conducted on 3 August 2026. Beyond the seizure date, seizing authority, and examination date stated above, no further chain-of-custody documentation (e.g., evidence transfer logs, storage location, or intermediate custodians) was provided as part of this exercise; this report makes no assumptions regarding those details.
5. Task 1 — Evidence Verification
5.1 Purpose
Integrity verification was performed by calculating the MD5 and SHA-256 hash values of the forensic image (cartel.img) prior to forensic examination. The purpose of this process is to ensure that the forensic image has not been modified during storage or analysis. The calculated hash values provide a digital fingerprint of the evidence and serve as a reference for verifying its authenticity throughout the investigation.
5.2 Hash Values Calculated
File	cartel.img
MD5	80348c58eec4c328ef1f7709adc56a54
SHA-256	ce550424200a997c61b413941c8ef4df9619a2f96579674952294a176a32be65

Figure 5.1 — MD5 and SHA-256 hash calculation of cartel.img
5.3 Why Matching Hashes Matter
Matching hash values confirm that the forensic image has not been altered during the examination. This preserves the integrity and authenticity of the digital evidence and demonstrates that the analysis was conducted on an exact, unmodified copy of the original data. Any difference in hash values would indicate possible alteration or corruption of the evidence, requiring further investigation before the findings could be considered reliable.
The hash values recorded above represent the values calculated for the image as examined. No independent, previously-recorded reference hash (e.g., a value generated at the point of acquisition by the seizing officers) was provided for direct comparison; consequently, this report records the calculated values as a verification baseline for this examination rather than as a confirmed match against an external record.
 
Figure 5.1 — MD5 and SHA-256 hash calculation of cartel.img
6. Task 2 — Initial Triage
Image format	Raw (dd-style) disk image
File system type	FAT16
Number of partitions	0 — no MBR partition table detected (unpartitioned “superfloppy” volume; the whole disk is a single FAT16 file system)
Total size	259,506,176 bytes (≈ 247.5 MB)
Sector size	512 bytes
Cluster size	4,096 bytes (8 sectors)
OEM name (boot sector)	mkdosfs — indicates the volume was originally formatted with the Linux dosfstools utility
Volume serial number	0x4092D9D1
Volume label	(none set)

6.1 Supporting Command Output
$ mmls cartel.img
(no output — no partition table found)

$ img_stat cartel.img
IMAGE FILE INFORMATION
--------------------------------------------
Image Type: raw
Size in bytes: 259506176
Sector size: 512

$ fsstat cartel.img
FILE SYSTEM INFORMATION
--------------------------------------------
File System Type: FAT16
OEM Name: mkdosfs
Volume ID: 0x4092d9d1
File System Type Label: FAT16

File System Layout (in sectors)
Total Range: 0 - 506847
* Reserved: 0 - 0
* FAT 0: 1 - 248
* FAT 1: 249 - 496
* Data Area: 497 - 506847
** Root Directory: 497 - 528
** Cluster Area: 529 - 506840
mmls / img_stat / fsstat output confirming image format, partition layout, and file system
6.2 Why These Preliminary Steps Are Necessary
Initial triage helps the examiner understand the structure and characteristics of the forensic image before performing a detailed investigation. It identifies the image format, file system, partition layout, and storage information, allowing the examiner to choose the correct forensic tools and examination methods. Performing this step reduces the risk of analysing the wrong data, misinterpreting the file system, or producing inaccurate results. It also provides a clear understanding of how the evidence is organized, making subsequent analysis and data recovery more effective.
7. Task 3 — Evidence Discovery
7.1 Artifact 1: GUMBO1.TXT and GUMBO2.TXT (Allocated Files)
Description
The FAT16 file system contained two allocated text files named GUMBO1.TXT and GUMBO2.TXT. These files contain recipes titled “Shrimp and Tasso Gumbo” and “Shrimp and Andouille Sausage Gumbo,” with file sizes of 2,815 bytes and 1,293 bytes respectively.
Relevance
These were the only user-accessible files present on the USB drive when it was examined. Their presence does not support the suspect's statement that the USB contained only personal photographs, as both files are recipe documents rather than image files.
Discovery Method
The files were identified by performing a recursive directory listing using fls, after which their contents and metadata were examined using icat and istat.
Supporting Evidence
$ fls cartel.img
r/r 4: gumbo1.txt
r/r 6: gumbo2.txt
v/v 8101619: $MBR
v/v 8101620: $FAT1
v/v 8101621: $FAT2
V/V 8101622: $OrphanFiles

$ istat cartel.img 4
Directory Entry: 4
Allocated
File Attributes: File, Archive
Size: 2815
Name: GUMBO1.TXT
Written: 2004-04-30 18:11:20 (UTC)
Created: 2004-04-30 18:11:20 (UTC)

$ istat cartel.img 6
Directory Entry: 6
Allocated
File Attributes: File, Archive
Size: 1293
Name: GUMBO2.TXT
Written: 2004-04-30 18:11:24 (UTC)
Created: 2004-04-30 18:11:24 (UTC)
fls listing and istat metadata for both allocated files
7.2 Artifact 2: Extensive ASCII Overwrite Pattern
Description
Examination of the forensic image revealed two consecutive regions occupying most of the storage medium that contained repetitive, human-readable ASCII strings. The first region, approximately 54.5 MB, consisted of repeated occurrences of the string “SORRY” (offsets approximately 273,920–54,735,350). This was immediately followed by a second region of approximately 148.5 MB containing repeated instances of the string “CHARLIE” (offsets approximately 54,735,360–203,186,168). Combined, these overwrite regions occupied roughly 208 MB, accounting for more than 80% of the 247.5 MB storage device.
Relevance
Unlike standard file deletion or a quick format — which typically leaves previous file contents recoverable within unallocated space — the presence of large, continuous regions filled with repetitive ASCII text is unusual during normal device operation. The observed overwrite pattern indicates that a significant portion of the storage medium was intentionally overwritten, suggesting an effort to permanently remove or obscure previously stored data before the device was seized.
Discovery Method
The overwrite pattern was identified through forensic examination of unallocated space using blkls output and direct byte-level analysis at specific offsets with Python scripts. A comprehensive scan of the forensic image confirmed the precise starting and ending offsets of both repetitive text regions, establishing their overall size and distribution across the storage medium.
Supporting Evidence
>>> data = open('cartel.img','rb').read()
>>> data[2200544:2200544+40]
b'SORRY\nSORRY\nSORRY\nSORRY\nSORRY\nSO'
>>> data[54800000:54800000+40]
b'CHARLIE\nCHARLIE\nCHARLIE\nCHARLIE\n'

# Pattern boundaries (regex scan of full image):
SORRY   : 8,835,577 occurrences, offsets 273663 - 54735350
CHARLIE : 18,372,608 occurrences, offsets 54735356 - 203186168
7.3 Artifact 3: Recovered Image Files from an Unaffected Storage Region
Description
A relatively small section of the forensic image, approximately 1.5 MB in size (offsets 53,277,184–54,735,360), remained unaffected by the large overwrite regions. Forensic carving of this area successfully recovered six JPEG images and two GIF images. This section represents one of the few portions of the storage media where image data remained intact.
Relevance
The recovered image files constitute the only identifiable photographs obtained from the forensic image. Examination of their metadata revealed no camera-generated EXIF information, GPS coordinates, or device identification details. However, one image contained the embedded comment “copyright 2000 philg@mit.edu,” while another included the compression comment “LEAD Technologies Inc.” These characteristics suggest that the images are more likely to be generic or stock graphics than photographs created by the suspect.
Discovery Method
The image files were identified by scanning unallocated space with Binwalk, which detected JPEG and GIF file signatures. The files were subsequently recovered using Foremost, which reconstructed the images by identifying their respective header and footer signatures.
Supporting Evidence
$ foremost -t jpg,gif,png,pdf,zip,doc,docx -i cartel.img -o carved_foremost
Num  Name(bs=512)   Size   File Offset   Comment
0:   00104057.jpg   93 KB  53277184
1:   00104249.jpg  405 KB  53375488
2:   00105065.jpg  401 KB  53793280
3:   00105873.jpg  258 KB  54206976
4:   00106393.jpg    6 KB  54473216
5:   00106409.jpg  225 KB  54481408
6:   00106865.gif   11 KB  54714880  (290 x 246)
7:   00106889.gif    4 KB  54727168  (150 x 87)
8:   00335081.jpg  258 KB  171561472
9:   00335017.doc   11 MB  171528704
10 FILES EXTRACTED
7.4 Artifact 4: Recovered Personal Text Fragment Referencing Prior Data Destruction
Description
A second, smaller surviving island (offsets ~171,516,224 – 171,843,904) was found inside the CHARLIE-wiped region. It contains a first-person, diary/journal-style text fragment, followed immediately by one further JPEG image.
Relevance
The recovered text is written in the first person and includes the passage transcribed in the supporting evidence below. This is the author's own contemporaneous account of destroying a prior drive and stating an intention to reformat the device now under examination — directly relevant to explaining the wiped condition of this volume (Artifact 2).
Discovery Method
Identified as a printable-text run inside the otherwise CHARLIE-filled region during offset-by-offset scanning for non-pattern content.
Supporting Evidence
Recovered text (offsets 171,531,840 - 171,538,118), verbatim excerpt:
"...Things are getting a little weird. I zapped the hard drive and
then threw it into the Mississippi River. I'm gonna reformat my USB
key after this entry, but try not to destroy the good stuff. I need
to change the password on the gnome account that Jeremy gave me. I
can probably just do that at Radio Shack."
7.5 Artifact 5: Duplicate File Identified via Hash Comparison
Description
SHA-256 hashing of every recovered file (performed as a chain-of-custody step) showed that the JPEG recovered at offset 54,206,976 (00105873.jpg) and the JPEG recovered at offset 171,561,472 (00335081.jpg) — over 117 MB apart on the disk — are byte-for-byte identical (SHA-256: f92654d9ee17ab6b684b09de01cf0bc4076383c007964946d3f31577447596fb).
Relevance
Identical content surviving in two widely separated, otherwise-wiped regions of the disk indicates the same file was written to the device on at least two separate occasions, or was present before both the SORRY and CHARLIE wipe operations — useful corroborating detail for sequencing events in Section 9.
Discovery Method
Routine SHA-256 hashing of all carved/recovered files, cross-referenced for exact matches.
Supporting Evidence
$ sha256sum 00105873.jpg 00335081.jpg
f92654d9ee17ab6b684b09de01cf0bc4076383c007964946d3f31577447596fb  00105873.jpg
f92654d9ee17ab6b684b09de01cf0bc4076383c007964946d3f31577447596fb  00335081.jpg

Offsets: 54,206,976 and 171,561,472 (delta approx. 117.4 MB apart)
8. Task 4 — Deleted Data Analysis
A total of nine deleted files were successfully recovered from unallocated space through file carving, exceeding the minimum requirement of three files. None of the recovered files retained their original FAT directory entries because the corresponding directory metadata had either been overwritten by the large pattern-fill overwrite or was no longer available. As a result, the recovery tools assigned filenames based on the files' starting sectors within the image. The three most significant recovered files are described below, while the remaining six are summarised in Table 8.1.
8.1 Recovered File 1: 00104057.jpg
Original Filename
Not recoverable. The original FAT directory entry was not present; therefore, the file was recovered using signature-based carving from unallocated space rather than through directory metadata.
File Type
JPEG image (JFIF 1.01), resolution 528 × 792 pixels, file size 93 KB. The image contains the embedded comment “copyright 2000 philg@mit.edu.”
Recovery Method
The file was recovered from unallocated space at approximately offset 53,277,184 using Foremost. Recovery was performed by identifying the JPEG Start of Image (SOI) marker (0xFFD8) and End of Image (EOI) marker (0xFFD9) after Binwalk detected the file signature.
Evidence of Successful Recovery
The recovered file opened successfully without corruption and was verified as a valid JPEG image using ImageMagick (identify) and manual visual examination. The image header, footer, and compressed image data were intact, confirming that the carving process recovered the complete file.
Significance
This recovered image is relevant to the investigation because it provides evidence regarding the suspect's statement that the USB contained only personal photographs. Although the file is a valid photographic image, the embedded copyright information indicates that it is more consistent with a generic or stock image than a personal photograph created or captured by the suspect.
 
Figure 8.1 — Recovered File 1 (00105873.jpg, a related recovered image shown for illustration), rendered from the carved data
8.2 Recovered File 2: 00106865.gif
Original Filename
Not recoverable — no surviving FAT directory entry (recovered via signature-based carving of unallocated space, not directory traversal).
File Type
GIF image (GIF89a), 290×246, 11 KB.
Recovery Method
Carved from unallocated space at offset 54,714,880 using Foremost, based on GIF header (“GIF89a”) / trailer (0x3B) signature matching.
Evidence of Successful Recovery
The recovered file decodes as a valid, complete GIF with an intact colour table and correct declared dimensions matching the rendered image — confirmed with identify and direct visual inspection.
Significance
Demonstrates that non-JPEG image formats are also present on the device, recovered from the same pre-wipe surviving region as Recovered File 1; corroborates Artifact 3.
 
Figure 8.2 — Recovered File 2 (00106865.gif), rendered from the carved data
8.3 Recovered File 3: diary_fragment.txt
Original Filename
Not recoverable. No corresponding FAT directory entry was available because the file was recovered directly from unallocated space through manual carving rather than from the file system directory structure.
File Type
Plain text (ASCII/extended ASCII), 6,278 bytes in size.
Recovery Method
The text fragment was manually recovered by locating a continuous sequence of readable ASCII characters within the CHARLIE overwrite region (offsets approximately 171,531,840–171,538,118). Since plain text files do not contain distinctive file signatures, conventional signature-based carving tools could not recover the file. Recovery was therefore performed through byte-level analysis of the image contents.
Evidence of Successful Recovery
The recovered content consists of coherent, grammatically consistent first-person text with no obvious truncation or corruption at either the beginning or end of the recovered fragment. The readable text is bounded on both sides by the surrounding CHARLIE overwrite pattern, indicating that the entire recoverable section was successfully extracted.
Significance
This recovered text represents one of the most important artefacts identified during the examination. Its contents reference the destruction of a previous hard drive and an intention to reformat the USB storage device. These statements are consistent with the large-scale overwrite pattern documented in Artifact 2 and provide contextual information that may assist in explaining the observed condition of the storage media. The significance is based solely on the recovered content and its relationship to other forensic findings.
8.4 Remaining Recovered Files
The remaining six deleted files recovered during this task are summarised in Table 8.1.
Filename	Type	Size	Offset	Note
00104249.jpg	JPEG	405 KB	53,375,488	1349×900, no EXIF
00105065.jpg	JPEG	401 KB	53,793,280	1686×1122, no EXIF
00105873.jpg	JPEG	258 KB	54,206,976	Duplicate of 00335081.jpg (Artifact 5)
00106393.jpg	JPEG	6 KB	54,473,216	169×228 thumbnail, “LEAD Technologies” comment
00106409.jpg	JPEG	225 KB	54,481,408	1024×685, no EXIF
00106889.gif	GIF	4 KB	54,727,168	150×87
00335017.doc	DOC	11 MB	171,528,704	Not further analysed within this examination

Table 8.1 — Summary of remaining recovered files
9. Task 5 — Timeline Reconstruction
Reconstructing a complete timeline was significantly limited due to the extensive overwrite pattern identified in Artifact 2. Files recovered through signature-based carving from unallocated space no longer retained their original filesystem metadata, including file creation, modification, access, and deletion timestamps, because the associated directory entries had been overwritten or were no longer available. Consequently, the only verifiable timestamps present within the forensic image were the Created and Modified/Written timestamps associated with the two allocated files that remained on the volume.
The timeline presented below combines these verified filesystem timestamps with the relative sequence of events inferred from the physical disk layout, recovered file locations, and other forensic observations obtained during the examination.
Order	Event	Timestamp / Basis	Evidence Type
1	GUMBO1.TXT written to volume	2004-04-30 18:11:20 (UTC) — FAT Created/Written time	Filesystem metadata (hard timestamp)
2	GUMBO2.TXT written to volume	2004-04-30 18:11:24 (UTC) — FAT Created/Written time	Filesystem metadata (hard timestamp)
3	Photographs (Artifact 3) written to device	Undated — inferred to predate the SORRY wipe, as they occupy the cluster range immediately before it	Relative position / inference, no hard timestamp
4	First wipe pass (“SORRY” pattern) executed	Undated — inferred to occur after item 3, before item 5	Content/pattern analysis, no hard timestamp
5	Diary text + duplicate JPEG (Artifacts 4-5) written or already present	Undated — the diary text itself describes a prior, separate drive being destroyed, distinct from this device	Content analysis (self-referential text), no hard timestamp
6	Second wipe pass (“CHARLIE” pattern) executed	Undated — inferred to occur after item 5, since it partially overwrote the region containing it	Relative position / inference, no hard timestamp
7	Device seized by EFCC	2026-08-02, 09:15 AM	Case background (chain of custody, not derived from the image)

Table 9.1 — Reconstructed relative event timeline
10. Limitations and Additional Evidence Required
A complete, absolutely-dated timeline cannot be produced from this image alone, for the following reasons:
●	The 2004-04-30 timestamps associated with the only remaining allocated files occurred more than 22 years before the device seizure date of 2 August 2026, making them inconsistent with the known investigation timeline. The most likely explanation is an inaccurate system clock on the computer that last accessed or modified the device. This may occur due to factors such as an incorrectly configured clock, depleted system battery, older hardware, or FAT metadata that was not updated after an initial manufacturing or testing process. However, this explanation cannot be conclusively verified from the forensic image alone.
●	All files recovered from unallocated space (Artifacts 3 and 4, including the nine files recovered during Task 4) do not contain recoverable MAC timestamps (Modified, Accessed, and Created times). This is because the relevant metadata is stored within filesystem directory entries, which were either overwritten or no longer available. File carving allows recovery of file content but does not restore associated filesystem metadata.
●	The two identified overwrite operations (the “SORRY” and “CHARLIE” regions) cannot be assigned exact dates based solely on the available evidence. However, their sequence can be determined from their physical arrangement within the image, with the SORRY overwrite region appearing before the CHARLIE overwrite region, and the recovered diary fragment being located within the CHARLIE overwrite area.
●	Establishing a complete and accurately dated timeline would require additional evidence sources, including access to the suspect's host computer to examine operating system and USB connection artefacts (such as Windows SetupAPI logs, USB registry records, or Linux udev/syslog entries) that may indicate when the device was connected and the system clock status at that time. Additional relevant evidence may include backup records, cloud synchronization data, communication records referencing the device, and forensic examination of the hard drive mentioned in the recovered diary text as having been “thrown into the Mississippi River,” should that device be located.
No information beyond that described above was provided regarding chain-of-custody handling after seizure, the suspect's host computer, network or cloud accounts associated with the device, or any prior forensic reports concerning this evidence. These items are noted as outstanding rather than assumed.
11. Conclusion
This examination was scoped to test the suspect's statement that the USB storage device “only contains personal photographs” against verifiable forensic evidence recovered from the forensic image cartel.img. Based solely on the evidence identified and documented in this report:
●	The only allocated, user-accessible files on the volume at the time of examination were two recipe text files (Artifact 1), not photographs.
●	More than 80% of the storage device had been overwritten with repetitive ASCII patterns, consistent with an intentional data-destruction effort rather than routine device use (Artifact 2).
●	The images that were recovered came from a small surviving region of the disk, carried no camera or GPS metadata, and carried embedded comments consistent with generic or stock imagery rather than personal photographs (Artifact 3).
●	A first-person text fragment recovered from the device describes prior destruction of a separate hard drive and a stated intention to reformat this USB device (Artifact 4).
●	A duplicated file recovered from two widely separated locations on the disk provides corroborating detail for the sequence of events described above (Artifact 5).
Taken together, the evidence recovered from the forensic image does not support the suspect's statement that the device contained only personal photographs. This conclusion is limited strictly to what was directly recovered and verified from the image during this examination; this report does not draw, and should not be read as drawing, any conclusion regarding the guilt or innocence of any individual. As detailed in Section 10, a fully dated and corroborated account of events would require additional evidence not available within the scope of this examination.

End of Report.
