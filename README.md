# Windows-print-server-setup
# 🖨️ Windows Print Server Setup Guide

This guide provides complete, step-by-step instructions to install and configure a printer on a **Windows Print Server**, from role installation to printer deployment via Group Policy.

---

## 📌 Prerequisites

- A server running **Windows Server (2016/2019/2022)**
- Administrator privileges on the server
- Printer connected via USB or network (with a static IP address)
- Appropriate printer drivers (downloaded from manufacturer website if needed)

---

## ✅ Step 1: Install Print Server Role

1. Open **Server Manager**
2. Go to **Manage** → **Add Roles and Features**
3. Choose **Role-based or feature-based installation**
4. Select the local server
5. Under **Server Roles**, check:
   - `Print and Document Services`
     - Expand and check `Print Server`
     - *(Optional)*: Add `Internet Printing` or `LPD Service` if required
6. Click **Next** and then **Install**
7. Wait for installation to complete

---

## ✅ Step 2: Connect the Printer

- **Option A**: USB - Plug the printer directly into the server
- **Option B**: Network - Ensure the printer has an IP address on your network

---

## ✅ Step 3: Install Printer Driver (Before Adding the Printer)

1. Open **Print Management Console**  
   (`Win + R` → `printmanagement.msc`)
2. Expand **Print Servers** → Your Server → Right-click **Drivers** → Select **Add Driver**
3. Follow the wizard:
   - Select **x64** or **x86** platform
   - Choose manufacturer/model OR click **Have Disk** to provide `.inf` file
4. Complete the wizard to install the driver

---

## ✅ Step 4: Add the Printer

1. Still in **Print Management**, right-click **Printers** → Select **Add Printer**
2. Choose one of:
   - `Add a new TCP/IP or Web Services printer by IP address or hostname`
   - `Add a new printer using an existing port`
3. Enter the IP address or choose the correct port
4. Select the driver you installed in Step 3
5. Name the printer (e.g., `HP_LaserJet_3rdFloor`)
6. Choose to **Share the printer**
7. Print a test page to confirm installation

---

## ✅ Step 5: Set Printer Permissions (Optional)

1. Right-click the printer → `Printer Properties` → `Security` tab
2. Set access:
   - Users: **Print**
   - Admins: **Manage Printers**, **Manage Documents**

---

## ✅ Step 6: Deploy via Group Policy (Optional)

1. In **Print Management**, right-click the printer → `Deploy with Group Policy`
2. Choose an existing or new **GPO**
3. Select:
   - `The users that this GPO applies to (per user)`
   - or `The computers that this GPO applies to (per machine)`
4. Click **Add** → **OK**

---

## ✅ Step 7: Test from a Client Machine

- Run `gpupdate /force` (optional)
- Open **Devices and Printers** → Confirm the printer is visible
- Print a test document

---

## ✅ Optional: Add Additional Drivers

1. On the server: Right-click printer → `Printer Properties` → `Sharing` tab
2. Click **Additional Drivers**
3. Add x86/x64 drivers as needed for mixed environments

---

## 🛠️ PowerShell Installation (Optional)

```powershell
# Install Print Server Role
Install-WindowsFeature Print-Server

# Add printer port (replace with your IP)
Add-PrinterPort -Name "IP_192.168.1.100" -PrinterHostAddress "192.168.1.100"

# Install printer (replace driver name with your specific driver)
Add-Printer -Name "HP_LaserJet_3rdFloor" -PortName "IP_192.168.1.100" -DriverName "HP Universal Printing PCL 6" -Shared -ShareName "HP_LaserJet_3rdFloor"
