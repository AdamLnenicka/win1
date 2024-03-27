
1. **Launching the Windows Deployment Services Console:**
   - Start the Windows Deployment Services console.

2. **Configuring the WDS Server:**
   - Go to Tools -> Action -> Windows Deployment Services -> Servers -> Configure Server.
   - Select "Respond only to known client" and click "Next."

3. **Adding Windows 10 Installation Image:**
   - Mount the Windows 10 ISO as a virtual DVD in the server.
   - In the WDS Manager, right-click on "Install Images" and select "Add new install image."
   - Create a group named "webinc" and add the image from sources/install.wim.
   
4. **Adding Boot Image:**
   - In the WDS Manager, go to "Boot Images" and select "New boot image."
   - Add the boot image from sources/boot.wim.

5. **Configuring Server Properties:**
   - In the server properties, set "Always continue to PXE for known clients."

6. **Starting the Server:**
   - Start the WDS server.

7. **Configuring Virtual Machine (PCZk2) for Network Boot:**
   - In the virtual machine settings, add a new network adapter with internal connection.
   - Make sure to disable Secure Boot.
   - Start the virtual machine and boot from the network.

8. **Assigning Static MAC Address:**
   - Go to the settings of the newly added network adapter in the virtual machine.
   - Navigate to Advanced Features -> MAC Address and set it to STATIC.
   - Use the MAC address 00155D6F4909.

9. **Pre-staging the Device in Active Directory:**
   - Switch back to the server and go to Active Directory prestaged device.
   - Add a new device with the PC name and MAC address, and assign it to the "webinc" group.
   - Fill in other details and save.

10. **Restarting the WDS Server:**
    - For safety, restart the WDS server.

11. **Starting the Virtual Machine:**
    - Start the virtual machine.
![alt text](image.png)