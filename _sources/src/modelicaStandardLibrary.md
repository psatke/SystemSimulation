# Modelica Standard Library
Die Bibliothek strukturiert sich wie folgt:
`````{grid}
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Electrical.Analog
^^^
```{figure} ../data/img/Lib-Electrical.png
```

````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Electrical.Digital
^^^
```{figure} ../data/img/Lib-Digital.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Electrical.
Machines
^^^
```{figure} ../data/img/Lib-Machines.png
```
````
`````
`````{grid}
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Magnetic.
FluxTubes
^^^
```{figure} ../data/img/Lib-FluxTubes.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Mechanics.
Translational
^^^
```{figure} ../data/img/Lib-Translational.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Mechanics.
Rotational
^^^
```{figure} ../data/img/Lib-Rotational.png
```
````
`````
`````{grid}
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Mechanics.
MultiBody
^^^
```{figure} ../data/img/Lib-MultiBody1.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Fluid
^^^
```{figure} ../data/img/Lib-Fluid.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Media
^^^
```{figure} ../data/img/Lib-Media.png
```
````
`````
`````{grid}
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Thermal.
FluidHeatFlow
^^^
```{figure} ../data/img/Lib-Thermal.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Thermal.
HeatTransfer
^^^
```{figure} ../data/img/Lib-HeatTransfer.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Blocks
^^^
```{figure} ../data/img/Lib-Blocks1.png
```
````
`````
`````{grid}
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.StateGraph
^^^
```{figure} ../data/img/Lib-StateGraph.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Math
^^^
```{figure} ../data/img/Lib-Math.png
```
````
````{grid-item-card}
:class-header: bg-light
:text-align: center
Modelica.Utilities
^^^
```{figure} ../data/img/Lib-Utilities.png
:width: 100 px
```
````
`````

Wie können wir mit den Klassen, die bereits in der Bibliothek deklariert wurden, arbeiten bzw. Systeme modellieren?

## Rückblick: Abkühlung eines Raumes

Die [Aufgabe: Abkühlung eines Raumes](einfuehrung:grundlagen:abkuehlung) aus der Einführung wird anschließend mit dem objektorientierten Ansatz gelöst.

```````{tab-set}
````{tab-item} Erinnerung: Aufgabe
```{figure} ../data/img/AbkuehlungRaum.png
:width: 550px
```
````
````{tab-item} Erinnerung: Modelica Modell
```modelica
model Raum "Abkuehlung eines unbeheizten Raums"
	extends SimModel(tStop=86400);
	
	parameter Real A   (unit="m2")      =13     "Wandfläche";
	parameter Real c_p (unit="J/(kg.K)")=1000   "spezifische Waermekapazität (isobar)";
	parameter Real k   (unit="W/(m.K)") =0.28   "Waermeleitfaehigkeit";
	parameter Real rho (unit="kg/m3")   =1.2    "Dichte";
	parameter Real L   (unit="m")       =0.4    "Wanddicke";
	parameter Real T_R0(unit="K")       =294.15 "Anfangstemperatur des Innenraums";
	parameter Real T_A (unit="K")       =273.15 "Aussentemperatur";
	parameter Real V   (unit="m3")      =65     "Volumen";
	
	Real T_R(unit="K") "Innenraumtemperatur";
	
	initial equation
		T_R=T_R0;
		
	equation
		c_p*V*rho*der(T_R)=k*A/L*(T_A-T_R);
end Raum;
```
````
````{tab-item} Lösung: OOP
```Modelica
model RaumOOP
    extends SimModel(tStop=86400);
    Modelica.Thermal.HeatTransfer.Components.HeatCapacitor    thermischeMasse(
		C=78000,
		T(start=294.15));
    Modelica.Thermal.HeatTransfer.Components.ThermalConductor konduktion(G=9.1);
    Modelica.Thermal.HeatTransfer.Sources.FixedTemperature    festeTemperatur(T=273.15);
    equation
        connect(thermischeMasse.port, konduktion.port_a);
        connect(konduktion.port_b, festeTemperatur.port);
end RaumOOP;
```
````
```````

Neben der rein textbasierten Modellierung, bietet SimulationX eine **graphische Benutzeroberfläche**, die genutzt werden kann, um Objekte zu instanziieren,
Verbindungen zu erstellen und Parameter zu ändern. Die Abkühlung des Raumes wird im Folgenden graphisch modelliert. Vergleicht man den entstehenden Quellcode der graphischen Modellierung mit der vorherigen objektorientierten Lösung, fallen die zusätzlichen `annotations` auf. Diese beschreiben die Position der graphischen Darstellung der Objekte.

```````{tab-set}
````{tab-item} Lösung: Graphisch
```{figure} ../data/img/AbkuehlungRaumGraphisch.png
:width: 550px
```
````
````{tab-item} Quellcode: Graphisch
```modelica
model RaumGraphisch
	extends SimModel(tStop=86400);
	Modelica.Thermal.HeatTransfer.Components.HeatCapacitor thermischeMasse(
		C=78000,
		T(start=294.15)) annotation(Placement(transformation(extent={{-120,65},{-100,85}})));
	Modelica.Thermal.HeatTransfer.Components.ThermalConductor konduktion(G=9.1) annotation(Placement(transformation(extent={{-85,45},{-65,65}})));
	Modelica.Thermal.HeatTransfer.Sources.FixedTemperature festeTemperatur(T=273.15) annotation(Placement(transformation(extent={{-15,45},{-35,65}})));
	equation
		connect(konduktion.port_a,thermischeMasse.port) annotation(Line(
			points={{-85,55},{-90,55},{-110,55},{-110,60},{-110,65}},
			color={191,0,0},
			thickness=0.0625));
		connect(festeTemperatur.port,konduktion.port_b) annotation(Line(
			points={{-35,55},{-40,55},{-60,55},{-65,55}},
			color={191,0,0},
			thickness=0.0625));
end RaumGraphisch;
```
````
```````
```{admonition} Hinweis
Um die Anfangstemperatur der thermischen Masse zu definieren, muss in die Textansicht gewechselt werden. Hier kann die Anfangstemperatur durch `T(start=294.15)` ergänzt werden. Die graphische Benutzeroberfläche von SimulationX ermöglicht lediglich die Änderung von Parametern, von Objekten, die direkt initialisiert werden.
```

## Aufgaben zur Modelica Standard Library

1. Finden Sie die Zusammenhänge zwischen unserer bisherigen Modellierung des Raums und den neuen Parametern `C` und `G`. Nutzen Sie dafür die Klassendefinitionen der Modelica Standard Library.

2. Welche formale Bedeutung hat das Verbinden von den Anschlüssen der Objekte? Schauen Sie dafür in die Deklaration des Connectors.

3. Erweitern Sie das bisherige graphische Modell um die thermische Masse der Wand.


Mögliche Aufgaben zur graphischen Modellierung:
 - Erweitern Sie das bisherige Modell um die thermische Kapazität der Wand (aufgeteilt in Mauerwerk und Dämmung)
 - Erweitern Sie ihr Modell für Fensterflächen (ohne Strahlung)
 - Ergänzen Sie eine Innere Wärmequelle (eine Person ca. 80W)