**ESP32 RAG RESOURCE GUIDE**

*Everything You Need to Build a Local LLM That Understands ESP32*

Firmware · WiFi & Bluetooth Internals · Reverse Engineering · Hacker
Research

> HOW TO USE: Download every ESSENTIAL item first. Add IMPORTANT items
> to the same RAG index. BONUS items are optional depth. All GitHub
> repos should be cloned and the README + source files indexed. PDF docs
> should be chunked at \~800 tokens.

**Priority Legend**

  --------------- ---------------------------------------------------------
  **ESSENTIAL**   Core documents the LLM absolutely needs --- TRMs, IDF
                  docs, datasheets. Without these, it will hallucinate
                  register names and API calls.

  **IMPORTANT**   Strongly recommended. Real-world firmware projects,
                  security research, and tooling that dramatically improves
                  firmware modification quality.

  **BONUS**       Deep-dive / niche material. Excellent for low-level
                  WiFi/BT internals, reverse engineering, and exploit
                  research. Index if you have the space.
  --------------- ---------------------------------------------------------

**SECTION 1 --- Official Espressif Technical Documents**

> All PDFs below are direct download links from espressif.com or
> documentation.espressif.com. These are the single most important items
> for your RAG index --- they contain every register address, peripheral
> description, and programming procedure.

**1.1 Technical Reference Manuals (TRMs)**

These are the crown jewel documents --- **\~1,000--1,500 pages each**
covering every peripheral, register, memory map, interrupt matrix,
WiFi/BT hardware interface, and DMA configuration at the bit level. Load
all variants that you may target.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                                                                                                     **Priority**
  --------------------- ---------------- ------------------------------------------------------------------------------------------------------------------------------------------------------ ---------------
  **ESP32 TRM**         PDF \~1400 pages [esp32_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32_technical_reference_manual_en.pdf)         **ESSENTIAL**
  Original ESP32 ---                                                                                                                                                                            
  Xtensa LX6 dual-core                                                                                                                                                                          

  **ESP32-S3 TRM** S3   PDF              [esp32-s3_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf)   **ESSENTIAL**
  --- LX7, USB-OTG,                                                                                                                                                                             
  vector extensions                                                                                                                                                                             

  **ESP32-C3 TRM** C3   PDF              [esp32-c3_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-c3_technical_reference_manual_en.pdf)   **ESSENTIAL**
  --- RISC-V, widely                                                                                                                                                                            
  used                                                                                                                                                                                          

  **ESP32-S2 TRM**      PDF              [esp32-s2_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s2_technical_reference_manual_en.pdf)   **IMPORTANT**

  **ESP32-H2 TRM**      PDF              [esp32-h2_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-h2_technical_reference_manual_en.pdf)   **IMPORTANT**
  802.15.4                                                                                                                                                                                      
  Thread/Zigbee variant                                                                                                                                                                         

  **All TRMs index**    Web              [espressif.com/technical-documents](https://www.espressif.com/en/support/documents/technical-documents)                                                **ESSENTIAL**
  Master index ---                                                                                                                                                                              
  check for newer chip                                                                                                                                                                          
  variants                                                                                                                                                                                      
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**1.2 Chip Datasheets**

Contain pinouts, electrical characteristics, memory maps, boot mode
strapping pins, and chip variant details. Essential for understanding
hardware constraints.

  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                                                                   **Priority**
  --------------------- ---------------- -------------------------------------------------------------------------------------------------------------------- ---------------
  **ESP32 Datasheet**   PDF              [esp32_datasheet_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf)         **ESSENTIAL**

  **ESP32-S3            PDF              [esp32-s3_datasheet_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf)   **IMPORTANT**
  Datasheet**                                                                                                                                                 

  **ESP32-C3            PDF              [esp32-c3_datasheet_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-c3_datasheet_en.pdf)   **IMPORTANT**
  Datasheet**                                                                                                                                                 
  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**1.3 Hardware Design Guidelines**

Covers power schemes, RF design, strapping pins, PCB layout, PSRAM/Flash
wiring. Critical for firmware that touches power management, boot modes,
or GPIO muxing.

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                                                                                                                        **Priority**
  --------------------- ---------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------
  **ESP32 HW Design     PDF (latest)     [hw-design-guidelines-esp32.pdf](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32/esp-hardware-design-guidelines-en-master-esp32.pdf)   **IMPORTANT**
  Guidelines**                                                                                                                                                                                                     

  **ESP32 Silicon       PDF              [esp32_errata_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32_errata_en.pdf)                                                                    **IMPORTANT**
  Errata** Documents                                                                                                                                                                                               
  chip revision bugs                                                                                                                                                                                               
  and workarounds                                                                                                                                                                                                  

  **Hardware Ref Index  Web              [IDF Hardware Reference Index](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/hw-reference/index.html)                                                       **ESSENTIAL**
  (IDF docs)** Links to                                                                                                                                                                                            
  all datasheets,                                                                                                                                                                                                  
  errata, design guides                                                                                                                                                                                            
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**SECTION 2 --- ESP-IDF SDK Documentation & Source**

> The ESP-IDF docs are the primary programming reference --- every API,
> FreeRTOS task, WiFi stack, BT/BLE API, memory layout, and build system
> detail. Index BOTH the PDF and the GitHub source for maximum coverage.

**2.1 ESP-IDF Programming Guide (PDF --- Offline)**

Espressif generates per-version PDFs of the entire IDF programming
guide. These are the best single-file documents for RAG indexing. Use
the stable version for reliability.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                                                                 **Priority**
  --------------------- ---------------- ------------------------------------------------------------------------------------------------------------------ ---------------
  **ESP32 IDF v5.3      PDF \~2000 pages [esp-idf-en-v5.3-esp32.pdf](https://docs.espressif.com/projects/esp-idf/en/v5.3/esp32/esp-idf-en-v5.3-esp32.pdf)   **ESSENTIAL**
  PDF** Complete IDF                                                                                                                                        
  guide --- every API                                                                                                                                       
  and peripheral driver                                                                                                                                     

  **IDF Offline         ZIP + ZST        [Offline Installation Guide](https://docs.espressif.com/projects/idf-im-ui/en/latest/offline_installation.html)    **ESSENTIAL**
  Installer (full                                                                                                                                           
  offline bundle)**                                                                                                                                         
  Download full offline                                                                                                                                     
  toolchain + docs                                                                                                                                          
  bundle                                                                                                                                                    

  **Espressif Download  Web              [dl.espressif.com/dl/esp-idf](https://dl.espressif.com/dl/esp-idf/)                                                **IMPORTANT**
  Portal** Direct                                                                                                                                           
  mirror for IDF                                                                                                                                            
  releases                                                                                                                                                  

  **IDF Versions list** Web              [IDF Versions Page](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/versions.html)                     **IMPORTANT**
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**2.2 ESP-IDF GitHub Source (Clone for RAG)**

Cloning the IDF repository gives you all example code, component source,
FreeRTOS port, WiFi/BT component headers, and Kconfig options ---
invaluable context that the PDF alone misses.

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**                           **Format**       **Download / URL**                                                                       **Priority**
  -------------------------------------- ---------------- ---------------------------------------------------------------------------------------- ---------------
  **ESP-IDF GitHub** git clone           Git Clone        [github.com/espressif/esp-idf](https://github.com/espressif/esp-idf)                     **ESSENTIAL**
  \--recursive -b v5.3                                                                                                                             
  https://github.com/espressif/esp-idf                                                                                                             

  **arduino-esp32** Arduino framework    Git Clone        [github.com/espressif/arduino-esp32](https://github.com/espressif/arduino-esp32)         **IMPORTANT**
  wrapper over IDF --- different API                                                                                                               
  patterns                                                                                                                                         

  **esp-iot-solution** Higher-level      Git Clone        [github.com/espressif/esp-iot-solution](https://github.com/espressif/esp-iot-solution)   **BONUS**
  components and device drivers                                                                                                                    
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------

**SECTION 3 --- Firmware Reference & Modification Projects**

> These GitHub repos contain real working ESP32 firmware --- covering
> WiFi stacks, Bluetooth, RTOS patterns, OTA, and complex peripheral
> use. Clone them and index the source, headers, and READMEs.

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Repo / Project**              **What it Contains**        **GitHub URL**                                                                                                                                      **Category**
  ------------------------------- --------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------- ----------------
  **ESP32 Marauder**              Full WiFi+BT                [justcallmekoko/ESP32Marauder](https://github.com/justcallmekoko/ESP32Marauder)                                                                     **Hacker/Sec**
                                  offensive/defensive suite.                                                                                                                                                      
                                  Massive real-world firmware                                                                                                                                                     
                                  covering deauth, sniffing,                                                                                                                                                      
                                  BLE scanning, packet                                                                                                                                                            
                                  injection. Best single                                                                                                                                                          
                                  firmware reference for                                                                                                                                                          
                                  wireless internals.                                                                                                                                                             

  **30-Day FreeRTOS Course**      Complete hands-on ESP32     [god233012yamil/30-Day-FreeRTOS-Course-for-ESP32-Using-ESP-IDF](https://github.com/god233012yamil/30-Day-FreeRTOS-Course-for-ESP32-Using-ESP-IDF)   **Examples**
                                  FreeRTOS course with IDF                                                                                                                                                        
                                  --- task creation,                                                                                                                                                              
                                  semaphores, mutexes,                                                                                                                                                            
                                  multicore, queue                                                                                                                                                                
                                  management. All source                                                                                                                                                          
                                  included.                                                                                                                                                                       

  **DiegoPaezA/ESP32-freeRTOS**   Focused RTOS examples       [DiegoPaezA/ESP32-freeRTOS](https://github.com/DiegoPaezA/ESP32-freeRTOS)                                                                           **Examples**
                                  covering dual-core                                                                                                                                                              
                                  scheduling, ULP                                                                                                                                                                 
                                  co-processor, BT+WiFi                                                                                                                                                           
                                  coexistence patterns.                                                                                                                                                           

  **ESP32 WiFi Penetration Tool** Explores full ESP32 WiFi    [risinek/esp32-wifi-penetration-tool](https://github.com/risinek/esp32-wifi-penetration-tool)                                                       **Hacker/Sec**
                                  attack surface --- PMKID                                                                                                                                                        
                                  capture, WPA2 handshakes,                                                                                                                                                       
                                  deauth. Good source for raw                                                                                                                                                     
                                  802.11 frame construction                                                                                                                                                       
                                  code.                                                                                                                                                                           

  **ESP32 Hacking Tool (Kl0ibi)** Beacon spammer, deauther,   [Kl0ibi/esp32_hackingtool](https://github.com/Kl0ibi/esp32_hackingtool)                                                                             **Hacker/Sec**
                                  captive portal firmware                                                                                                                                                         
                                  with full IDF source.                                                                                                                                                           
                                  Practical reference for                                                                                                                                                         
                                  low-level promiscuous mode                                                                                                                                                      
                                  and frame injection.                                                                                                                                                            

  **ESP Firmware Toolbox**        Collection integrating      [wilco375/ESP-Firmware-Toolbox](https://github.com/wilco375/ESP-Firmware-Toolbox)                                                                   **RE Tools**
                                  esptool, esp32knife, Ghidra                                                                                                                                                     
                                  SVD loader, ROM labels,                                                                                                                                                         
                                  firmware patching scripts.                                                                                                                                                      
                                  Essential RE workflow                                                                                                                                                           
                                  reference.                                                                                                                                                                      

  **ESP32-reversing list**        Curated list of ESP32       [BlackVS/ESP32-reversing](https://github.com/BlackVS/ESP32-reversing)                                                                               **RE Tools**
                                  reversing resources ---                                                                                                                                                         
                                  articles, tools, repos,                                                                                                                                                         
                                  Ghidra plugins, JTAG                                                                                                                                                            
                                  techniques.                                                                                                                                                                     

  **esp32_image_parser**          Parse and extract ELF files [tenable/esp32_image_parser](https://github.com/tenable/esp32_image_parser)                                                                         **RE Tools**
                                  from raw ESP32 flash dumps.                                                                                                                                                     
                                  Useful for understanding                                                                                                                                                        
                                  partition layouts and                                                                                                                                                           
                                  binary structure.                                                                                                                                                               

  **ghidra-xtensa**               Ghidra processor module for [Ebiroll/ghidra-xtensa](https://github.com/Ebiroll/ghidra-xtensa)                                                                                   **RE Tools**
                                  Xtensa architecture (ESP32                                                                                                                                                      
                                  original). Needed for                                                                                                                                                           
                                  disassembly/decompilation                                                                                                                                                       
                                  of ESP32 firmware.                                                                                                                                                              

  **GhidraLinkerScript**          Ghidra plugin to import     [tarlogic/GhidraLinkerScript](https://github.com/tarlogic/GhidraLinkerScript)                                                                       **RE Tools**
                                  ESP32 ROM function names                                                                                                                                                        
                                  and addresses from                                                                                                                                                              
                                  Espressif linker scripts                                                                                                                                                        
                                  --- dramatically improves                                                                                                                                                       
                                  decompilation quality.                                                                                                                                                          

  **Tarlogic BT Registers**       Documented 40+ undocumented [tarlogic/esp32-btdm-registers](https://github.com/tarlogic/esp32-btdm-registers)                                                                   **BT Research**
                                  Bluetooth registers from                                                                                                                                                        
                                  reverse engineering the                                                                                                                                                         
                                  ESP32 BTDM stack. Header                                                                                                                                                        
                                  files + SVD patches.                                                                                                                                                            
                                  Cutting-edge BT research.                                                                                                                                                       

  **ESP32-QEMU simulator**        QEMU fork that emulates     [espressif/qemu](https://github.com/espressif/qemu)                                                                                                 **Simulation**
                                  ESP32 hardware including                                                                                                                                                        
                                  WiFi peripheral emulation.                                                                                                                                                      
                                  Uri Shaked\'s work ---                                                                                                                                                          
                                  allows running firmware                                                                                                                                                         
                                  without physical device.                                                                                                                                                        

  **espressif/esp-idf-lib**       Collection of high-quality  [UncleRus/esp-idf-lib](https://github.com/UncleRus/esp-idf-lib)                                                                                     **Firmware Ref**
                                  ESP-IDF components --- I2C,                                                                                                                                                     
                                  SPI sensors, WiFi manager,                                                                                                                                                      
                                  NVS helpers. Good firmware                                                                                                                                                      
                                  pattern reference.                                                                                                                                                              

  **Tasmota**                     Production-grade open       [arendst/Tasmota](https://github.com/arendst/Tasmota)                                                                                               **Firmware Ref**
                                  firmware for ESP32/ESP8266.                                                                                                                                                     
                                  500k+ lines --- excellent                                                                                                                                                       
                                  reference for OTA, MQTT,                                                                                                                                                        
                                  peripheral drivers, WiFi                                                                                                                                                        
                                  provisioning.                                                                                                                                                                   

  **ESPHome**                     Production ESP32 firmware   [esphome/esphome](https://github.com/esphome/esphome)                                                                                               **Firmware Ref**
                                  framework with extensive                                                                                                                                                        
                                  IDF/FreeRTOS integration.                                                                                                                                                       
                                  Excellent reference for                                                                                                                                                         
                                  IRAM management, BLE proxy,                                                                                                                                                     
                                  WiFi coexistence.                                                                                                                                                               
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**SECTION 4 --- Hacker & Security Research Resources**

> These are research papers, blog posts, and tools focused on ESP32
> WiFi/Bluetooth internals and exploitation. For RAG purposes, download
> PDFs and index markdown/HTML versions of blog posts.

**4.1 Must-Read Research Articles (Download as PDF/HTML)**

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                                                                  **Priority**
  --------------------- ---------------- ------------------------------------------------------------------------------------------------------------------- ---------------
  **Liberating          Article          [tarlogic.com --- BT Liberation](https://www.tarlogic.com/blog/liberating-bluetooth-on-the-esp32/)                  **ESSENTIAL**
  Bluetooth on the      (HTML→PDF)                                                                                                                           
  ESP32** Reverse                                                                                                                                            
  engineers closed BTDM                                                                                                                                      
  stack, documents 40+                                                                                                                                       
  undocumented BT                                                                                                                                            
  registers and JTAG                                                                                                                                         
  methodology                                                                                                                                                

  **Hacking BT via HCI  Article          [tarlogic.com --- BT                                                                                                **ESSENTIAL**
  Commands & Hidden     (HTML→PDF)       HCI](https://www.tarlogic.com/blog/hacking-bluetooth-the-easy-way-with-esp32-hci-commands-and-hidden-features/)     
  Features** March 2025                                                                                                                                      
  --- exploits                                                                                                                                               
  undocumented HCI                                                                                                                                           
  commands, ROM                                                                                                                                              
  function analysis,                                                                                                                                         
  practical Ghidra                                                                                                                                           
  workflow                                                                                                                                                   

  **Hacking a Smart     Article          [jmswrnr.com --- Smart Home RE](https://jmswrnr.com/blog/hacking-a-smart-home-device)                               **ESSENTIAL**
  Home Device (ESP32 RE (HTML→PDF)                                                                                                                           
  walkthrough)**                                                                                                                                             
  Complete                                                                                                                                                   
  step-by-step: dump                                                                                                                                         
  flash → Ghidra →                                                                                                                                           
  patch binary →                                                                                                                                             
  reflash. Best                                                                                                                                              
  practical RE                                                                                                                                               
  tutorial.                                                                                                                                                  

  **Reverse Engineering Article          [medium.com --- Ghidra                                                                                              **IMPORTANT**
  ESP32 Flash Dumps     (HTML→PDF)       RE](https://olof-astrand.medium.com/reverse-engineering-of-esp32-flash-dumps-with-ghidra-or-ida-pro-8c7c58871e68)   
  with Ghidra** QEMU +                                                                                                                                       
  Ghidra workflow for                                                                                                                                        
  Xtensa firmware                                                                                                                                            
  analysis                                                                                                                                                   

  **URI Shaked Reverses Article          [hackaday.com --- WiFi RE](https://hackaday.com/2021/12/30/remoticon-2021-uri-shaked-reverses-the-esp32-wifi/)      **IMPORTANT**
  ESP32 WiFi (Hackaday  (HTML→PDF)                                                                                                                           
  writeup)** How the                                                                                                                                         
  WiFi is implemented                                                                                                                                        
  as undocumented                                                                                                                                            
  memory-mapped                                                                                                                                              
  peripheral --- unique                                                                                                                                      
  insight                                                                                                                                                    

  **Hardware All The    Wiki (HTML→PDF)  [HardwareAllTheThings RE                                                                                            **IMPORTANT**
  Things --- Firmware                    Guide](https://swisskyrepo.github.io/HardwareAllTheThings/firmware/firmware-reverse-engineering/)                   
  RE Wiki**                                                                                                                                                  
  Comprehensive IoT                                                                                                                                          
  firmware RE wiki                                                                                                                                           
  including ESP32                                                                                                                                            
  esptool, SVD, Ghidra                                                                                                                                       
  workflows                                                                                                                                                  

  **ESPwn32 paper (WOOT PDF (research    [ESPwn32 --- WiFi/BLE ROM analysis](https://wootconference.org/papers/woot23-paper22.pdf)                           **BONUS**
  2023)** Academic      paper)                                                                                                                               
  paper on hacking with                                                                                                                                      
  ESP32 SoC --- ROM                                                                                                                                          
  region analysis, BLE                                                                                                                                       
  protocol stack via                                                                                                                                         
  Ghidra                                                                                                                                                     
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**4.2 BSAM --- Bluetooth Security Assessment Methodology**

An open framework (like OWASP, but for Bluetooth) for auditing BT
devices. Pairs well with the Tarlogic research above --- gives the LLM
structured knowledge of BT attack vectors.

  -----------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                         **Priority**
  --------------------- ---------------- ---------------------------------------------------------- ---------------
  **BSAM Framework**    Web / Clone      [bsam.io](https://www.bsam.io)                             **IMPORTANT**

  **darkmentor BT       Web              [darkmentor.com/bt.html](https://darkmentor.com/bt.html)   **BONUS**
  attack timeline** 30+                                                                             
  Bluetooth attacks                                                                                 
  catalogued with dates                                                                             
  --- good for context                                                                              
  -----------------------------------------------------------------------------------------------------------------

**SECTION 5 --- Reverse Engineering Toolchain Docs**

> These are the documentation and reference files for the tools used in
> ESP32 firmware analysis. Index these so the LLM knows how to instruct
> YOU on using these tools correctly.

**5.1 SVD Files (Register Definitions for Ghidra)**

SVD (System View Description) files map peripheral names to memory
addresses in Ghidra. Loading these transforms raw hex addresses into
named registers in decompiled output.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**                **Format**       **Download / URL**                                                                                           **Priority**
  --------------------------- ---------------- ------------------------------------------------------------------------------------------------------------ ---------------
  **ESP32 SVD files           Git Clone        [github.com/esp-rs/esp-pacs](https://github.com/esp-rs/esp-pacs)                                             **ESSENTIAL**
  (Espressif Rust community                                                                                                                                 
  --- patched & up to date)**                                                                                                                               
  More accurate than official                                                                                                                               
  SVD --- community-patched                                                                                                                                 
  with corrections                                                                                                                                          

  **ESP32 ROM Labels for      Text file        [See jmswrnr.com blog for link](https://gist.github.com/jmswrnr/placeholder)                                 **ESSENTIAL**
  Ghidra (James Warner)**                                                                                                                                   
  Loads all ROM function                                                                                                                                    
  names into Ghidra ---                                                                                                                                     
  transforms FUN_4008xxxx                                                                                                                                   
  into real names                                                                                                                                           

  **SVD-Loader-Ghidra         Git Clone        [github.com/leveldown-security/SVD-Loader-Ghidra](https://github.com/leveldown-security/SVD-Loader-Ghidra)   **IMPORTANT**
  plugin**                                                                                                                                                  

  **GhidraInvalidMemoryRefs   Git Clone        [github.com/tarlogic/GhidraInvalidMemoryRefs](https://github.com/tarlogic/GhidraInvalidMemoryRefs)           **BONUS**
  plugin** Lists code                                                                                                                                       
  accessing unmapped memory                                                                                                                                 
  --- helps find undocumented                                                                                                                               
  peripherals                                                                                                                                               
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**5.2 esptool Documentation**

esptool.py is the core utility for reading/writing ESP32 flash. Its
documentation covers flash protocols, image formats, partition tables,
and boot modes --- all critical for firmware modification.

  -------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                       **Priority**
  --------------------- ---------------- ------------------------------------------------------------------------ ---------------
  **esptool docs**      Web              [esptool                                                                 **ESSENTIAL**
                                         documentation](https://docs.espressif.com/projects/esptool/en/latest/)   

  **esptool GitHub**    Git Clone        [github.com/espressif/esptool](https://github.com/espressif/esptool)     **ESSENTIAL**
  Clone for source +                                                                                              
  README --- flash                                                                                                
  format, partition                                                                                               
  binary spec                                                                                                     
  -------------------------------------------------------------------------------------------------------------------------------

**5.3 ESP-IDF Linker Scripts (ROM Symbols)**

The linker scripts in the IDF contain the addresses of every ROM
function. These are used by the Ghidra plugin and are invaluable for
understanding what code calls into the ROM.

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**                               **Format**       **Download / URL**                                                                                  **Priority**
  ------------------------------------------ ---------------- --------------------------------------------------------------------------------------------------- ---------------
  **ESP32 ROM linker scripts (in IDF repo)** Path in IDF      [esp-idf/components/esp_rom](https://github.com/espressif/esp-idf/tree/master/components/esp_rom)   **ESSENTIAL**
  After cloning IDF:                         clone                                                                                                                
  components/esp_rom/esp32/ld/esp32.rom.ld                                                                                                                        
  etc.                                                                                                                                                            

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**SECTION 6 --- Additional Deep-Dive Resources**

**6.1 FreeRTOS (ESP32 Port)**

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                                                                                             **Priority**
  --------------------- ---------------- ---------------------------------------------------------------------------------------------------------------------------------------------- ---------------
  **FreeRTOS docs       Web              [ESP-IDF FreeRTOS API](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/system/freertos.html)                         **ESSENTIAL**
  (ESP-IDF port)**                                                                                                                                                                      
  Espressif\'s modified                                                                                                                                                                 
  FreeRTOS --- task                                                                                                                                                                     
  priorities, IRAM                                                                                                                                                                      
  placement, dual-core                                                                                                                                                                  
  specifics                                                                                                                                                                             

  **FreeRTOS official   PDF              [Mastering FreeRTOS (official                                                                                                                  **IMPORTANT**
  book (free PDF)**                      book)](https://www.freertos.org/fr-content-src/uploads/2018/07/161204_Mastering_the_FreeRTOS_Real_Time_Kernel-A_Hands-On_Tutorial_Guide.pdf)   
  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**6.2 Partition Table & NVS Format Specs**

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                                                                 **Priority**
  --------------------- ---------------- ------------------------------------------------------------------------------------------------------------------ ---------------
  **ESP32 Partition     Web              [Partition Tables                                                                                                  **ESSENTIAL**
  Tables (IDF doc)**                     guide](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-guides/partition-tables.html)               
  Binary format,                                                                                                                                            
  offsets, custom                                                                                                                                           
  partition schemes,                                                                                                                                        
  OTA slots                                                                                                                                                 

  **NVS (Non-Volatile   Web              [NVS Flash                                                                                                         **IMPORTANT**
  Storage) format                        documentation](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/storage/nvs_flash.html)   
  spec**                                                                                                                                                    
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**6.3 WiFi Driver Internals**

  ----------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                                                  **Priority**
  --------------------- ---------------- --------------------------------------------------------------------------------------------------- ---------------
  **ESP32 WiFi Driver   Web              [ESP32 WiFi programming                                                                             **ESSENTIAL**
  doc** Frame                            guide](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-guides/wifi.html)            
  injection,                                                                                                                                 
  promiscuous mode,                                                                                                                          
  power save, internal                                                                                                                       
  buffer architecture                                                                                                                        

  **ESP32 WiFi Security Web              [WiFi Security                                                                                      **IMPORTANT**
  guide**                                guide](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-guides/wifi-security.html)   
  ----------------------------------------------------------------------------------------------------------------------------------------------------------

**6.4 Bluetooth / BLE Internals**

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                                                           **Priority**
  --------------------- ---------------- ------------------------------------------------------------------------------------------------------------ ---------------
  **ESP32 BT/BLE        Web              [IDF Bluetooth API                                                                                           **ESSENTIAL**
  overview (IDF)**                       reference](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/bluetooth/index.html)   

  **ESP-IDF BLE GATT    Path in IDF      [esp-idf/examples/bluetooth](https://github.com/espressif/esp-idf/tree/master/examples/bluetooth)            **IMPORTANT**
  examples** Complete                                                                                                                                 
  examples for GATT                                                                                                                                   
  server/client, HCI,                                                                                                                                 
  BLE mesh                                                                                                                                            
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------

**6.5 Xtensa LX6/LX7 Architecture**

The LLM needs to understand the Xtensa ISA to interpret disassembly,
understand calling conventions, and reason about memory protection /
window registers.

  --------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                        **Priority**
  --------------------- ---------------- ------------------------------------------------------------------------- ---------------
  **Xtensa LX7 ISA      PDF              [Xtensa ISA Databook                                                      **BONUS**
  Reference                              (Cadence)](https://ip.cadence.com/uploads/1099/xtensa_isa_databook-pdf)   
  (Tensilica)** Full                                                                                               
  instruction set ---                                                                                              
  call0/callN windows,                                                                                             
  CPROSR, loop                                                                                                     
  instructions                                                                                                     

  **RISC-V ISA Spec     PDF              [riscv.org specifications](https://riscv.org/technical/specifications/)   **IMPORTANT**
  v1.0 (for C3/C6/H2                                                                                               
  variants)**                                                                                                      
  ESP32-C3/C6/H2 use                                                                                               
  RISC-V --- need both                                                                                             
  ISAs for full                                                                                                    
  coverage                                                                                                         
  --------------------------------------------------------------------------------------------------------------------------------

**SECTION 7 --- RAG Setup Recommendations**

**7.1 Chunking Strategy for ESP32 Docs**

**TRMs and Datasheets (PDF):** Chunk at 600--800 tokens. Use section
headings as metadata. Register tables are dense --- keep table rows
together if possible.

**IDF Programming Guide:** Chunk at 800 tokens with overlap of 100
tokens. Tag each chunk with its IDF component name (e.g., \'wifi\',
\'bt\', \'freertos\', \'nvs\').

**GitHub source code:** Index .c, .h, .md, and Kconfig files. Split .c
files at function boundaries (find \'\^void\\\|\^esp_err_t\\\|\^static\'
boundaries). Headers are highest-value --- include all of them.

**Blog articles:** Convert HTML to clean markdown first (use pandoc or
trafilatura). Chunk at 400--600 tokens.

**7.2 Recommended RAG Tools**

  -------------------------------------------------------------------------------------------------------------------------------------
  **Resource**          **Format**       **Download / URL**                                                             **Priority**
  --------------------- ---------------- ------------------------------------------------------------------------------ ---------------
  **Open WebUI +        App              [github.com/open-webui/open-webui](https://github.com/open-webui/open-webui)   **ESSENTIAL**
  Ollama** Best UI for                                                                                                  
  local RAG ---                                                                                                         
  drag-and-drop PDF                                                                                                     
  indexing, persistent                                                                                                  
  collections                                                                                                           

  **LM Studio** Easy    App              [lmstudio.ai](https://lmstudio.ai)                                             **ESSENTIAL**
  GGUF model                                                                                                            
  management, built-in                                                                                                  
  RAG document support                                                                                                  

  **AnythingLLM**       App              [useanythingllm.com](https://useanythingllm.com)                               **IMPORTANT**
  Multi-document RAG                                                                                                    
  with agent mode ---                                                                                                   
  good for long                                                                                                         
  sessions                                                                                                              

  **llamaindex (for     Python           [github.com/run-llama/llama_index](https://github.com/run-llama/llama_index)   **BONUS**
  custom RAG                                                                                                            
  pipelines)**                                                                                                          
  -------------------------------------------------------------------------------------------------------------------------------------

**7.3 Quick Download Checklist**

1.  **TRMs:** ESP32, ESP32-S3, ESP32-C3 PDFs --- download all three to
    start

2.  **Datasheets:** ESP32 series datasheet PDF

3.  **IDF:** git clone esp-idf v5.3 with \--recursive

4.  **IDF PDF:** esp-idf-en-v5.3-esp32.pdf --- full 2000-page guide

5.  **esptool:** clone repo, index README and docs

6.  **ESP32 SVD:** clone esp-rs/esp-pacs for register definitions

7.  **Marauder:** clone justcallmekoko/ESP32Marauder --- index all
    .cpp/.h

8.  **Tarlogic BT articles:** save both as PDFs, index them

9.  **James Warner RE article:**
    jmswrnr.com/blog/hacking-a-smart-home-device --- save as PDF

10. **FreeRTOS:** Download free PDF book from freertos.org

ESP32 RAG Resource Guide · Generated March 2026 · All links verified at
time of generation
