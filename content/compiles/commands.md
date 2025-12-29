---
title: Terminal Toolkit
tags: [pwsh, cmd, linux, docker]
date: 2025-11-30
---

### Windows Command Line 

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

---

### Windows Powershell

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
<summary> Copy the file </summary>  

```bash
Copy-Item -Path <source> -Destination <destination>
```
</details>

<!-- case 2 -->

<details>
<summary> Remove the file </summary>  

```bash
Remove-Item -Path "C:\path\to\your\file.txt"
```
</details>

<!-- case 3 -->

<details>
<summary> Rename the file </summary>  

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

---

### Docker Command Line Interface
 
<!-- case 1 -->

<details>
<summary> Create a new container </summary>  

```bash
docker run -it image-name 
```
- `it` - opens interactive shell
- check if image is present locally, else fetch it from [docker hub](https://hub.docker.com)
</details>

<details>
  <summary>List active containers</summary>  

```bash
docker container ls
```
</details>

<details>
  <summary>List all (non-active/active) containers</summary>  

```bash
docker container ls -a
```
</details>

<details>
  <summary>Run a container</summary>  

```bash
docker start container-name
```
</details>

</details>

<details>
  <summary>Terminate a container</summary>  

```bash
docker stop container-name
```
</details>

<details>
  <summary>Execute a command inside a container</summary>  

```bash
docker exec [-it] container-name command
```
</details>

<details>
  <summary>To list Docker Images</summary>
  
```bash
docker images
```
</details>

<details>
  <summary>To map ports b/w host and image</summary>
  
```bash
docker run -p base-port:image-port image-name
```
</details>

<details>
  <summary>To map environmental variables b/w host and image</summary>
  
```bash
docker run -e key1=value1 -e key2=value2 image-name
```
</details>

<details>
  <summary>Containerize an Image</summary>

- create a `Dockerfile`. Refer this [page](/scribbles/setups/quartz-dev) for example Dockerfile. 
```Dockerfile
	FROM ubuntu
	// run inside image
	RUN command
	// copy code repo
	COPY source destination
	// example copy commands
	COPY package.json package.json
    COPY main.js main.js
    ENTRYPOINT ["entry"]
```

- build the image 

```bash
Docker build -t image-name dockerfile-folder
```

- example

```bash
Docker build -t test-img .
```
- `t` - tag the image so that it can be used along with `start` and `stop` commands
</details>

<details>
  <summary>Check if docker can identify Nvida GPU drivers</summary>
  
```bash
docker run --rm --gpus all nvidia/cuda:12.0.1-base-ubuntu22.04 nvidia-smi
```
</details>

<details>
  <summary>Create from a `docker-compose.yaml` file</summary>
 
 ```bash
 docker compose up -d
 ```
</details>

<details>
  <summary>Install Open-webui and Ollama as single image</summary>
 
 ```bash
 docker run -d -p 3000:8080 --gpus=all -v ollama:/root/.ollama -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama
 ```
</details>


