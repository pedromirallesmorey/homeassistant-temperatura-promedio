# 🏠 Cómo calcular la temperatura promedio de tu casa con Home Assistant

Este tutorial te guía paso a paso para crear un sensor personalizado que calcule la temperatura promedio de varias estancias en tu hogar usando Home Assistant.

## 🧠 ¿Qué vamos a hacer?

Crear un sensor personalizado que calcule la temperatura promedio de varias estancias y mostrarlo en una tarjeta Lovelace con estilo personalizado.

## 🛠️ Paso 1: Crear el sensor de temperatura promedio

Agrega este bloque en tu archivo configuration.yaml o en el archivo de sensores si lo tienes separado:

```yaml
sensor:
  - platform: template
    sensors:
      temperaturapromedio:
        friendly_name: "Temperatura promedio"
        unit_of_measurement: "°C"
        value_template: >
          {{ ( ( ( states('sensor.temperatura_salon_temperature') | float(15) ) + 
            ( states('sensor.temperatura_bano_temperature') | float(15) )  + 
            ( states('sensor.temperatura_cocina_temperature') | float(15) ) + 
            ( states('sensor.remote_dormitorio_temperatura') | float(15) ) + 
            ( states('sensor.casa_realfeel_temperature') | float(15) )) / 5 ) | float(0) | round(1) }}
```

🔍 Este sensor toma cinco lecturas de temperatura y calcula su promedio. El valor 15 es el valor por defecto si algún sensor no está disponible.

## 🎨 Paso 2: Mostrarlo en el dashboard con una tarjeta entities

Usa una tarjeta tipo entities en tu dashboard para mostrar las temperaturas individuales y el promedio:

```yaml
type: entities
title: Promedio temperatura
entities:
  - entity: sensor.temperatura_salon_temperature
    name: Salón
  - entity: sensor.temperatura_bano_temperature
    name: Baño
  - entity: sensor.remote_dormitorio_temperatura
    name: Dormitorio
  - entity: sensor.casa_realfeel_temperature
    name: Exterior
    card_mod:
      style: |
        :host {
          color: green;
          font-weight: 600;
          vertical-align: top;
        }
  - entity: sensor.temperaturapromedio
    name: PROMEDIO
    card_mod:
      style: |
        :host {
          color: blue;
          font-weight: 600;
          vertical-align: top;
        }
card_mod:
  style:
    .: |
      ha-card div#states div {
        margin-top: -10px !important;
        margin-bottom: -10px !important;
      }
```

## 🎥 Recursos adicionales

Aquí tienes algunos vídeos recomendados para complementar tu configuración en Home Assistant:

- 📹 [Smart home – get your temperature in Home Assistant](https://www.youtube.com/watch?v=UTokVY74fv0)
- 📹 [CONTROL TEMP🌡️HOME ASSISTANT](https://www.youtube.com/watch?v=7EFlfvvFphk)
- 📹 [Exprime tus datos con Home Assistant y sus atractivos sensores](https://www.youtube.com/watch?v=iNut3g4v-Q0)
- 📹 [Min/Max Ayudantes Home Assistant](https://www.youtube.com/watch?v=B8tMbjUZFyk)
