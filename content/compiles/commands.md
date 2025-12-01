---
title: Terminal Toolkit
tags: [pwsh, cmd, linux]
date: 2025-11-30
---

<details>
<summary>Windows CMD</summary>

#### Files & Folders 
<!-- case 1 -->

<details>
<summary>Deleting a non empty directory </summary>  

```bash
rmdir /s /q $DIR_NAME
```
</details>

<!-- case 2 -->

<details>
<summary>Creating a empty file </summary>  

```bash
type NULL > $FILE_NAME
```
</details>

<!-- case 3 -->

<details>
<summary>Removing a File </summary>  

```bash
del $FILE_NAME
```
</details>

#### Variables

<!-- case 1 -->

<details>
<summary> windows-installation\Users\USER-NAME\AppData\local </summary>  

```bash
%localappdata%
```
</details>


</details>

---

<details>
<summary>Windows PowerShell</summary>

#### Paths


<!-- case 1 -->

<details>
<summary> Copy the current directory path </summary>  

```bash
(Get-Location).Path | Set-Clipboard
```
</details>

#### Files and Folders

<!-- case 1 -->

<details>
<summary> Copy the a file </summary>  

```bash
Copy-Item -Path <source> -Destination <destination>
```
</details>

<!-- case 2 -->

<details>
<summary> Remove the a file </summary>  

```bash
Remove-Item -Path "C:\path\to\your\file.txt"
```
</details>

<!-- case 3 -->

<details>
<summary> Rename the a file </summary>  

```bash
Rename-Item -Path "C:\path\to\your\file.txt" -Newname "updatedname"
```
</details>

<!-- case 4 -->

<details>
<summary> Move a file </summary>  

```bash
Move-Item -Path "C:\path\to\your\file.txt" -Destination "C:\path\to\your\destinaton"
```
</details>

<!-- case 5 -->

<details>
<summary> Check file hash </summary>  

```bash
Get-FileHash -Path <string> [-Algorithm <string>]
```
</details>

<!-- case 6 -->

<details>
<summary> Remove a non empty directory </summary>  

```bash
Remove-Item C:\path\to\non\empty\folder -Recurse -Force 
```
</details>


</details>
