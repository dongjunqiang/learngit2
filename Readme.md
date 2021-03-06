# goipmi

Package goipmi provides utilities to access to i and pass around stack traces.
对使用从ipmi协议进行通信的服务器采集数据提供了底层的接口，
主要是dsn.go对外提供链接服务器接口，client_sdr.go 对外提供采集数据接口，

client_sdr.go 
## Usage
#### type SdrSensorInfo
```go
type SdrSensorInfo struct {
	SensorType  string
	BaseUnit    string
	Value       float64
	DeviceId    string
	StatusDesc  string
	SensorEvent []string
	Avail       bool
}
```
SdrSensorInfo 定义用户所关心采集数据的结构：SensorType所采集类型，如"Temperature", "Voltage", "Current", "Fan"；BaseUnit数据的单位，如"degrees C", "Volts", "Amps", "Watts", "Joules"；value所采集数据类型的值(float64)；DeviceId描述所采集Sensor，如“PSU FAULT”，“BIOS”，“Watchdog”等；StatusDesc描述sensor类型是Threshold类型的sensor的状态描述；SensorEvent记录sensor类型是Sensor-specific或generic时envent的列表；avail描述所采集的数据值是否可用，false为unavailable，true为available

#### func  GetSensorList
```go
func GetSensorList(reservationID uint16) ([]SdrSensorInfo, error) 
```
reservationID是采集前获取的初始值，
用来迭代RecordId直到RecordId等于oxffff，将获取的信息保存到数组SdrSensorInfo中

#### func  GetSDR
```go
func GetSDR(reservationID uint16, recordID uint16) (sdr *sDRRecordAndValue, next uint16, err error) 
```
执行ipmi命令   ‘Get SDR’commands ，
reservationID保持连接用的，
用来迭代RecordId直到RecordId等于oxffff，将获取的信息保存到数组SdrSensorInfo中
## License

This project is available under the [Apache 2.0](./LICENSE) license.
