# What is Gatt
GATT is an acronym for the **Generic ATTribute**. GAP manage connection, GATT care about data transfer.

In contrast with GAP which defines the low-level interactions with devices, GATT deals only with actual data transfer procedures and formats. GATT also provides the reference framework for all GATT-based profiles

It makes use of a generic data protocol called the **Attribute Protocol (ATT)**, which is used to store Services, Characteristics and related data in a simple lookup table.

GATT defines the way that two Bluetooth Low Energy devices transfer data back and forth using **Services** and **Characteristics**

# Gatt table
the GATT layer of the Bluetooth low energy protocol stack is used by the application for data communication between two connected devices. 
Data is passed and stored in the form of characteristics which are stored in memory on the Bluetooth low energy device. 
From a GATT standpoint, when two devices are connected they are each in one of two roles.

- The **GATT server**
    the device containing the characteristic database that is being read or written by a GATT client.
- The **GATT client**
    the device that is reading or writing data from or to the GATT server.

# Wrap esp_attr_desc_t class

```cpp
 typedef struct
 {
     uint16_t uuid_length;              /*!< UUID length */
     uint8_t  *uuid_p;                  /*!< UUID value */
     uint16_t perm;                     /*!< Attribute permission */
     uint16_t max_length;               /*!< Maximum length of the element*/
     uint16_t length;                   /*!< Current length of the element*/
     uint8_t  *value;                   /*!< Element value array*/
 } esp_attr_desc_t;
```

- uuid_p là một con trỏ trỏ đến giá trị uuid bởi vì có nhiều loại uuid khác nhau với dataType khác nhau
- value cũng là một con trỏ, và nó có thể trỏ đến bất kì dataType nào
Và chúng ta không thích điều này
uuid: chúng ta sẽ lưu trữ giá trị uuid vào một biến ở trong class
