# MSP Aircraft Tracking Receiver

A DIY ADS-B receiver project for tracking aircraft in the central and south Minnesota, and western Wisconsin areas.

## Overview

This project involves building your own ADS-B (Automatic Dependent Surveillance-Broadcast) receiver to track aircraft in real-time. Aircraft broadcast their position, altitude, speed, and other data on 1090 MHz, which you can receive directly with inexpensive hardware.

**What you'll see:**
- Commercial, private, and military aircraft within 100-250 miles
- Real-time position, altitude, speed, heading, callsign
- Aircraft registration, type, origin/destination information
- Typical range: 100-250 miles line-of-sight

## Hardware Required

### Basic Setup (~$75-85)

- **Raspberry Pi Zero W**: ~$15 (any Pi model with USB works)
- **RTL-SDR USB dongle**: ~$30
  - RTL-SDR Blog V3 (recommended)
  - NooElec NESDR Smart
  - FlightAware Pro Stick
- **ADS-B antenna (1090 MHz)**: ~$20-25
  - Commercial magnetic mount antenna, or
  - DIY cantenna/spider antenna (nearly free)
- **MicroSD card**: ~$8 (16GB minimum)
- **USB power supply**: ~$8 (or use existing phone charger)

**Total: ~$75-85**

### Optional Upgrades

- LNA (Low Noise Amplifier) for better range
- Quality coaxial cable if antenna is remote mounted
- Bandpass filter to reduce interference
- Weatherproof enclosure for outdoor mounting

## Antenna Placement

### Indoor Setup
- Near a window (upper floors work best)
- Away from electronics and USB chargers (minimize interference)
- Vertical orientation preferred
- South-facing windows ideal (Northern Hemisphere - most traffic routes)
- Magnetic base antennas work well on metal surfaces (filing cabinets, shelves)



## Software Options

Several pre-configured images are available for Raspberry Pi:

### PiAware (FlightAware)
- Most popular option
- Well-documented with active community
- Includes dump1090 decoder (provides data on port 30005)
- Web interface at `http://[pi-ip]:8080`
- Shares data with FlightAware community

### Stratux
- Originally designed for pilots and EFB apps
- Provides SBS-1 format on port 30003
- Web UI at `http://192.168.10.1`
- Good for portable setups

### ADS-B Exchange
- Community-focused, open data philosophy
- Good for supporting unfiltered flight data
- Active development community

### Standalone dump1090
- Minimal installation
- Just the decoder, no extras
- Good for custom integrations

## Setup Steps

1. **Flash SD card** with chosen image (use Etcher or Raspberry Pi Imager)
2. **Connect hardware**: Insert SD card, attach RTL-SDR dongle and antenna
3. **Boot Raspberry Pi** and wait for startup (~2 minutes)
4. **Find Pi's IP address**:
   - Check your router's connected devices, or
   - Use network scanner tool
5. **Verify data feed** in web browser:
   - PiAware: `http://[pi-ip]:8080`
   - Stratux: `http://192.168.10.1`
6. **Access data streams**:
   - Beast format: `[pi-ip]:30005`
   - SBS-1 format: `[pi-ip]:30003`

## Technical Details

**ADS-B Frequency**: 1090 MHz  
**Typical Range**: 100-250 miles (line of sight)  
**Data Output Formats**:
- Beast (port 30005)
- SBS-1 BaseStation (port 30003)

**What limits range:**
- Antenna height and placement
- Antenna quality
- RF interference
- Terrain and obstructions
- Aircraft altitude (higher planes = longer range)

## Advanced: adsb_hub3

For advanced users, adsb_hub3 is a Java application that provides:
- Aggregation of multiple data sources
- Data filtering and compression
- Bonjour auto-discovery on local network
- API integrations (OpenSky, ADS-B Exchange)

Download from: http://www.realadsb.com

## Indoor Reception at MSP

**Special note for Minneapolis area (3 miles from MSP airport):**

An indoor setup near a window on an upper floor should provide excellent results:
- Strong signals from aircraft on approach/departure to MSP
- Low-altitude traffic clearly visible
- 50-100+ mile range typical for indoor setups
- You'll see planes banking, descending, lining up for landing
- Even north-facing windows work well at this proximity

## Troubleshooting

**No aircraft showing:**
- Verify antenna connection is secure
- Check antenna placement (try moving to window)
- Wait 5-10 minutes for first detections
- Rural areas naturally have less traffic
- Verify dump1090 is running: `ps aux | grep dump1090`

**Limited range:**
- Try relocating antenna higher or near window
- Check for RF interference from nearby electronics
- Consider antenna upgrade or LNA
- Verify antenna is 1090 MHz specific

**Connection issues:**
- Verify Pi is on same network as viewing device
- Check firewall settings
- Try alternative port (30003 instead of 30005)
- Restart dump1090 service

## Data Access

Once your receiver is running, the data can be accessed by:
- Any compatible viewing application
- Web browser (built-in interfaces)
- Custom software you develop
- Multiple devices simultaneously

## Privacy & Data Sharing

When using services like FlightAware or ADS-B Exchange:
- Your receiver data may be shared with the community
- Check each service's data sharing policy
- Some software can be configured for private use only
- You control what data leaves your network

## Resources

- **FlightAware PiAware**: https://flightaware.com/adsb/piaware/
- **RTL-SDR Blog**: https://www.rtl-sdr.com/
- **ADS-B Exchange**: https://www.adsbexchange.com/
- **Reddit r/RTLSDR**: Active community for support and ideas
- **FlightRadar24 Blog**: Technical articles about ADS-B

## Next Steps

1. Order hardware components
2. Choose software (PiAware recommended for beginners)
3. Set up receiver near window
4. Monitor initial results
5. Experiment with antenna placement
6. Consider outdoor mounting if desired
7. Explore data visualization options

## License

This project documentation is provided as-is for educational purposes. Hardware and software mentioned are trademarks of their respective owners.

---

*Last updated: November 2024*
