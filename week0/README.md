
# RISC-V Reference SoC Tapeout Program VSD

## Tools Installation

#### <ins>All the instructions for installation of required tools can be found here:</ins>

### **System Requirements**
- RAM: 6 GB or more
- Disk Space: 50 GB HDD (minimum)
- Operating System: CentOS 9 (Stream)
- CPU: 4 vCPUs or higher

### **TOOL CHECK**

#### <ins>**Yosys**</ins>
```bash
# 1. Update the system
sudo dnf update -y      # For CentOS 8+ or use `yum update -y` on older versions

# 2. Install essential tools
sudo dnf groupinstall "Development Tools" -y
sudo dnf install epel-release -y
sudo dnf install clang bison flex readline-devel gawk tcl-devel \
    libffi-devel git graphviz xdot pkgconfig python3 boost-devel zlib-devel -y

# 3. Clone Yosys repository
git clone https://github.com/YosysHQ/yosys.git
cd yosys

# 4. Configure build
make config-gcc

# 5. Initialize submodules
git submodule update --init --recursive

# 6. Build Yosys
make

# 7. Install Yosys system-wide
sudo make install

# 8. Verify installation
yosys -V

```
![Alt Text](https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/week0/images/Image_Yosys.png)

#### <ins>**Iverilog**</ins>
```bash
# Update package metadata
sudo dnf update -y        # or `sudo yum update -y` on older CentOS

# Install Icarus Verilog
sudo dnf install iverilog -y

```
![Alt Text](https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/week0/images/Image_iverilog.png)

#### <ins>**gtkwave**</ins>
```bash
# Update package metadata
sudo dnf update -y       # or `sudo yum update -y` on older versions

# Install GTKWave
sudo dnf install gtkwave -y

```
![Alt Text](https://github.com/ShashwatSinghal123/RISC-V-SOC-TAPEOUT-VSD/blob/main/week0/images/Image_gtkwave.png)
