# ESParklines
Nice sparklines for ESP8266, ESP32, Arduino

> [Sparklines](https://en.wikipedia.org/wiki/Sparkline) are small, high resolution
> graphics embedded in a context of words, numbers or images". Edward Tufte
> describes sparklines as "data-intense, design-simple, word-sized graphics".

Read more about Sparklines [here](https://www.edwardtufte.com/bboard/q-and-a-fetch-msg?msg_id=0001OR). 

### Description 
This library is very simple, it
- holds a buffer of any numeric data type of speficied size
- renders sparklines using a passed-in function to draw lines

### Showcase
Click picture to watch video (35sec) on YouTube|Example App: Geiger Counter
------------ | -------------
[![DemoVideo]](http://www.youtube.com/watch?v=Pvfijfrt5HI) | ![AppExample]

### Example Usage
Create Sparkline with a buffer for `25` items of `uint16_t` and pass in u8g2's `drawLine`:
```cpp
SparkLine<uint16_t> MySparkLine(25, [&](const uint16_t x0, const uint16_t y0, const uint16_t x1, const uint16_t y1) { 
  u8g2.drawLine(x0, y0, x1, y1);
});
```

Add data regularly, e.g. upon sensor read, or `analogRead()`. The data contents will be automatically rotated (FIFO), once the buffer exceeds the specified number of items.
```cpp
MySparkLine.add(42);
```

Then, periodically display sparkline at 0x10, 50 pixel wide, 20 pixel high, e.g. from `loop()` like so:
```cpp
static esp8266::polledTimeout::periodicMs periodicSecond(1000);

if (periodicSecond) {
  MySparkLine.draw(0, 10, 50, 20);
}
```

### Dependency Information
This library depends on [FixedPointsArduino](https://github.com/Pharap/FixedPointsArduino). It will be automatically installed via PlatformIO.

To remove this dependency, redefine `num_t` to `float` or `double` and remove all occurences of `.getInteger()` in `SparkLine.h`.

### License
```
MIT License

Copyright (c) 2020 karl@pitrich.com

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

[DemoVideo]:http://img.youtube.com/vi/Pvfijfrt5HI/0.jpg
[AppExample]:/doc/Sparklines%20Example%20GeigerCounter.jpg
