# 📘 Primer Modelo de Semáforos Inteligentes en Cruce con Metro – UPPAAL

## 📜 Descripción del modelo

En este primer modelo se simula un cruce entre coches y un metro. El objetivo es gestionar el flujo de tráfico y garantizar la seguridad en el cruce. Se implementan semáforos que regulan el paso de los coches y permiten a estos y al metro cruzar sin problemas. La principal funcionalidad de este modelo es la sincronización entre el semáforo de coches y el metro, asegurando que el metro solo cruce cuando no hay coches en el cruce, y que los coches solo crucen cuando el semáforo se lo permite (es decir, cuando el metro no está cruzando ni está cerca del cruce).

## ✅ Requisitos implementados

- [X] Los coches se detienen cuando el semáforo está en rojo.
- [X] El metro solo cruza cuando no hay coches en el cruce.
- [X] Sincronización del semáforo con la llegada del metro.
- [X] Gestión de múltiples coches en el cruce mediante contador.
- [X] Control del flujo de coches en ambos sentidos.
- [X] Semáforos también en ambos sentidos.

## 💡 Consideraciones del diseño

- El semáforo actúa como controlador central que regula el acceso al cruce.
- El metro tiene prioridad, no necesita semáforo para esta iteración, pero verifica condiciones de seguridad antes de cruzar.
- Se modelan múltiples instancias de coches y semáforos para simular concurrencia.
- Se usan canales para sincronización entre procesos (metroAproximando, metroSaliendo...).

## 🐞 Problemas encontrados y soluciones

| Problema                                      | Solución aplicada                                |
|----------------------------------------------|--------------------------------------------------|
| El uso de un boolean `enCruce` compartido, da problemas de sincronización ya que tenemos varios coches  | Reemplazado por __contador__ `cochesEnCruce`         |
| El canal `metroSaliendo` da problemas de sincronización similares al problema anterior, ya que tenemos 2 metros | Añadir un contador `metrosEnCruce`   |

## 📈 Posibles mejoras futuras

- Añadir nuevo semáforo inferior para gestionar el flujo de coches en el otro cruce.

## 🗃️ Archivos del repositorio

- `PrimerModelo.xml`: Primera iteración con metro, coches y semáforo básico.
