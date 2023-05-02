# Lab: NodeRed_MQTT_control_LED_Psoc6

## **Overview**

**Node-red** → Msg[TURN ON/TURN OFF] →Publish → MQTT(QoS0)

**PSoC6** →MQTT(QoS0)→ Subscribe → control [LED ON/OFF]


**Sequence of operation**

1. The user button is pressed. (use the button on node-red)
2. Node-red publishes a message TURN ON/TURN OFF  on a topic.
3. The MQTT broker sends back the message to the MQTT client because it is also subscribed to the same topic.
4. When the message is received, the subscriber task turns the LED ON or OFF. As a result, the user LED toggles every time the user presses the button.



## Requirements
- [ModusToolbox™ software](https://www.infineon.com/modustoolbox) v3.0 or later (tested with v3.0)
- Board support package (BSP) minimum required version: 4.0.0
- Programming language: C
- Associated parts: All [PSoC™ 6 MCU](https://www.infineon.com/PSoC6) parts, [AIROC™ CYW43012 Wi-Fi & Bluetooth® combo chip](https://www.infineon.com/cms/en/product/wireless-connectivity/airoc-wi-fi-plus-bluetooth-combos/cyw43012), [AIROC™ CYW4343W Wi-Fi & Bluetooth® combo chip](https://www.infineon.com/cms/en/product/wireless-connectivity/airoc-wi-fi-plus-bluetooth-combos/cyw4343w/), [AIROC™ CYW4373 Wi-Fi & Bluetooth® combo chip](https://www.infineon.com/cms/en/product/wireless-connectivity/airoc-wi-fi-plus-bluetooth-combos/cyw4373/), [AIROC™ CYW43439 Wi-Fi & Bluetooth® combo chip](https://www.infineon.com/cms/en/product/wireless-connectivity/airoc-wi-fi-plus-bluetooth-combos/cyw43439/)
- Node-red(tested with v.19.12.1 LTS on Windows x64)
- npm(tested with v.9.1.2 on Windows x64)

## **Hardware setup**

This example uses the board's default configuration. See the kit user guide to ensure that the board is configured correctly.

## **Software setup**
1. ใช้ example [code Infineon/mtb-example-wifi-mqtt-client](https://github.com/Infineon/mtb-example-wifi-mqtt-client)
2. ใช้ Mosquitto MQTT broker online link https://test.mosquitto.org/
3. node-red on PC (Windows)
## **Supported toolchains (make variable 'TOOLCHAIN')**
- GNU Arm® embedded compiler v10.3.1 (`GCC_ARM`) - Default value of `TOOLCHAIN`
- Arm® compiler v6.16 (`ARM`)
- IAR C/C++ compiler v9.30.1 (`IAR`)

## **Using the code example**
- สร้างโปรเจคบน MTB ใช้ example [code Infineon/mtb-example-wifi-mqtt-client](https://github.com/Infineon/mtb-example-wifi-mqtt-client)
## **Operation**
1. Connect the board to your PC using the provided USB cable through the KitProg3 USB connector.
2. การตั้งค่าให้ใช้งาน Broker ของ mosquitto >> link https://test.mosquitto.org/ แบบ ไม่ต้องเข้ารหัส (unencrypted) และ ไม่ต้องตรวจสอบตัวตน(unauthenticated) 

![fig: how to setup server port of Mosquitto broker](https://paper-attachments.dropboxusercontent.com/s_DE46169648EC1184505F0FEE30B79C93229F6F5C4674567BCFF38BB678D2F8D7_1681979509506_Untitled.png)


- ไปที่ config >> mqtt_client_config.h
   - ตั้งค่าที่อยู่ broker และ port แบบ Non-secure 
   - ตั้งค่าการเชื่อมต่อ MQTT เป็นแบบ Non-secure 
   - ตั้งชื่อ mqtt Topic สำหรับใช้ในการสื่อสารระหว่าง  publisher และ subscriber 
   - ตั้งค่า QoS เป็น mode 0 ในการสื่อผ่าน MQTT แบบ Non-secure
        
        
 `````  
    // i. MQTT CLIENT CONNECTION CONFIGURATION MACROS
    #define MQTT_BROKER_ADDRESS               "test.mosquitto.org"
    #define MQTT_PORT                         1883
    
    // ii. MQTT NON-SECURE
    #define MQTT_SECURE_CONNECTION            ( 0 )
    
    /* iii. The MQTT topics to be used by the publisher and subscriber. */
    #define MQTT_PUB_TOPIC                    "PSoC6Status"
    #define MQTT_SUB_TOPIC                    "PSoC6Status"
    
    /* iv. Set the QoS that is associated with the MQTT publish, and subscribe message. */
    #define MQTT_MESSAGES_QOS                 ( 0 )

`````

3. Build และ upload โปรแกรมลงบอร์ด 
4. เขียนโปรแกรม Node-red ใช้ MQTT  เป็น publisher ควบคุม LED บอร์ด PSoC6 
<<<<<<< HEAD
    - import code โปรแกรม node-red และตั้งค่า node-red ตามขั้นตอน [setting Node-red](https://github.com/Advance-Innovation-Centre-AIC/Lab_NodeRed_MQTT_control_LED_Psoc6#setting-node-red)(https://www.dropbox.com/scl/fi/u3asndwvf30y1gog2t6lp/BDH_mtb_example-node-red_control_leb_psoc6_via_mqtt.paper?dl=0&rlkey=vqr4wopa8oxgyw5ug59u624sp#:uid=879208286062881172009041&h2=Setting-Node-red) 

5.  run โปรแกรม Node-red และเปิดเบราว์เซอร์ url: http://127.0.0.1:1880/ui
6. ทดลองกด ON/OFF บนเบราว์เซอร์ node-red สังเกตุ LED บอร์ด ติด-ดับ 


Fig: output ON/OFF control by Node-red
![Fig: output ON/OFF control by Node-red](https://paper-attachments.dropboxusercontent.com/s_DE46169648EC1184505F0FEE30B79C93229F6F5C4674567BCFF38BB678D2F8D7_1681982082489_image.png)


## **Setting Node-red** 
1. เริ่มใช้งาน  node-red → [ติดตั้ง Node-red บน Windows](https://github.com/Advance-Innovation-Centre-AIC/EE_Curriculum/blob/main/term2_65_EMB62_IoT/LAB01/Get_started_Node-red.md#%E0%B8%95%E0%B8%B4%E0%B8%94%E0%B8%95%E0%B8%B1%E0%B9%89%E0%B8%87-node-red-%E0%B8%9A%E0%B8%99-windows)
2. import code Button control LED PSoC6
    1. คลิ๊กที่ ไฟล์ source >>[Button control LED PSoC6.json](https://github.com/Advance-Innovation-Centre-AIC/Lab_NodeRed_MQTT_control_LED_Psoc6/blob/2ced2d2bc014f3dbe35f54c52da94548c25be7fc/flow/ButtonControl_LED_PSoC6.json)
    2. copy code ไปที่ แถบขวามือ>>คลิ๊กที่แถบ ขีดสามขีด>> เลือก import


fig: import function


![fig: import function](https://camo.githubusercontent.com/5a37c5f182695a69f125fdd207bf995385cf5c8d6ab5ed87048e6928f4595c9e/68747470733a2f2f70617065722d6174746163686d656e74732e64726f70626f7875736572636f6e74656e742e636f6d2f735f453532434539363336434332314535344342373834434341384132374342353633354439364536373037383033364238413842393236444142443634383644365f313637363139383932383536355f556e7469746c65642e706e67)




    c. วาง code ลงหน้า cilpboard >> เลือก new flow >> คลิ๊ก import


![](https://camo.githubusercontent.com/a8fbe0623c069fe495f9b36538e32d60a5f637ba013be3131af2ea78ab35c78f/68747470733a2f2f70617065722d6174746163686d656e74732e64726f70626f7875736572636f6e74656e742e636f6d2f735f453532434539363336434332314535344342373834434341384132374342353633354439364536373037383033364238413842393236444142443634383644365f313637363139393230363238375f556e7469746c65642e706e67)





    d. สังเกตที่แถบ flow จะมี flow ชื่อว่า “Button control LED PSoC6” ปรากฎ

*หมายเหตุ:* สำหรับหรับ node-red ที่ติดตั้งใหม่ ให้ติดตั้ง module dashboard เพิ่ม 

    - ไปที่ Manage palette >>Install>> search modules >> พิมพ์ “Dashboard >> เลือก node-red-dashboard >> install
    - ดูตัวอย่างการใช้งาน dashboard เบื้องต้นได้จาก [LAB: Basic Node-red Dashboard](https://github.com/Advance-Innovation-Centre-AIC/EE_Curriculum/tree/main/term2_65_EMB64_Applied_ES/LAB11#lab11-basic-node-red-dashboard)



![](https://paper-attachments.dropboxusercontent.com/s_DE46169648EC1184505F0FEE30B79C93229F6F5C4674567BCFF38BB678D2F8D7_1683012990126_image.png)


3. ตรวจสอบการตั้งค่า ใช้ node-red เป็น Publish โดยดับเบิ้ลคลิ๊กที่ mqtt out node หรือ PSoC6Status node ที่โปรแกรม node-red และ แก้ไขการตั้งค่าข้อมูลช่องให้ตรงตามข้อมูลด้านล่าง ดังนี้
    1. Server >> แก้ไขเป็น “IP address ของ Broker” port: “1883”
    2. Topic >> ตั้งค่าเป็น PSoC6Status
    3. QoS >> ตั้งค่าเป็น “0”


fig: mqtt out node


![fig: mqtt out node](https://paper-attachments.dropboxusercontent.com/s_DE46169648EC1184505F0FEE30B79C93229F6F5C4674567BCFF38BB678D2F8D7_1683013295944_image.png)




4. ตรวจสอบการตั้งค่า ใช้ node-red dashboard ปุ่ม button node ทั้ง 2 node สำหรับควบคุม On/Off LED 
   - การตั้งค่าตรงดังภาพด้านล่างหรือไม่ 
    



 fig: button node for OFF 

![fig: button node for OFF LED](https://paper-attachments.dropboxusercontent.com/s_DE46169648EC1184505F0FEE30B79C93229F6F5C4674567BCFF38BB678D2F8D7_1683013661342_image.png)

 fig: button node for ON LED

![fig: button node for ON LED](https://paper-attachments.dropboxusercontent.com/s_DE46169648EC1184505F0FEE30B79C93229F6F5C4674567BCFF38BB678D2F8D7_1683013674573_image.png)



![fig: button node for ON LED](https://paper-attachments.dropboxusercontent.com/s_DE46169648EC1184505F0FEE30B79C93229F6F5C4674567BCFF38BB678D2F8D7_1683013674573_image.png)


fig: button node set to switch off

![fig: button node set to switch off](https://paper-attachments.dropboxusercontent.com/s_DE46169648EC1184505F0FEE30B79C93229F6F5C4674567BCFF38BB678D2F8D7_1681980738044_Untitled.png)


fig: button node set to switch on

![fig: button node set to switch on](https://paper-attachments.dropboxusercontent.com/s_DE46169648EC1184505F0FEE30B79C93229F6F5C4674567BCFF38BB678D2F8D7_1681980818819_Untitled.png)



4. คลิ๊ก Deploy เพื่อให้ Node-red ทำงานตามโปรแกรมที่ได้ตั้งค่าไว้
5. เปิดหน้าเบราว์เซอร์ขึ้นมา ใส่ “your ip address:1883/ui” จะปรากฎหน้า dashboard ของ node-red

ตัวอย่าง
`````
        http://127.0.0.1:1880/ui
`````


![fig: node-red dashboard](https://paper-attachments.dropboxusercontent.com/s_DE46169648EC1184505F0FEE30B79C93229F6F5C4674567BCFF38BB678D2F8D7_1681981115605_image.png)







----------




[https://github.com/Infineon/mtb-example-wifi-mqtt-radar-presence](https://github.com/Infineon/mtb-example-wifi-mqtt-radar-presence)

[https://github.com/Infineon/mtb-example-wifi-dual-core-virtual-mqtt-client](https://github.com/Infineon/mtb-example-wifi-dual-core-virtual-mqtt-client)

[https://www.infineon.com/dgdl/Infineon-ModusToolbox_3.0_Eclipse_IDE_User_Guide-GettingStarted-v01_00-EN.pdf](https://www.infineon.com/dgdl/Infineon-ModusToolbox_3.0_Eclipse_IDE_User_Guide-GettingStarted-v01_00-EN.pdf?fileId=8ac78c8c8386267f0183a8d7043b58ee&utm_source=cypress&utm_medium=referral&utm_campaign=202110_globe_en_all_integration-files&redirId=188241)


