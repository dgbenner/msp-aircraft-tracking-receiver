# RealADSB on Apple TV

A guide to setting up aircraft tracking on Apple TV using the RealADSB app.

## Overview

RealADSB is an aircraft tracking application that displays real-time ADS-B (Automatic Dependent Surveillance-Broadcast) data from nearby aircraft. Similar to FlightRadar24, but designed to work with your own receiver or public feeds.

The app shows:
- Commercial, private, and military aircraft
- Altitude, speed, heading, callsign
- Aircraft registration, type, origin/destination
- Map view with airport overlays and weather radar

## Connection Options

RealADSB requires a data source. You have three options:

### 1. Public Feeds (Easiest - No Hardware)

The app mentions three public feeds, though connection details aren't publicly advertised:
- **KEWR** - Newark Liberty International Airport area
- **EGCN** - United Kingdom coverage
- **EDDC** - Germany coverage

**To access:** Check the app's built-in "Public Feeds" or "Connections" settings. The developer may provide access details via email: admin@realadsb.com

### 2. Airplanes.live API (No Hardware)

Starting with version 2.12, RealADSB integrated with Airplanes.live API for global coverage without requiring your own receiver. This should be available as a built-in option in the app settings.

### 3. Your Own ADS-B Receiver (Best Coverage)

Build your own receiver for comprehensive local coverage (100-250 mile radius).

## Building Your Own Receiver

### Hardware Required

**Basic Setup (~$50-80):**
- Raspberry Pi (any model with USB, Pi Zero W works)
- MicroSD card (16GB minimum)
- RTL-SDR USB dongle (~$25-35)
  - RTL-SDR Blog V3 (recommended)
  - NooElec NESDR Smart
  - FlightAware Pro Stick
- ADS-B antenna (1090 MHz)
  - Commercial antenna (~$20-40), or
  - DIY cantenna/spider antenna (nearly free)
- Power supply for Raspberry Pi
- (Optional) Weatherproof enclosure if mounting outdoors

**Recommended Upgrades:**
- LNA (Low Noise Amplifier) for better range
- Quality coaxial cable if antenna is remote
- Bandpass filter to reduce interference

### Software Options

Several pre-configured images available:

1. **PiAware** (FlightAware)
   - Most popular, well-documented
   - Includes dump1090 (provides data on port 30005)
   - Web interface at `http://[pi-ip]/`

2. **Stratux**
   - Originally for pilots/EFB apps
   - Provides SBS-1 format on port 30003
   - Web UI at `http://192.168.10.1`

3. **ADSBexchange**
   - Community-focused
   - Good for supporting open data

4. **Standalone dump1090**
   - Minimal setup
   - Just the decoder, no extras

### Basic Setup Steps

1. Flash SD card with chosen image (use Etcher or Raspberry Pi Imager)
2. Boot Raspberry Pi with RTL-SDR dongle and antenna connected
3. Find Pi's IP address (check your router or use network scanner)
4. Verify data feed:
   - PiAware: `http://[pi-ip]:8080` 
   - Stratux: `http://192.168.10.1`
5. In RealADSB app, enter: `[pi-ip]:30005` (Beast format) or `[pi-ip]:30003` (SBS-1 format)

## Using adsb_hub3 (Optional)

For advanced users, adsb_hub3 is a Java application that:
- Aggregates multiple data sources
- Filters and compresses data for mobile devices
- Supports Bonjour auto-discovery on local network
- Enables API integrations (OpenSky, ADS-B Exchange)

Download from: http://www.realadsb.com

## Connection Format

When entering connection details in RealADSB:

**Local receiver:**
```
192.168.1.100:30005
```

**With airport ICAO code:**
```
192.168.1.100:4567:KEWR
```
(Sets reference location to Newark airport)

## Antenna Placement Tips

- Height matters - roof mounting gives best results
- Clear line of sight to sky
- Away from metal objects and electronics
- South-facing in Northern Hemisphere (most traffic)
- USB extension cable if needed (max 15ft without active extension)

## Cost Breakdown

**DIY Receiver Option:**
- Raspberry Pi Zero W: ~$15
- RTL-SDR dongle: ~$30
- Antenna: ~$25 (or DIY for ~$5)
- SD card: ~$8
- Power supply: ~$8
- **Total: ~$85**

**No-Hardware Option:**
- RealADSB app: $4.99 (one-time purchase)
- Use public feeds or Airplanes.live API
- **Total: ~$5**

## Troubleshooting

**App won't connect:**
- Verify Pi is on same network as Apple TV
- Check firewall settings
- Ensure dump1090 is running: `ps aux | grep dump1090`
- Try port 30003 instead of 30005

**No aircraft showing:**
- Verify antenna connection
- Check antenna placement
- May take 5-10 minutes for first aircraft
- Rural areas have less traffic

**Slow performance:**
- Use adsb_hub3 to filter data
- Reduce refresh rate
- Check network congestion

## Resources

- RealADSB official site: http://www.realadsb.com
- Developer contact: admin@realadsb.com
- FlightAware PiAware: https://flightaware.com/adsb/piaware/
- RTL-SDR Blog: https://www.rtl-sdr.com/
- Reddit r/RTLSDR community

## Technical Details

**ADS-B Frequency:** 1090 MHz  
**Typical Range:** 100-250 miles (line of sight)  
**Data Formats Supported:**
- Beast (port 30005)
- SBS-1 BaseStation (port 30003)
- adsb_hub3 custom format

**App Requirements:**
- Apple TV (tvOS)
- iOS/iPadOS 14.0+
- macOS (Apple Silicon or Intel)

## Privacy & Data Sharing

When using public feeds or services like FlightAware:
- Your receiver data may be shared with the community
- Check each service's data sharing policy
- adsb_hub3 can be configured for private use only

## License

This README is provided as-is for educational purposes. RealADSB is developed by Nikolay Klimchuk. Hardware and software mentioned are trademarks of their respective owners.

---

*Last updated: November 2024*
