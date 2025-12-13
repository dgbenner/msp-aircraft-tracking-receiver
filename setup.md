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
