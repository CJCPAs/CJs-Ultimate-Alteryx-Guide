# Installation & Setup

[Home](../../README.md) > [Getting Started](./README.md) > Installation

---

## System Requirements

### Minimum Requirements
| Component | Requirement |
|-----------|-------------|
| **Operating System** | Windows 10/11 (64-bit) or Windows Server 2016+ |
| **Processor** | Intel or AMD x64 processor |
| **RAM** | 8 GB minimum |
| **Storage** | 10 GB free space |
| **Display** | 1024 x 768 resolution |

### Recommended Requirements
| Component | Recommendation |
|-----------|----------------|
| **RAM** | 16 GB or more |
| **Storage** | SSD with 50+ GB free |
| **Processor** | Multi-core processor (4+ cores) |
| **Display** | 1920 x 1080 or higher |

### Important Notes
- Alteryx Designer runs **only on Windows**
- Mac users need virtualization (Parallels, VMware) or cloud solutions
- More RAM = better performance with large datasets
- SSD storage significantly improves workflow speed

---

## Download Options

### 1. Free Trial
Get a 14-day free trial of Alteryx Designer:
1. Visit [alteryx.com/free-trial](https://www.alteryx.com/designer-trial/free-trial)
2. Fill out the registration form
3. Download the installer
4. Receive your trial license via email

### 2. Licensed Version
If your organization has Alteryx licenses:
1. Contact your IT department or Alteryx administrator
2. Obtain the installer and license key
3. Follow enterprise installation procedures

### 3. Alteryx for Good
Non-profits may qualify for free licenses:
- Visit [alteryx.com/alteryx-for-good](https://www.alteryx.com/alteryx-for-good)

### 4. Academic Program
Students and educators can access free licenses:
- Visit [community.alteryx.com/t5/Alteryx-Academy](https://community.alteryx.com/t5/Alteryx-Academy/ct-p/alteryx-academy)

---

## Installation Steps

### Step 1: Run the Installer
1. Double-click the downloaded `.exe` file
2. If prompted by Windows Security, click **Run anyway**
3. Accept the license agreement

### Step 2: Choose Installation Type
- **Typical** - Recommended for most users (installs all features)
- **Custom** - Choose specific components to install

### Step 3: Select Installation Location
Default: `C:\Program Files\Alteryx\`

Considerations:
- Use the default unless you have specific requirements
- Ensure the drive has sufficient space
- Avoid network drives for installation

### Step 4: Complete Installation
1. Click **Install**
2. Wait for installation to complete (5-15 minutes)
3. Click **Finish**

---

## Activation

### License Key Activation
1. Launch Alteryx Designer
2. Go to **Help** > **Manage Licenses**
3. Enter your license key
4. Click **Activate**

### Offline Activation
If your computer doesn't have internet access:
1. Go to **Help** > **Manage Licenses** > **Offline Activation**
2. Generate an activation request file
3. Upload the file at [licenses.alteryx.com](https://licenses.alteryx.com)
4. Download and import the response file

### Trial Activation
Trial licenses typically activate automatically after download. Check your email for the license key if needed.

---

## Post-Installation Setup

### Configure User Settings
Go to **Options** > **User Settings** > **Edit User Settings**

**Recommended settings:**

#### General Tab
- **Default Workflow Directory**: Set to your preferred folder
- **Temp Files Directory**: Use an SSD location if available

#### Canvas Tab
- **Connection Progress**: Show (helps debugging)
- **Zoom**: Adjust default zoom level for your display

#### Defaults Tab
- **Default Input Format Settings**: Configure date formats, delimiters
- **Default Output Format Settings**: Set preferred output options

### Install Data Drivers
For database connectivity, install appropriate drivers:

| Database | Driver Needed |
|----------|---------------|
| SQL Server | SQL Server Native Client or ODBC Driver |
| Oracle | Oracle Client |
| PostgreSQL | PostgreSQL ODBC Driver |
| MySQL | MySQL ODBC Connector |
| Snowflake | Snowflake ODBC Driver |

### Configure Proxy Settings (If Required)
For corporate networks with proxy servers:
1. Go to **Options** > **User Settings** > **Edit User Settings**
2. Navigate to **Proxy** tab
3. Enter proxy server details

---

## Verify Installation

### Test Basic Functionality
1. Launch Alteryx Designer
2. Drag an **Input Data** tool to the canvas
3. Browse to any CSV or Excel file
4. Run the workflow (Ctrl+R)
5. Verify data appears in the Results window

### Check Tool Availability
1. Go to the Tool Palette on the left side
2. Ensure all tool categories are visible
3. Search for "Formula" - tool should appear

---

## Common Installation Issues

### Issue: Installation Fails
**Solutions:**
- Run installer as Administrator
- Disable antivirus temporarily
- Ensure sufficient disk space
- Check Windows Event Viewer for errors

### Issue: License Activation Fails
**Solutions:**
- Verify license key is correct (no extra spaces)
- Check internet connectivity
- Ensure firewall allows Alteryx traffic
- Contact your Alteryx administrator

### Issue: Tools Missing
**Solutions:**
- Re-run installer and select **Repair**
- Ensure you have the correct Alteryx product (Designer vs. Server)
- Some tools require additional licenses (Predictive, Spatial)

### Issue: Slow Performance After Install
**Solutions:**
- Increase Windows virtual memory
- Move temp directory to SSD
- Close unnecessary background applications
- Check Alteryx system requirements

---

## Updating Alteryx

### Check for Updates
Go to **Help** > **Check for Updates**

### Update Best Practices
1. **Backup workflows** before updating
2. **Test critical workflows** after updating
3. **Read release notes** for breaking changes
4. **Coordinate with team** if sharing workflows

### Version Compatibility
- Workflows created in newer versions may not open in older versions
- Alteryx generally maintains backward compatibility
- Test workflows after major version updates

---

## Multiple Version Installation

You can install multiple Alteryx versions side-by-side:
1. Install each version to a different directory
2. Useful for testing workflows across versions
3. Be aware of which version opens .yxmd files by default

---

## Uninstallation

### Standard Uninstall
1. Go to **Windows Settings** > **Apps**
2. Find **Alteryx Designer**
3. Click **Uninstall**

### Complete Removal
For complete removal including settings:
1. Uninstall via Windows
2. Delete: `C:\Users\[Username]\AppData\Roaming\Alteryx`
3. Delete: `C:\ProgramData\Alteryx` (requires admin rights)

---

## Next Steps

After successful installation:
1. [Learn the Alteryx Interface →](03-Interface-Overview.md)
2. [Build Your First Workflow →](04-First-Workflow.md)

---

## Additional Resources

- [Alteryx System Requirements (Official)](https://help.alteryx.com/current/en/installation-and-licensing.html)
- [Alteryx Community Installation Forum](https://community.alteryx.com/t5/Alteryx-Designer/bd-p/alteryx-designer)
