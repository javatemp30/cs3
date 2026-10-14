# Program 3: OS Command Injection Using DVWA

A simple guide to **setting up Metasploitable 2** and performing the DVWA command-injection experiment.

**Already required:** Oracle VirtualBox and a working Kali Linux VM.

> **Safety:** Use only your own isolated VirtualBox lab. Metasploitable is deliberately vulnerable. Do not connect it to a Bridged/public network.

## 1. Download Metasploitable 2

1. Open https://sourceforge.net/projects/metasploitable/.
2. Download **Metasploitable 2** (ZIP file).
3. Extract the ZIP file.
4. Open the extracted folder and locate **`Metasploitable.vmdk`**.

## 2. Add Metasploitable to VirtualBox

1. Open **Oracle VirtualBox** and click **New**.
2. Name the VM **Metasploitable**.
3. Select **Linux** and a compatible **32-bit Linux** version (such as Ubuntu 32-bit, if available).
4. Assign **512 MB–1 GB RAM**.
5. Choose **Use an existing virtual hard disk**.
6. Select **`Metasploitable.vmdk`** from the extracted folder.
7. Finish creating the VM.

**Important:** Attach `Metasploitable.vmdk`, **not** Kali's `.vdi` file. If the wrong disk is attached, power off the VM, remove only the incorrect attachment under **Settings → Storage** (do **not** delete the file), and attach the correct `.vmdk`.

## 3. Configure the network

1. Power off both VMs.
2. In **VirtualBox → Kali → Settings → Network**, select a **Host-only Adapter**.
3. In **Metasploitable → Settings → Network**, select the **same Host-only network**.
4. Save the settings.

## 4. Start Metasploitable

1. Start the Metasploitable VM.
2. Log in:

   ```text
   Username: msfadmin
   Password: msfadmin
   ```

3. Find its IP address:

   ```bash
   ifconfig
   ```

In our lab, the IP address was **`192.168.56.102`**. Your IP may differ.

## 5. Test the connection from Kali

1. Start Kali and open Terminal.
2. Run (replace the IP if needed):

   ```bash
   ping -c 4 192.168.56.102
   ```

**Expected:** `4 packets transmitted, 4 received, 0% packet loss`.

## 6. Open DVWA

1. Open **Firefox** in Kali.
2. Enter:

   ```text
   http://192.168.56.102/dvwa/
   ```

3. Log in:

   ```text
   Username: admin
   Password: password
   ```

4. **First-time setup only:** If DVWA asks for database setup, click **Create / Reset Database** and wait for success. Do not reset it every time.

## 7. Set security to Low

1. Click **DVWA Security** in the left menu.
2. Select **Low**.
3. Click **Submit**.

## 8. Perform the experiment

1. Open **Command Execution** in the DVWA menu.
2. Enter the normal test input:

   ```text
   127.0.0.1
   ```

3. Click **Submit**. You should see ping output.
4. Now enter:

   ```text
   127.0.0.1; ls
   ```

5. Click **Submit** again.

**Output observed in our lab:**

```text
help
index.php
source
```

The extra file listing shows that DVWA executed an additional operating-system command. Capture a screenshot of the result.

## 9. Common mistakes

| Problem | Fix |
|---|---|
| Metasploitable boots incorrectly | Check that `Metasploitable.vmdk`, not Kali's `.vdi`, is attached. |
| Kali cannot ping Metasploitable | Check both VMs are running, using the same Host-only network, and that the target IP is correct. |
| DVWA page does not open | Check the IP address and ping connection. |
| DVWA shows setup page | Create the database once if needed, then log in. |
| Only ping output appears | Confirm **DVWA Security = Low** and enter `127.0.0.1; ls` exactly. |
| Input box clears after Submit | This is normal; check the output below the form. |

## 10. Result

**OS Command Injection was successfully demonstrated using DVWA.** The vulnerable application ran an additional command (`ls`) supplied through its input field.

shell execution, validate inputs, and use least privilege.
