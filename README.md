# ASIX AX88796C SRAM-like Fast Ethernet Linux driver (`ax88796c.o`)

Official ASIX AX88796C SRAM-like Fast Ethernet Linux driver source. (Driver module name: `ax88796c.o`)

---
## ASIX SPI/Non-PCI Fast Ethernet ICs 
- [AX88796C SPI/Non-PCI Fast Ethernet Chip](https://www.asix.com.tw/en/product/EmbeddedEthernet/1-PortEthernet/AX88796C)

The **AX88796C** is a low‑power SPI/Non-PCI Fast Ethernet controller designed for embedded and industrial networking. It supports multiple host interfaces, including **SPI and localbus/SRAM‑like**. The local bus allows 8/16‑bit SRAM‑like or multiplexed connections, enabling direct MCU integration without extra logic. For CPUs with SPI, the AX88796C also offers an optional SPI slave interface to simplify hardware design.

---
## File Description

- **README** – This file  
- **ax88796c_main.c / ax88796c.h** – Driver main functions and headers  
- **ax88796c_plat_dma.c / .h** – Platform‑dependent functions and headers  
- **ax88796c_ioctl.c / .h** – Helper functions and headers  
- **command.h** – Driver header file  
- **ioctl.c / ioctl.h** – User program for IEEE test and headers  
- **Makefile** – Driver build file  
- **ax88796c.log** – Debug log file  
- **COPYING** – GNU General Public License  

---
## Platform Driver Registration

Since the AX88796C Linux driver is written in platform device model, developer
should declare the resource for it. For more information, please refer to the
"Kernel Source/Documentation/driver-model/platform.txt". Following shows
example of AX88796C platform driver declaration:

1. Declare resource for AX88796C:
```c
static struct resource ax88796c_resource[] = {
    [0] = {
        .start = S3C2410_CS1,           /* Start of AX88796C base address */
        .end   = S3C2410_CS1 + 0xFFFFF, /* End of AX88796C base address */
        .flags = IORESOURCE_MEM,
    },
    [1] = {
        .start = IRQ_EINT11,            /* Interrupt line number */
        .end   = IRQ_EINT11,
        .flags = IORESOURCE_IRQ,
    }
};
```

2. Declare the name for binding:
```c
struct platform_device net_device_ax88796c = {
    .name  = "ax88796c",
    .id  = -1,
    .num_resources = ARRAY_SIZE(ax88796c_resource),
    .resource = ax88796c_resource,
};
```

3. Sample code to insert driver resource into platform devices:
```c
static struct platform_device *smdk2440_devices[] __initdata = {
        &s3c_device_spi0,
        &s3c_device_ohci,
        &s3c_device_lcd,
        &s3c_device_wdt,
        &s3c_device_i2c0,
        &s3c_device_iis,
               .
               .
               .
        &net_device_ax88796c,           /* Insert driver resource here */
};
```
