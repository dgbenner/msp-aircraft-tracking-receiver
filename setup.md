# Setup Log

## SD Card Flashing (December 12, 2024)

**Issue with Raspberry Pi Imager:**
- Attempted to flash PiAware image using Raspberry Pi Imager v2.0.0
- Image appeared to write successfully but macOS couldn't mount FAT32 partition
- Error: "Operating system did not mount FAT32 partition"
- Verification failed despite image being present on card

**Solution - Used Balena Etcher instead:**
- Downloaded Balena Etcher (arm64 version for Apple Silicon Mac)
- Successfully flashed piaware-sd-card-10.0.img.zip
- BOOTFS partition mounted correctly
- Able to edit piaware-config.txt for WiFi configuration

**Lesson learned:** For macOS users flashing Linux images, Etcher handles partition mounting more reliably than Raspberry Pi Imager.


## Hardware Assembly and First Boot (December 12, 2024)

### Hardware Setup

Successfully assembled the receiver with the following components:
- Raspberry Pi Zero 2 W
- RTL-SDR Blog V3 dongle
- ADS-B antenna (1090 MHz)
- 8GB microSD card (flashed with PiAware 10.0)
- Micro-USB OTG adapter
- USB power supply

**Assembly notes:**
- microSD card insertion has subtle click (not always obvious when seated)
- Pi Zero 2 W has two micro-USB ports side by side:
  - PWR (outer port) = Power
  - USB (inner port) = Data/OTG adapter
- RTL-SDR connects via OTG adapter to data port
- Antenna screws onto gold SMA connector on RTL-SDR

**Placement:**
- Upper floor, north-facing window
- Indoor setup approximately 3 miles from MSP airport
- Antenna positioned near window (not outdoor mounting required)

### First Boot and WiFi Configuration

**WiFi setup process:**
1. After flashing SD card with Etcher, BOOTFS partition mounted successfully
2. Edited existing `piaware-config.txt` file on BOOTFS partition
3. Changed `wireless-ssid` and `wireless-password` values
4. Left `wireless-network yes` unchanged
5. Saved and ejected card

**Boot sequence:**
- First boot took approximately 5 minutes
- Green LED pattern: initially rapid blinking, then settled to mostly-on with brief blinks every few seconds
- This steady pattern indicates successful boot and normal operation

**Network discovery:**
- System accessible via hostname: `piaware.local`
- Also assigned two IP addresses:
  - 192.168.1.123 (likely wired network config, though no ethernet connected)
  - 10.0.0.219 (WiFi connection)
- Used `ping piaware.local` in Terminal to discover IP addresses

### Web Interface Success

**Access method:**
- Browser URL: `http://piaware.local:8080`
- Displays SkyAware map interface with real-time aircraft data

**Initial results:**
- Within 10 minutes of first boot, receiving 3-6 aircraft consistently
- Aircraft data includes:
  - Position, altitude, speed, heading
  - Flight numbers (Canadian and US flights visible)
  - Real-time updates every second
- PiAware status page shows all systems functioning (green indicators)
- System uptime tracking correctly
- MLAT shows as not yet synchronized (normal for new installation)

**Performance observations:**
- Indoor antenna placement working well despite north-facing window
- Proximity to MSP airport (3 miles) providing good low-altitude coverage
- No outdoor antenna needed for initial operation

### Apple TV App Troubleshooting (Ongoing Issue)

**App:** RealADSB for Apple TV (tvOS)
**Problem:** App connects but does not display aircraft or increment counter

**Connection attempts tested:**
1. `10.0.0.219:30005` - Green flashing indicator, no aircraft display
2. `10.0.0.219:30003` - Same result
3. `192.168.1.123:30005` - Green flashing, counter stays at zero
4. `192.168.1.123:30003` - No change
5. `piaware.local:30005` - Same issue persists

**Observations:**
- Green flashing indicator suggests data packets are being received
- Counter in upper-left remains at all zeros
- No aircraft icons appear on map
- Same network as Mac (verified)
- Web interface on other devices works perfectly with same data source

**Settings adjusted in app:**
- Info window size: Large
- Show info for: All aircraft
- Distance from airport: Left blank (to show all)
- No improvement after adjustments

**Ports tested:**
- Port 30005 (Beast format) - No display
- Port 30003 (SBS-1 BaseStation format) - No display

**Current status:**
- Issue reported to developer (admin@realadsb.com)
- Waiting for response or troubleshooting guidance
- Workaround: Using web browser on other devices to view aircraft

**Hypothesis:**
- Possible compatibility issue with Apple TV version of RealADSB
- Web interface proves receiver and data streams are working correctly
- Issue appears specific to tvOS app implementation

### Workarounds and Alternatives

**Current viewing methods that work:**
- Mac browser: `http://piaware.local:8080` (bookmarked as "PiAware Local")
- iPhone/iPad browser: Same URL
- Any device on local network can access web interface

**Future possibilities:**
- Build custom web interface (hybrid tracker project planned)
- Try alternative Apple TV viewing apps if available
- Use AirPlay from Mac/iPad to display on TV (temporary solution)

### Next Steps

1. Wait for developer response regarding Apple TV app issue
2. Continue monitoring web interface for performance baseline
3. Consider claiming PiAware receiver on FlightAware.com for statistics tracking
4. Explore additional antenna placement options for range optimization
5. Plan for weather satellite receiver addition (separate project)

### Lessons Learned

**SD card flashing:**
- Balena Etcher more reliable than Raspberry Pi Imager for Linux images on macOS
- FAT32 mounting errors are normal - don't indicate failure
- Verify with `diskutil list` rather than relying on Finder visibility

**WiFi configuration:**
- Editing `piaware-config.txt` directly is straightforward
- File already exists on BOOTFS partition with examples
- No need to create from scratch

**Hardware assembly:**
- Port identification crucial - double-check PWR vs USB labels
- Indoor placement viable for initial testing
- Proximity to major airport (MSP) provides excellent coverage even indoors

**Network troubleshooting:**
- `ping hostname.local` useful for finding IP addresses
- Multiple IP assignments possible (wired + wireless configs)
- Hostname access (piaware.local) often more reliable than IP addresses

**Performance expectations:**
- 3-6 aircraft visible immediately reasonable for indoor setup
- Coverage adequate without outdoor antenna mounting
- Upper floor placement beneficial for line-of-sight


### Apple TV App Troubleshooting (Ongoing Issue)

[your existing troubleshooting content]

**Developer Response (December 13, 2025):**

Received response from N_kolay Klimch_k (RealADSB Support Team) with troubleshooting suggestions:

**Developer's working setup:**
- Raspberry Pi 5 with PiAware
- Connection: `192.168.1.216:30003`
- Successfully displaying traffic on Apple TV

**Potential issues to investigate:**

1. **Network segmentation:** Pi on 2.4GHz WiFi, Apple TV on 5GHz
   - Both devices must be on same WiFi network/band
   - Check router settings for device connections

2. **Verify dump1090 configuration:**
   - SSH command to check: `ps -aux | grep 1090`
   - Should show `—net-sbs-port 30003` in parameters
   - Confirms SBS-1 feed is available

3. **Port 30003 limitations:**
   - Lacks MLAT (multilateration) traffic
   - Only shows direct ADS-B reception
   - Developer suggests adsb_hub for combined feed<img width="1280" height="89" alt="images:screenshot-from-dev" src="https://github.com/user-attachments/assets/d9d3be93-a471-46a6-882c-e9a1bfb028d4" />


**Next steps to try:**
- [ ] Verify both devices on same WiFi network
- [ ] Enable SSH and run diagnostic command
- [ ] Consider installing adsb_hub for port 4567 combined feed

**Status:** Awaiting SSH access to run diagnostics

---

*Updated: December 12, 2025 - Initial hardware setup and troubleshooting*
