<div align="center">

# هسته آردوینو برای میکروکنترلرهای WCH CH32

**با استفاده از Arduino IDE، ‏Arduino CLI و API آشنای آردوینو، میکروکنترلرهای RISC-V خانواده CH32 شرکت WCH را برنامه‌نویسی کنید.**

[![Release](https://img.shields.io/github/v/tag/openwch/arduino_core_ch32?label=release&sort=semver)](https://github.com/openwch/arduino_core_ch32/tags)
[![Open issues](https://img.shields.io/github/issues/openwch/arduino_core_ch32)](https://github.com/openwch/arduino_core_ch32/issues)
[![Arduino IDE 2](https://img.shields.io/badge/Arduino%20IDE-2.x-00979D?logo=arduino&logoColor=white)](https://www.arduino.cc/en/software)
[![Toolchain](https://img.shields.io/badge/toolchain-WCH%20GCC%2015.2-blue)](#زنجیره-ابزار)

[نصب](#نصب) · [تراشه‌های پشتیبانی‌شده](#تراشهها-و-بردهای-پشتیبانیشده) · [امکانات](#امکانات) · [شروع به کار](#شروع-به-کار) · [آپلود](#آپلود-برنامه) · [تاریخچه تغییرات](CHANGELOG.md) · [مشارکت](CONTRIBUTING.md)

</div>

---

## چرا از این هسته استفاده کنیم؟

- **یک هسته برای خانواده CH32V و تراشه‌های مرتبط:** شامل V003، خانواده V002/V004/V005/V006/V007، مدل X035، سری‌های V103، V203، V307 و L103؛ با ظرفیت حافظه Flash از ۱۶ تا ۲۵۶ کیلوبایت و امکان استفاده از کد یکسان در پروژه‌ها.
- **رابط برنامه‌نویسی استاندارد آردوینو:** توابعی مانند `digitalWrite`، ‏`analogRead`، ‏`analogWrite`، ‏`attachInterrupt`، ‏`Serial`، ‏`Wire`، ‏`SPI`، ‏`tone` و `HardwareTimer`، به‌همراه درایورهای جانبی WCH برای دسترسی به امکانات پیشرفته‌تر.
- **زنجیره ابزار اختصاصی WCH:** کامپایلر GCC نسخه 15.2 با افزونه‌های اختصاصی WCH (از جمله دستورهای فشرده `xw` و عملیات سخت‌افزاری ذخیره/بازیابی پشته) و OpenOCD نسخه 2.10 برای پروگرامر WCH-LinkE؛ هر دو از طریق Boards Manager نصب می‌شوند.
- **گزینه‌های قابل تنظیم:** منبع و فرکانس کلاک، سطح بهینه‌سازی (از جمله LTO برای مدل V003 با حافظه Flash برابر ۱۶ کیلوبایت)، کتابخانه C، پشتیبانی از printf/scanf اعشاری، پشته USB و روش پروگرام.
- **آزمایش روی سخت‌افزار واقعی:** پیش از انتشار در Boards Manager، نسخه‌ها روی بردهای EVT هر خانواده از نظر GPIO، وقفه‌ها، ADC، PWM، تایمرها، I2C، SPI و Serial بررسی می‌شوند.

## نصب

### نصب در Arduino IDE 2 از طریق Boards Manager

1. از مسیر **File → Preferences → Additional boards manager URLs** نشانی زیر را اضافه کنید:

   ```
   https://github.com/openwch/board_manager_files/raw/main/package_ch32v_index.json
   ```

2. از مسیر **Tools → Board → Boards Manager** عبارت **CH32** را جست‌وجو کرده و بسته **CH32 MCU EVT Boards** را نصب کنید.  
   با نصب این بسته، زنجیره ابزار شامل WCH GCC 15.2 و OpenOCD 2.10 نیز دریافت می‌شود.

3. از مسیر **Tools → Board → CH32 MCU EVT Boards** خانواده تراشه را انتخاب کنید؛ سپس مدل دقیق تراشه را از بخش **Tools → Board Select** مشخص کنید.

### نصب با Arduino CLI

دستورهای زیر را در ترمینال اجرا کنید:

```bash
arduino-cli config add board_manager.additional_urls https://github.com/openwch/board_manager_files/raw/main/package_ch32v_index.json
arduino-cli core update-index
arduino-cli core install WCH:ch32v
arduino-cli compile -b WCH:ch32v:CH32V20x_EVT:pnum=CH32V203C8 MySketch
```

در دستور آخر، نمونه برنامه با نام `MySketch` برای تراشه CH32V203C8 کامپایل می‌شود.

### استفاده از نسخه توسعه (Git)

برای استفاده از آخرین تغییرات شاخه `main`، مخزن را در مسیر سخت‌افزارهای Sketchbook آردوینو کلون کنید:

```bash
mkdir -p ~/Documents/Arduino/hardware/WCH
cd ~/Documents/Arduino/hardware/WCH
git clone --recursive https://github.com/openwch/arduino_core_ch32.git ch32v
```

نسخه کلون‌شده از همان زنجیره ابزار نسخه انتشار استفاده می‌کند. برای دریافت ابزارها می‌توانید یک‌بار نسخه انتشار را از طریق Boards Manager نصب کنید؛ یا مسیر نصب ابزارهای خود را در فایل `platform.local.txt`، در کنار `platform.txt`، با گزینه‌های `wch.gcc.dir` و `wch.openocd.dir` مشخص کنید.

### سیستم‌عامل‌های پشتیبانی‌شده

| سیستم‌عامل | وضعیت | توضیحات |
|---|---|---|
| Windows 10/11 نسخه 64 بیتی | ✅ | برای نصب درایور WCH-LinkE، یک‌بار [WCH-LinkUtility](https://www.wch-ic.com/downloads/WCH-LinkUtility_ZIP.html) را نصب کنید. |
| Linux نسخه x86_64 | ✅ | پس از نصب، اسکریپت زیر را یک‌بار اجرا کنید تا WCH-LinkE و سایر ابزارهای دیباگ بدون دسترسی sudo قابل استفاده باشند: `~/.arduino15/packages/WCH/tools/beforeinstall/2.0.0/start.sh` |
| macOS | ⚠️ | ابزارهای نسخه 1.0.5 و جدیدتر هنوز برای macOS منتشر نشده‌اند؛ بنابراین در این سیستم‌عامل نسخه 1.0.4 از طریق Boards Manager ارائه می‌شود. |

**نکته برای macOS و نسخه 1.0.4:** پیش از اولین بارگذاری برنامه، کتابخانه `libusb` را نصب کنید:

```bash
brew install libusb
```

اگر پس از نصب `libusb` همچنان هنگام آپلود با خطاهای مرتبط با آن مواجه شدید، با تیم MounRiver Studio از طریق نشانی support@mounriver.com تماس بگیرید؛ زنجیره ابزار و OpenOCD توسط این تیم ساخته می‌شوند.

## تراشه‌ها و بردهای پشتیبانی‌شده

| خانواده (Tools → Board) | تراشه‌ها / بردها (Board Select) | هسته پردازنده | حداکثر فرکانس | Flash / RAM | از نسخه |
|---|---|---|---|---|---|
| **CH32V00x** | CH32V003F4 EVT | QingKe V2A | 48 MHz | 16 KB / 2 KB | 1.0.0 |
| **CH32VM00X** | CH32V006K8 EVT | QingKe V2C | 48 MHz | 62 KB / 8 KB | 1.0.5 |
| **CH32X035** | CH32X035G8U EVT | QingKe V4C | 48 MHz | 62 KB / 20 KB | 1.0.1 |
| **CH32V10x** | CH32V103R8T6 EVT | QingKe V3A | 72 MHz | 64 KB / 20 KB | 1.0.3 |
| **CH32V20x** | CH32V203C6، C8، G6، G8، RB EVT، ‏Adafruit QT Py CH32V203 | QingKe V4B | 144 MHz | حداکثر 128 KB / 64 KB | 1.0.0 |
| **CH32V30x** | CH32V307VCT6 EVT | QingKe V4F (دارای FPU) | 144 MHz | 256 KB / 64 KB | 1.0.3 |
| **CH32L10x** | CH32L103C8T6 EVT | QingKe V4C | 96 MHz | 64 KB / 20 KB | 1.0.5 |

پشتیبانی از پکیج‌های بیشتر خانواده V002/V004/V005/V006/V007 و همچنین CH32X033 در دست توسعه است ([#200](https://github.com/openwch/arduino_core_ch32/pull/200)، [#171](https://github.com/openwch/arduino_core_ch32/pull/171)).  
بردهایی که در جدول بالا نام برده نشده‌اند، معمولاً می‌توانند از Variant مربوط به همان تراشه استفاده کنند. برای اطلاعات بیشتر، راهنمای [افزودن برد](CONTRIBUTING.md#adding-a-chip-or-board) را ببینید.

## امکانات

| قابلیت | V00x | VM00X | X035 | V10x | V20x | V30x | L10x |
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| GPIO، مقاومت Pull-up/Pull-down و خروجی Open-drain | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `attachInterrupt` (وقفه EXTI) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `analogRead` (مبدل ADC) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `analogWrite` (خروجی PWM) و `tone` | ✅ | ✅ | ✅ ¹ | ✅ | ✅ | ✅ | ✅ |
| `HardwareTimer` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `Serial` (واحدهای U(S)ART) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ ² |
| `Wire` (ارتباط I2C در حالت Master) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✓ |
| `SPI` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✓ |
| بهینه‌سازی LTO (از مسیر Tools → Optimize) | ✅ | ✅ | ✅ | ⚠️ ⁴ | ⚠️ ⁴ | ⚠️ ⁴ | ⚠️ ⁴ |
| `EEPROM` (با استفاده از Option Bytes) | ✓ | | | ⚠️ ³ | | | |
| USB Device (بر پایه Adafruit TinyUSB) | | | | | ✓ | ✓ | |
| دریافت‌کننده USB-PD (`USBPD_SINK`) | | | ✓ | | | | |

راهنمای وضعیت‌ها:

- ✅ پشتیبانی روی برد EVT آزمایش شده است.
- ✓ قابلیت در دسترس است، اما هنوز روی برد آزمایش نشده است.
- ⚠️ محدودیت یا مشکل شناخته‌شده وجود دارد.

توضیحات:

1. در خروجی‌های مکمل تایمر (N) هنوز پشتیبانی نمی‌شود؛ جزئیات در [Issue #183](https://github.com/openwch/arduino_core_ch32/issues/183).
2. مشکل گزارش‌شده در [Issue #149](https://github.com/openwch/arduino_core_ch32/issues/149).
3. مشکل گزارش‌شده در [Issue #213](https://github.com/openwch/arduino_core_ch32/issues/213).
4. در این خانواده‌ها، فعال‌سازی LTO باعث حذف Handlerهای وقفه هسته می‌شود ([Issue #204](https://github.com/openwch/arduino_core_ch32/issues/204))؛ بنابراین از گزینه‌های معمول بهینه‌سازی استفاده کنید.

فهرست مشکلات و قابلیت‌های در دست بررسی در بخش [Issues مخزن](https://github.com/openwch/arduino_core_ch32/issues) قرار دارد.

## شروع به کار

نمونه ساده زیر یک پایه را تغییر وضعیت می‌دهد و اطلاعات را از طریق رابط سریال ارسال می‌کند. در برد EVT، پورت COM مربوط به WCH-Link با نرخ 115200 بیت بر ثانیه قابل استفاده است.

```cpp
// تغییر وضعیت یک پایه و ارسال اطلاعات از طریق Serial
#define LED_PIN PA8   // نام پایه‌ها مطابق نام‌گذاری تراشه است: PA8، PD4، PC1 و ...

void setup() {
  Serial.begin(115200);
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_PIN, !digitalRead(LED_PIN));
  Serial.print("millis: ");
  Serial.println(millis());
  delay(500);
}
```

ورودی‌های آنالوگ نیز با نام‌هایی مانند `A0`، ‏`A1` و ... قابل دسترسی هستند. رابط `Serial` پیش‌فرض، واحد USART متصل به WCH-Link روی برد EVT است.

برای استفاده از UARTهای دیگر می‌توانید نمونه‌ای مانند زیر تعریف کنید:

```cpp
HardwareSerial Serial2(USART2);
```

پایه‌های مورد استفاده باید مطابق نقشه پایه‌های Variant انتخاب شوند. برای بردهای V20x و X035، واحد USART2 روی پایه‌های PA2/PA3 قرار دارد؛ در حال حاضر برای سایر Variantها فقط UART مربوط به `Serial` فهرست شده است.

مثال‌های کتابخانه‌ها از مسیر **File → Examples → (Examples for CH32 MCU EVT Boards)** در دسترس هستند؛ از جمله EEPROM، ‏SPI (حافظه Flash)، ‏Wire (اسکنر I2C، تراشه AT24C02 و حالت I2C Slave) و USBPD_SINK.

### استفاده از درایورهای جانبی WCH

درایورهای SDK شرکت WCH با فایل‌های سربرگ `ch32*_*.h` در تمام Sketchها قابل استفاده هستند. بنابراین می‌توانید امکاناتی را که API آردوینو مستقیماً پوشش نمی‌دهد، مانند DMA، ‏CAN، ‏OPA، ‏RTC و DAC، در کنار کد آردوینو به کار بگیرید.

نمونه راه‌اندازی GPIO با استفاده از درایور استاندارد WCH:

```cpp
void setup() {
  RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
  GPIO_InitTypeDef gpio = {0};
  gpio.GPIO_Pin = GPIO_Pin_0;
  gpio.GPIO_Mode = GPIO_Mode_Out_PP;
  gpio.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOA, &gpio);
}
```

## آپلود برنامه

| روش انتخابی در Tools → Upload method | سخت‌افزار موردنیاز |
|---|---|
| **WCH-SWD** (روش پیش‌فرض) | پروگرامر WCH-LinkE در حالت RV یا WCH-Link روی برد EVT؛ از طریق پایه‌های SWDIO/SWCLK یا رابط SWIO (پایه PD1) در خانواده‌های V00x/VM00X |
| **WCH-ISP** | Bootloader داخلی تراشه از طریق USB یا UART، در تراشه‌هایی که این قابلیت را دارند؛ هنگام Reset باید پایه BOOT0 در سطح High قرار گیرد. |

نکات عیب‌یابی:

- **اگر WCH-LinkE در حالت ARM قرار دارد:** با نرم‌افزار WCH-LinkUtility آن را به حالت RV تغییر دهید.
- **اگر تراشه دیگر متصل نمی‌شود:** ممکن است برنامه، پایه‌های دیباگ را به GPIO تغییر داده باشد. در این حالت هنگام پروگرام، BOOT0 را در سطح High نگه دارید یا تراشه را با WCH-LinkUtility پاک کنید.

## زنجیره ابزار

| ابزار | نسخه | روش نصب |
|---|---|---|
| WCH GCC (`riscv32-wch-elf`) | 15.2.0 | از طریق Boards Manager با بسته `riscv-wch-gcc` |
| WCH OpenOCD | 2.10 | از طریق Boards Manager با بسته `openocd-wch` |

نسخه‌های GCC 15.2 و OpenOCD 2.10، نسخه‌های ساخته‌شده توسط [MounRiver Studio](http://www.mounriver.com/) هستند.

در نسخه‌های تا 1.0.4 از GCC 8.2 ([openwch/risc-none-embed-gcc](https://github.com/openwch/risc-none-embed-gcc)) و OpenOCD 1.0.0 ([openwch/openocd_wch](https://github.com/openwch/openocd_wch)) استفاده شده است. این نسخه‌های قدیمی همچنان از طریق Boards Manager قابل نصب هستند.

## ساختار پوشه‌های پروژه

ساختار اصلی مخزن به شکل زیر است:

```
cores/arduino/       پیاده‌سازی API آردوینو (HardwareSerial، HardwareTimer، wiring_*، Tone و ...)
cores/arduino/ch32/  لایه ارتباطی میان API آردوینو و WCH SDK (UART، آنالوگ، وقفه، کلاک و تایمر)
variants/<series>/   پوشه اختصاصی هر تراشه یا برد: نام پایه‌ها، نقشه پایه‌ها و واحدهای جانبی فعال
system/<series>/     فایل‌های WCH SDK: راه‌اندازی اولیه، Linker Scriptها و درایورهای جانبی
libraries/           کتابخانه‌های Wire، SPI، EEPROM، USBPD_SINK و Adafruit TinyUSB
boards.txt           تعریف بردها و گزینه‌های منوی Tools
platform.txt         دستورهای ساخت، زنجیره ابزار و ابزارهای پروگرام
```

## مشارکت در توسعه

گزارش خطا، ارائه اصلاحات و افزودن بردهای جدید مورد استقبال است. فایل [CONTRIBUTING.md](CONTRIBUTING.md) روش گزارش خطا به‌گونه‌ای که قابل بازتولید باشد، نحوه ساخت و آزمایش تغییرات و روش افزودن تراشه یا برد جدید را توضیح می‌دهد.

- گزارش خطا و درخواست قابلیت جدید: [ثبت Issue](https://github.com/openwch/arduino_core_ch32/issues/new/choose)
- یادداشت‌های انتشار: [CHANGELOG.md](CHANGELOG.md)
- فهرست افرادی که در اصلاحات، افزودن بردها و آزمایش‌ها مشارکت داشته‌اند: [Contributors](https://github.com/openwch/arduino_core_ch32/graphs/contributors)

## پروژه‌ها و منابع مرتبط

- [WCH-LinkUtility](https://www.wch-ic.com/downloads/WCH-LinkUtility_ZIP.html): نرم‌افزار پروگرام، پاک‌کردن تراشه و پیکربندی WCH-Link
- [MounRiver Studio](http://www.mounriver.com/): محیط توسعه اختصاصی WCH و منبع زنجیره ابزار
- [وب‌سایت رسمی WCH](https://www.wch-ic.com/): دریافت دیتاشیت‌ها و مستندات فنی تراشه‌ها

## ارتباط و پشتیبانی

- ثبت و پیگیری مشکلات: [github.com/openwch/arduino_core_ch32/issues](https://github.com/openwch/arduino_core_ch32/issues)
- ایمیل: yy@wch.cn

---

![MES Divider](Images/MES-Divider.svg)

## تهیه و تدوین

---

**معمار الکترونیک صنعت**\\
**MEMAR ELECTRONIC SANAT (MES)**

همراه شما در مسیر طراحی و ساخت مدارهای مطمئن

- کانال تلگرام: [https://t.me/Memar_Electronic](https://t.me/Memar_Electronic)
- پیج اینستاگرام: [https://www.instagram.com/Memar.Electronic](https://www.instagram.com/Memar.Electronic)

[مشاهده سایر آموزش‌ها](https://github.com/MEMAR-ELECTRONIC-SANAT)
