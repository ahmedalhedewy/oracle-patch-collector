# Oracle Patch Collector

A Python script that connects to multiple Oracle servers via SSH, retrieves patch information using OPatch commands from all Oracle homes, and exports the results to an Excel file.

## Features

- **Multi-server support**: Connect to multiple Oracle servers simultaneously
- **Automatic Oracle home detection**: Finds all Oracle installations using multiple detection methods
- **Comprehensive patch information**: Collects database, OJVM, and OCW patch releases
- **Excel export**: Results exported to timestamped Excel files
- **Retry mechanism**: Built-in connection retry with fallback credential options
- **Error handling**: Robust error handling for network and authentication issues

## Requirements

- Python 3.6+
- SSH access to Oracle servers
- Oracle servers with OPatch utility installed

## Installation

1. Clone this repository:
```bash
git clone https://github.com/yourusername/oracle-patch-collector.git
cd oracle-patch-collector
```

2. Install required dependencies:
```bash
pip install -r requirements.txt
```

## Usage

### Basic Usage

Run the script and follow the prompts:

```bash
python oracle_patch_collector.py
```

### Input Methods

**Option 1: Direct input**
```
Enter IP addresses/hostnames (comma-separated or from a file path): 192.168.1.10,192.168.1.11,oracledb01.company.com
```

**Option 2: From file**
Create a text file with one server per line:
```
servers.txt:
192.168.1.10
192.168.1.11
oracledb01.company.com
```

Then specify the file path:
```
Enter IP addresses/hostnames (comma-separated or from a file path): servers.txt
```

### Authentication

The script will first attempt to connect using the default `oracle` username. If authentication fails, you'll be prompted to enter alternative credentials for each server.

### Output

The script generates an Excel file named `oracle_patches_YYYYMMDD_HHMMSS.xlsx` containing:

| Column | Description |
|--------|-------------|
| Hostname | Server hostname/IP |
| SID | Oracle database SID |
| Oracle Home | Path to Oracle installation |
| Oracle Version | Oracle database version |
| OPatch Version | OPatch utility version |
| Database Release | Latest database patch release |
| OJVM Release | Latest OJVM patch release |
| OCW Release | Latest OCW patch release |

## Oracle Home Detection

The script uses multiple methods to detect Oracle installations:

1. **oratab file parsing** - Most reliable method (`/etc/oratab` or `/var/opt/oracle/oratab`)
2. **Directory search** - Searches common Oracle paths (`/u01`, `/opt`, `/oracle`)
3. **Environment variables** - Checks `ORACLE_HOME` environment variable
4. **Fallback default** - Uses common default path if no installations found

## Error Handling

- Connection timeouts and retries
- Authentication failure recovery
- Missing OPatch utility detection
- Graceful handling of inaccessible Oracle homes
- Non-destructive error reporting

## Security Considerations

- Passwords are not stored or logged
- SSH connections use paramiko with proper host key handling
- Connection attempts are limited to prevent account lockouts
- Supports secure credential retry mechanism

## Troubleshooting

### Common Issues

**Connection Failures**
- Verify SSH access to target servers
- Check network connectivity and firewall rules
- Ensure correct username and password

**No Oracle Homes Found**
- Verify Oracle installations exist on target servers
- Check that the oracle user has proper permissions
- Manually verify oratab file exists and is readable

**OPatch Not Found**
- Ensure OPatch utility is installed in Oracle homes
- Verify Oracle home paths are correct
- Check that OPatch has execute permissions

## Support

If you find this tool useful and would like to support its development, consider making a donation:

[![PayPal](https://img.shields.io/badge/PayPal-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://paypal.me/AhmedAlhedewy?country.x=EG&locale.x=en_US)

Your support helps maintain and improve this project!

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Built using [paramiko](https://www.paramiko.org/) for SSH connections
- Excel export powered by [openpyxl](https://openpyxl.readthedocs.io/)
