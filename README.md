AS_BH1750_SoftI2C
=================

This is an Arduino library for the BH1750FVI Digital Light Sensor
(http://s6z.de/joomla3/index.php/arduino/sensoren/15-umgebungslichtsensor-bh1750)
and is a fork of https://github.com/hexenmeister/AS_BH1750.

This fork supports controlling the light sensor using software I2C with user selectable pins for SDA and SCL.

Required library:
 * todbot's version of [SoftI2CMaster](https://github.com/todbot/SoftI2CMaster)

Differences to original version:

 - Class name: ``AS_BH1750_SoftI2C`` instead of ``AS_BH1750``

 - Includes:
   ```
   #include "SoftI2CMaster.h"
   #include <AS_BH1750_SoftI2C.h>
   ```

   instead of
   ```
   #include <Wire.h>
   #include <AS_BH1750.h>
   ```


 - The pins can be set when calling the constructor (default: SDA = 7, SCL = 6):
   ```
   AS_BH1750_SoftI2C sensor([<i2c address>], [<pin for SDA>], [<pin for SCL>]);
   ```
   e.g.:
   ```
   AS_BH1750_SoftI2C sensor(BH1750_DEFAULT_I2CADDR, A3, A2);
   ```




Unchanged description of original version (in german):



Features:

 - unterstützt beide möglichen I2C-Adressen des Sensors:
   Standardadresse: 0x23, Alternativadresse: 0x5C. 
   Es sind entsprechende Konstanten definiert: BH1750_DEFAULT_I2CADDR und BH1750_SECOND_I2CADDR.
 
 - alle Hardware-Auflösungsmodi:
   * RESOLUTION_LOW:         Physische Sensormodus mit 4 lx Auflösung. Messzeit ca. 16ms. Bereich 0-54612. 
   * RESOLUTION_NORMAL:      Physische Sensormodus mit 1 lx Auflösung. Messzeit ca. 120ms. Bereich 0-54612.
   * RESOLUTION_HIGH:        Physische Sensormodus mit 0,5 lx Auflösung. Messzeit ca. 120ms. Bereich 0-54612.

 - Virtueller Modus:
   * RESOLUTION_AUTO_HIGH:   Die Werte im MTreg ('Measurement Time'-Register) werden je nach Helligkeit 
                              automatisch so angepasst, dass eine maximalmögliche Auflösung 
                              und Messbereich erziehlt werden.
                              Die messbaren Werte fangen von 0,11 lx und gehen bis über 100000 lx.
                              (ich weis nicht, wie genau die Werte in Grenzbereichen sind, 
                              besonders bei hohen Werte habe ich da meine Zweifel. 
                              Die Werte scheinen jedoch weitgehend linear mit der steigenden Helligkeit zu wachsen.)
                              Auflösung im Unteren Bereich ca. 0,13 lx, im mittleren 0,5 lx, im oberen etwa 1-2 lx.
                              Die Messzeiten verlängern sich durch mehrfache Messungen und 
                              die Änderungen von Measurement Time (MTreg) bis max. ca. 500 ms.
 
 - Methode zur Änderung von 'Measurement Time'-Registers. Damit kann die Empfindlichkeit beeinflusst werden.
 
 - auto power down: Der Sensor wird nach der Messung in den Stromsparmodus versetzt. 
   Das spätere Aufwecken wird ggf. automatisch vorgenommen, braucht jedoch geringfügig mehr Zeit.

 Defaultwerte: Modus = RESOLUTION_AUTO_HIGH, AutoPowerDown = true
