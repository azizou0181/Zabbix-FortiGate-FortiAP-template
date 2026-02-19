# Zabbix Template: FortiGate SNMP + FortiAP Monitoring

Based on the original [fortinet-zabbix](https://github.com/mbdraks/fortinet-zabbix) template by Michel Barbosa (v2.1.0), with added FortiAP wireless access point monitoring.

---

## What's Inside

### Original Template (Unchanged)
All original monitoring from `Template Net Fortinet FortiGate SNMP` is preserved, including interface discovery (`net.if.walk`), CPU, memory, system uptime, VPN tunnels, and HA status.

### Added: FortiAP Discovery
A new SNMP discovery rule (`fortiap.discovery`) has been added alongside the existing interface discovery. It automatically detects all FortiAP access points managed by the FortiGate wireless controller.

**Discovery Rule:**
- Key: `fortiap.discovery`
- OID: `1.3.6.1.4.1.12356.101.14.4.3.1.1` (FortiGate proprietary MIB)
- Interval: 1 hour

**Item Prototype:**
- `FortiAP [{#SNMPVALUE}]: Connection State` — monitors online/offline status, updated every minute

**Trigger Prototype:**
- `FortiAP [{#SNMPVALUE}] is offline` — HIGH severity, fires when state = 1 (down) or 3 (offline), auto-recovers when state = 2 (online)

**Value Mapping (FortiAP Connection State):**
| Value | State |
|-------|-------|
| 1 | down |
| 2 | online |
| 3 | offline |
| 4 | rogue |
| 5 | initializing |
| 6 | firmware-update |

---

## Requirements

- Zabbix 5.2+
- FortiOS 7.x
- SNMP v2c enabled on FortiGate
- Zabbix server IP allowed in FortiGate SNMP community

---

## Import

1. Download the template file
2. In Zabbix: **Configuration → Templates → Import**
3. Select the file and click **Import**
4. Link template to your FortiGate host
5. Execute discovery manually or wait up to 1 hour for FortiAPs to appear

---

## Credits

Original template: [Michel Barbosa](https://github.com/mbdraks/fortinet-zabbix)  
FortiAP additions: this repository
