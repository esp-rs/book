# Simulating

Simulating projects can be handy. It allows users to test projects using CI, try projects without having hardware available, and many other scenarios.

At the moment, there are a few ways of simulating Rust projects on Espressif chips. Every way has some limitations, but it's quickly evolving and getting better every day.

In this chapter, we will discuss currently available simulation tools.

Refer to the table below to see which chip is supported in every simulating method:

|              | **[Wokwi][wokwi]** | **QEMU** |
| :----------: | :----------------: | :------: |
|  **ESP32**   |         ✅          |    ✅     |
| **ESP32-C2** |         ❌          |    ❌     |
| **ESP32-C3** |         ✅          |    ❌     |
| **ESP32-C6** |         ✅          |    ❌     |
| **ESP32-H2** |         ✅          |    ❌     |
| **ESP32-S2** |         ✅          |    ❌     |
| **ESP32-S3** |         ✅          |    ❌     |

[wokwi]: https://docs.wokwi.com/guides/esp32#simulation-features
