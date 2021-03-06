Code generator SolarThermalSystem
=================================

This CoTeTo code generator is able to generate 16 different structures of a solar thermal system.

The maximum configuration of the solar thermal system includes the
four solar collector fields `col1` to `col4` with potentially
different orientations, collector field areas and storage volumens:

![SolarThermalSystem_1111](https://github.com/UdK-VPT/BuildingSystems/blob/master/BuildingSystems/Resources/CodeGeneration/CoTeTo/Images/SolarThermalSystem_1111.png)

The collector field areas are individually controlled by four two-point controllers.
The incoming cold water, defined by the components `bou1` to `bou4`, flows
through the four thermal hot water storages `sto1` to `sto4`. The
leaving hot water is unified by the four junctions `jun1` to `jun4`.

The head of the code generator file main.mot includes several Bolean variables (`colLoop1` to `colLoop4`) and
also variables with numerical values of Modelica model parameters:

```python
<%
# model structure
colLoop1 = True
colLoop2 = False
colLoop3 = True
colLoop4 = False
# model class name
modelClassName = 'SolarThermalSystem_'+str('1' if colLoop1 == True else '0')\
  +str('1' if colLoop2 == True else '0')\
  +str('1' if colLoop3 == True else '0')\
  +str('1' if colLoop4 == True else '0')
# model parameters
# Location
weatherDataClass = 'WeatherDataMeteonorm.WeatherDataFile_Germany_Berlin'
#weatherDataClass = 'WeatherDataMeteonorm.WeatherDataFile_Egypt_ElGouna'
#weatherDataClass = 'WeatherDataMeteonorm.WeatherDataFile_Brasil_Manaus'
#weatherDataClass = 'WeatherDataMeteonorm.WeatherDataFile_USA_SanFrancisco'
# collector area
areaCol1 = 20.0 # m2
areaCol2 = 20.0 # m2
areaCol3 = 20.0 # m2
areaCol4 = 20.0 # m2
# collector orientation
azimuthAngleCol1 = 0.0 # degree
azimuthAngleCol2 = 90.0 # degree
azimuthAngleCol3 = 180.0 # degree
azimuthAngleCol4 = -90.0 # degree
tiltAngleCol1 = 30.0 # degree
tiltAngleCol2 = 30.0 # degree
tiltAngleCol3 = 30.0 # degree
tiltAngleCol4 = 30.0 # degree
# storage volume
volumeSto1 = 1.0 # m3
volumeSto2 = 1.0 # m3
volumeSto3 = 1.0 # m3
volumeSto4 = 1.0 # m3
# collector area specific mass flow
mFlowColArea = 30/3600 # kg/(m2*s)
%>
...
```

With the help of these variables and the logic of the code template 16 different
structures of the solar thermal system can be automatically generated:

![SolarThermalSystem_structuralVariants](https://github.com/UdK-VPT/BuildingSystems/blob/master/BuildingSystems/Resources/CodeGeneration/CoTeTo/Images/SolarThermalSystem_structuralVariants.png)

This is realised by activating and deactivating different parts of the code template:

```python
...
% if colLoop1 == True:
  // collector loop 1
  BuildingSystems.Climate.SolarRadiationTransformers.SolarRadiationTransformerIsotropicSky solRadTra1
    "Solar radiation transformer 1"
    annotation (Placement(transformation(extent={{-78,24},{-58,44}})));
...
  BuildingSystems.Technologies.SolarThermal.ThermalCollector col1(
    redeclare package Medium = Medium,
    angleDegTil=${tiltAngleCol1},
    angleDegAzi=${azimuthAngleCol1},
    redeclare BuildingSystems.Technologies.SolarThermal.Data.Collectors.FlatSolarCollector1 collectorData,
    m_flow_nominal=0.1,
    dp_nominal=1,
    width=${areaCol1},
    height=1,
    AColData=false)
    "Solar thermal collector 1"
    annotation (Placement(transformation(extent={{-60,4},{-40,24}})));
...  
% endif
...
```

In addition, centrally defined important system parameters

* the weather location,
* the azimuth and tilt angle of the collector fields,
* the area of the collector fields,
* the volumes of the thermal storages and
* the area specific mass flow rate through the collector

are substituted in the component models during the code generation process.
