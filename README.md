# Root-Me Forensics Challenge: Deleted File

---

## Overview/Description
The challenge involves recovering a deleted or hidden file from a USB disk image and extracting the hidden owner’s name formatted as `firstname_lastname`.

Your cousin found a USB drive in the library this morning. He’s not very good with computers, so he’s hoping you can find the owner of this stick!

The flag is the owner’s identity in the form firstname_lastname.

---

## Challenge Description

- A compressed archive containing a USB disk image was provided.
- The disk image contains a FAT32 filesystem.
- A file has been deleted or hidden inside this image.
- The task is to recover the deleted file and extract the flag (the owner’s name).

---

## Tools Used

| Tool           | Purpose                               |
|----------------|-------------------------------------|
| `gunzip`       | Decompress `.gz` archive             |
| `tar`          | Extract `.tar` archive               |
| `file`         | Identify file types                  |
| `mount`        | Mount the disk image for inspection |
| `foremost`     | Carve deleted files from image      |
| `strings`      | Extract readable strings from binaries |
| `grep`         | Filter for specific patterns        |


---

## Step-by-Step Solution

### 1. Decompress and Extract Archive

Decompress the archive:

```bash
gunzip ch39.gz
```
---


Rename and extract the tar archive:

```bash
mv ch39 ch39.tar
tar -xvf ch39.tar
```

---

### 2. Identify Disk Image
Check the file type of the extracted image:

```bash
file usb.image
```

Expected output:
```bash
usb.image: DOS/MBR boot sector, FAT32 filesystem
```

---

### 4. Recover Deleted Files Using Foremost
Carve deleted files from the disk image:

```bash
foremost -i usb.image -o output
```

Check carved PNG files:
```bash
ls output/png/
```
You should find:
```bash
00000168.png
```

---

### 5. Extract Hidden Data
Search for readable strings matching the flag format inside the PNG:

```bash
strings output/png/00000168.png | grep -E '[a-zA-Z]+[a-zA-Z]+'
```

This outputs:
```bash
<rdf:li>Javier Turcot</rdf:li>
```

The owner’s name is hidden as metadata inside the image.

---

📌 Summary
- This challenge involved decompressing a .gz archive.
- We extracted a USB image and used foremost to recover deleted files.
- Finally, we analyzed strings inside the .png to find hidden metadata.




