# 4 Installing MySQL on Windows

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/5Rn49PJVJYc?si=kfWsL1xjV9zzS9JY"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🛠️ Installing MySQL on Windows

### 🎥 Installation Tutorial

In this tutorial, we'll go through the process of installing **MySQL** on a **Windows** computer. If you are using **Mac**, refer to the previous section for installation steps on macOS.

---

### Step 1: Download MySQL Installer

1. Open your **browser** and visit [MySQL.com](https://mysql.com).
2. Go to the **Downloads** page.
3. Scroll down and select the **MySQL Community Edition** (this is free for use in the course).
4. Under the **MySQL Community Server** section, click on **MySQL Installer for Windows**. This is the recommended way to install MySQL on Windows.

---

### Step 2: Start the Installation Process

1. On the next page, scroll down and click **Download** for the first available version of the **MySQL Installer**.
2. On the subsequent page, click **"No thanks, just start my download"** to avoid creating an account.
3. Save the installer to your computer and **run** it once the download completes.

---

### Step 3: Use the Setup Wizard

The installation process will be managed by the **Setup Wizard**. Here's what you need to do:

1. **Choose the Setup Type**: Select **Developer Default** to install MySQL Server and other useful tools.

2. **Address Potential Warnings**: You may see a warning about installing a Python 3.7 connector. This is not an issue unless you have Python on your system, so click **Next** to continue.

3. The wizard will show the list of products being installed:

   * **MySQL Server**
   * **MySQL Workbench** (used to manage your MySQL databases)

4. Click **Execute** to start the installation. This may take about 5-10 minutes. The installer will install all the necessary components.

---

### Step 4: Configure MySQL Server

After installation, the wizard will guide you through some configuration steps:

1. **Group Replication Page**: Just click **Next** (default settings are fine).

2. **Networking Settings**: Leave everything at its default values and click **Next**.

3. **Set Root Password**:

   * Enter a **password** for the **root (admin)** user in the provided box.
   * Click **Next**.

4. **Other Configuration Options**: Again, leave all settings as default and click **Next**.

5. Click **Execute** to apply the configuration.

---

### Step 5: Complete the Installation

1. After a few more steps, click **Next** and then **Finish**.
2. **Connect to MySQL Server**: The wizard will prompt you to enter the **root password** (the one you set during installation).
3. Once you've entered the password, click **Check** to verify the connection.

If the connection is successful, click **Next** to finish the installation process.

---

### Step 6: Launch MySQL Workbench

1. The wizard will now launch **MySQL Workbench** for you. This tool will be your primary interface to interact with MySQL databases and run SQL queries.
2. Close the **Command Prompt** window (we don’t need this).
3. If this is your first time using MySQL Workbench, you may need to create a new connection:

   * Click the **"+"** icon to add a new connection.
   * Give the connection a name, such as **"local instance"**.
   * For the password, click **Store in Keychain** and enter the password you set during installation.
   * Click **Test Connection** to ensure everything is working.
   * Once successful, click **OK** to save the connection.

---

### Step 7: Start Using MySQL Workbench

1. Now you can click the connection to connect to your **MySQL server**.
2. In **MySQL Workbench**, you’ll use:

   * **Navigator panel** (left side) to view and manage your databases.
   * **Query editor** (center) to write and execute your SQL queries.
   * **SQL Editor** (right side) for additional options and results.

---

### Summary of Installation Steps

| Step                            | Action                                                                  |
| ------------------------------- | ----------------------------------------------------------------------- |
| **1. Download MySQL Installer** | Go to MySQL website and download the installer.                         |
| **2. Run Setup Wizard**         | Use the Setup Wizard and choose **Developer Default**.                  |
| **3. Set Root Password**        | Enter a secure password for the **root** user.                          |
| **4. Install MySQL Server**     | Click **Execute** to install MySQL Server and Workbench.                |
| **5. Test Connection**          | Enter the root password and verify the connection.                      |
| **6. Launch MySQL Workbench**   | Use MySQL Workbench to connect to the server and manage your databases. |

---