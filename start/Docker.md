**🐳 Quick Docker Notes for Running PowerShell on Kali**

---

I have setup a powershell docker container and used alias:
startpwsh- to start container
stoppwsh- to stop container

---


Since you’re mainly using Docker to run **PowerShell**, here’s a **minimalist** reference guide to keep handy:

**🛠️ 1. Run PowerShell in a Docker Container**

```
docker run --rm -it mcr.microsoft.com/powershell:latest pwsh
```

✅ **--rm** = Removes container after exit

✅ **-it** = Interactive session

  

🔹 This **drops you into PowerShell** inside a **clean** container.

**📌 2. Run a One-Off PowerShell Command**

  

If you just need to run **a single PowerShell command**, use:

```
docker run --rm mcr.microsoft.com/powershell:latest pwsh -c "Get-Process"
```

This **executes** Get-Process inside the container **without interactive mode**.

**📂 3. Run PowerShell with a Persistent Volume**

  

If you need to **save files between runs**:

```
docker run --rm -it -v $(pwd):/pwsh mcr.microsoft.com/powershell:latest pwsh
```

🔹 This **mounts** your **current directory (pwd)** into /pwsh inside the container.

  

✅ **Example Use Case**:

```
PS> cd /pwsh
PS> echo 'Write-Host "Hello from Docker"' > script.ps1
PS> ./script.ps1
Hello from Docker
```

**🔥 4. Keep a Reusable PowerShell Container Running**

  

If you want to **reuse** a container instead of starting a new one each time:

```
docker run -dit --name powershell-container mcr.microsoft.com/powershell:latest pwsh
```

Now, you can **attach** to it later:

```
docker exec -it powershell-container pwsh
```

Or **stop/start it**:

```
docker stop powershell-container
docker start -ai powershell-container
```

**🗑️ 5. Clean Up Containers & Images**

• **Remove all stopped containers**:

```
docker container prune
```

  

• **Remove specific container**:

```
docker rm powershell-container
```

  

• **Remove images**:

```
docker rmi mcr.microsoft.com/powershell
```

**🚀 TL;DR - Most Useful Commands**

|**Task**|**Command**|
|---|---|
|Start PowerShell (temp)|docker run --rm -it mcr.microsoft.com/powershell:latest pwsh|
|Run a single command|docker run --rm mcr.microsoft.com/powershell:latest pwsh -c "Get-Process"|
|Mount local folder|docker run --rm -it -v $(pwd):/pwsh mcr.microsoft.com/powershell:latest pwsh|
|Persistent container|docker run -dit --name powershell-container mcr.microsoft.com/powershell:latest pwsh|
|Attach to running container|docker exec -it powershell-container pwsh|
|Stop/start the container|docker stop powershell-container && docker start -ai powershell-container|
|Clean up unused containers|docker container prune|

**🛠 Example - Base64 Encoding in PowerShell**

  

Once inside PowerShell:

```
$Bytes = [System.Text.Encoding]::Unicode.GetBytes($Text)
$EncodedText =[Convert]::ToBase64String($Bytes)
$EncodedText
```

Then exit:

```
exit
```

This should **cover everything you need** for running PowerShell in Docker while keeping things simple & efficient. Let me know if you need anything else! 🚀