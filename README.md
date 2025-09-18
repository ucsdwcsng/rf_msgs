# RF sensor msgs for ROS

A catkin package that provides definitions for CSI and AOA messages. 
To be used in conjunction with the [wiros_csi_node](https://github.com/ucsdwcsng/wiros_csi_node) 
and [wiros_processing_node](https://github.com/ucsdwcsng/wiros_processing_node).

< Go back to the [index page](https://github.com/ucsdwcsng/WiROS)

## ROS1 and ROS2

This package by default supports ROS2, but there are legacy ROS1 build files you can use for ROS1 support.
```
# Enable ROS1 support
mv rf_msgs/CMakeLists_ROS1.txt rf_msgs/CMakeLists.txt
mv rf_msgs/package_ROS1.xml rf_msgs/package.xml
```

## Message Formats

### Wifi Message
Below are the fields of the WiFi messages and their meanings.

- uint8 ap_id : user-defined id to distinguish data from mulitple AP's. 
- uint8 txmac[] : Should be of length 6, the MAC address of the transmitter
- uint8 chan : Channel number
- uint32 n_sub : Number of subcarriers (including pilot subcarriers and channel edges)
- uint8 n_rows : Number of rows of the CSI matrix (number of receivers)
- uint8 n_cols : Number of cols of the CSI matrix (number of transmitters). Note that the ASUS will always detect 4 transmitters but some may be zero. In the future the node may automatically remove 0 transmitters.
- uint8 bw : Bandwidth in MHz
- uint8 mcs : Only used for quantenna
- int32 rssi : Signal strength in dB, measured for each receive antenna
- uint32 fc : Frame control byte
- uint32 seq : Frame sequence number/packet number 
- float64[] csi_real : Real part of Channel state information (CSI)
- float64[] csi_imag : Imaginary part of CSI
- string rx_id : IP of the asus that received the packet. This can be used to figure out which compensation data to apply.
- uint32 msg_id : Used for quantenna

The real[] and imag[] arrays are an n_sub x n_rx x n_tx 3-d array stored in fortran order (so, all subcarriers for a given rx-tx pair are stored next to eachother, and all receiver's subcarriers for a given tx are stored next to eachother.) So you can reshape them in python as
```
reshaped_array = flattened_array.reshape((n_sub, n_rx, n_tx), order='F')
```
### Bearing Message 
Below are the fields of the Bearing message and their meaning 

- uint8 ap_id: AP identifier which collected the CSI data, sequential starting from 0, up to (total number of AP's - 1)
- uint8[] txmac: Should be of length 6, the MAC address of the transmitter
- uint8 n_tx: number of antennas on the transmitter antenna
- uint8 n_rx: number of antennas on the receiver antenna
- uint32 seq: packet number
- int32[] rssi:  Signal strength in dB, measured for each receive antenna
- float64[] aoa: Angle of arrival in the coordinates of  the receiving WiFi radio for each transmit antenna. 
If CSI is averaged across the transmitter antennas, then all the values will be equal.   
- float64[] aod: Angle of departure in the coordinates of the transmitting WiFi radio. 
