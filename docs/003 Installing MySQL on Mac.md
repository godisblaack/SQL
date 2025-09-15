# 3 Installing MySQL on Mac

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/_P9T89azRoo?si=0PoyNfx9l-GA9THM"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🛠️ Installing MySQL on a Mac

### 🎥 Installation Tutorial

In this tutorial, we’ll walk through the process of installing **MySQL** on a **Mac** computer. If you're using **Windows**, feel free to skip this part (we'll cover it separately). Here's how to get MySQL installed and running on your Mac.

---

### Step 1: Download MySQL Community Edition

1. Open your **browser** and go to [MySQL.com](https://mysql.com).
2. Navigate to the **Downloads** page.
3. Scroll down and select **MySQL Community Edition** (this is free and what we will use for the course).
4. Under the "MySQL Community Server" section, choose the **DMG Archive** for **Mac OS** (this is the easiest option for Mac users).

---

### Step 2: Download and Start Installation

1. Click on **MySQL Community Server** to begin the download.
2. On the next page, click on **"No thanks, just start my download"** to begin the download immediately.
3. After the DMG file finishes downloading, **open** it to start the installation process.

---

### Step 3: Install MySQL

1. **Double-click** on the downloaded DMG file to launch the **MySQL Installation Wizard**.

2. Click **Continue** to start the installation process.

3. Click **Agree** to accept the **license agreement**.

4. Click **Install** to proceed with the installation.

   * You’ll be prompted to enter your **computer's password** to allow the installation.
   * After that, you’ll be asked to set a password for the **root (admin)** user.
   * Set a **complex password** for the **root** user and click **Next**.

5. Once the installation is complete, enter your computer’s password again and click **Finish** to complete the installation.

---

### Step 4: Install MySQL Workbench (Graphical Tool)

To manage MySQL databases, we need a graphical tool called **MySQL Workbench**.

1. Go back to the **Downloads** page on MySQL's website and scroll to the bottom.
2. Under the **MySQL Community Edition**, find and click on **MySQL Workbench**.
3. Download the **DMG Archive** for MySQL Workbench.
4. Once the DMG file finishes downloading, **open** it.

---

### Step 5: Complete MySQL Workbench Installation

1. In the installation window, **drag** the **MySQL Workbench** icon to the **Applications** folder.
2. Once the app is copied, go to your **Applications** folder and search for **MySQL Workbench** using **Command + Space**.

---

### Step 6: Set Up MySQL Workbench

1. Open **MySQL Workbench**. The first time you open it, you might get a security warning since it’s an app downloaded from the internet. Click **Open** to proceed.

2. **Create a new connection**:

   * Click the **"+" icon** to add a new connection.
   * Name the connection (e.g., `local instance`).
   * Set the **connection method** to **TCP/IP** (this is the default).
   * For **Host Name**, enter `127.0.0.1` (this points to your local machine).
   * Set the **Port** to `3306` (the default MySQL port).
   * For the **Username**, use `root` (this is the default admin user).
   * **Enter the password** you set during the installation process.
   * Check the **"Store in Keychain"** option to save the password.

3. Click **Test Connection** to ensure MySQL Workbench can connect to your MySQL server. If successful, click **OK**.

---

### Step 7: Finalizing the Setup

Now, every time you open **MySQL Workbench**, you can use this connection to interact with your local MySQL server. You're all set to start working with MySQL databases!

---

### Summary of Installation Steps

1. **Download MySQL Community Server** for Mac from the official website.
2. **Install MySQL** by following the installation wizard.
3. **Download MySQL Workbench** and install it by dragging it to the Applications folder.
4. **Set up a MySQL connection** in Workbench and test the connection.