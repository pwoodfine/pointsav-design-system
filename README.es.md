<div align="center">

<img src="https://raw.githubusercontent.com/pointsav/pointsav-media-assets/main/ASSET-SIGNET-MASTER.svg" width="72" alt="PointSav Design System">

# Sistema de Diseño PointSav
### Protocolos Lingüísticos y Estándares Visuales
*La fuente única de verdad para la voz institucional y la identidad visual en cada despliegue de PointSav.*

[![Estado: Verificado](https://img.shields.io/badge/Estado-Verificado-22863a.svg?style=flat-square)](#repositorios-de-tokens)
[![WCAG: 2.2 AAA](https://img.shields.io/badge/WCAG-2.2_AAA-0075ca.svg?style=flat-square)](#estándares-visuales)
[![Licencia: Apache 2.0](https://img.shields.io/badge/Licencia-Apache_2.0-0075ca.svg?style=flat-square)](#licencia)

<br/>

**[→ Monorepo de Ingeniería](https://github.com/pointsav/pointsav-monorepo)** &nbsp;·&nbsp; **[→ Wiki de Documentación](https://github.com/pointsav/content-wiki-documentation)** &nbsp;·&nbsp; **[→ Despliegue en Producción](https://github.com/woodfine/woodfine-fleet-deployment)** &nbsp;·&nbsp; **[→ pointsav.com](https://pointsav.com)**

</div>

---

## Qué es Este Repositorio

La mayoría de los sistemas de diseño son colecciones de colores y tipografías. Este es diferente.

PointSav opera una plataforma donde los mismos datos subyacentes deben proyectarse con precisión en divulgaciones públicas para inversores, terminales internas de operadores, presentaciones regulatorias y superficies de marketing bilingüe. Un color que se muestra correctamente en modo oscuro debe mantener su validez legal en una presentación regulatoria impresa. Un protocolo lingüístico que enruta datos estructurados de forma determinista debe aplicarse de manera consistente tanto en Vancouver como en Madrid.

El sistema de diseño aplica estas restricciones como tokens legibles por máquina — archivos YAML que son consumidos automáticamente por cada superficie de despliegue. Un cambio a una regla tipográfica se propaga en todas partes. No existe un paso de alineación manual. No existe desviación de versiones. El sistema es la ley.

---

## Protocolos Lingüísticos

El lenguaje en un contexto institucional no es una preferencia estilística. Es un compromiso estructural. La palabra incorrecta en una divulgación para inversores es un evento regulatorio. La palabra correcta, aplicada de forma inconsistente entre jurisdicciones, genera una fractura legal.

PointSav trata el lenguaje como infraestructura. Los seis archivos de protocolo lingüístico de este repositorio codifican las reglas que rigen cada texto que produce la plataforma:

| Archivo de Token | Qué gobierna |
|:---|:---|
| `protocol-text.yaml` | Reglas de escritura fundamentales — tono, registro, estructura de oraciones, vocabulario prohibido |
| `protocol-comm.yaml` | Despacho interno — distinción de activos, clasificación de enrutamiento |
| `protocol-extract.yaml` | Reglas de análisis determinista — cero inferencia de IA, gramática léxica estricta |
| `protocol-legal.yaml` | Traducción de afirmaciones de mercado a hechos estructurales — estándares de lenguaje regulatorio |
| `protocol-translate.yaml` | Reglas de límite bilingüe — bloqueos de nombres propios, manejo de términos específicos por jurisdicción |

`protocol-translate.yaml` merece una mención especial. PointSav opera en Canadá, Estados Unidos, España y México. El mismo concepto de inversión — una Solución de Tenencia Directa — tiene una designación legal diferente en cada jurisdicción. El archivo de protocolo codifica qué términos son traducibles y cuáles deben permanecer en su idioma original independientemente del idioma del documento. Esto previene la fractura legal en documentos bilingües.

---

## Estándares Visuales

El sistema de tokens visuales aplica el cumplimiento de WCAG 2.2 AAA como una restricción estructural, no como una lista de verificación. El razonamiento es institucional y no filosófico: una divulgación para inversores que no cumple con los estándares de accesibilidad en una jurisdicción donde esos estándares tienen peso legal es un pasivo, no una inconveniencia.

Los cinco archivos de tokens visuales:

| Archivo de Token | Qué gobierna |
|:---|:---|
| `typography.yaml` | Composición tipográfica fluida, proporciones de cuadrícula suiza, reglas de renderizado de subpíxeles |
| `spacing.yaml` | Sistema de cuadrícula de 8px, mínimos de objetivo táctil, densidad accesible |
| `color.yaml` | Paleta específica por entidad, cumplimiento de modo oscuro, aplicación de ratios de contraste |
| `TOKEN-UI-PHYSICS.yaml` | Definiciones estructurales WCAG 2.2 AAA — ratios de contraste, indicadores de foco, movimiento |
| `TOKEN-TYPOGRAPHY-RENDER.yaml` | Antialias de subpíxeles, consistencia de renderizado multiplataforma |

---

## Gobernanza Legal

| Archivo de Token | Qué gobierna |
|:---|:---|
| `LEGAL-incubation-license.txt` | Protocolo de retención propietaria para componentes en fase de incubación |

---

## Cómo Se Usa el Sistema

La plataforma PointSav no edita documentos. Proyecta datos. Un Totebox Archive almacena todos los registros como archivos planos legibles por máquina. Las variantes de ConsoleOS leen esos archivos y los proyectan en interfaces legibles por personas utilizando los tokens de este repositorio como la ley de renderizado.

Esta separación es relevante por razones institucionales. Si la capa de renderizado estuviera integrada en la capa de datos, un cambio en un token de color podría, en principio, requerir modificar registros. Al mantener el sistema de diseño separado del archivo, las actualizaciones visuales se propagan sin tocar los registros subyacentes. La integridad de cumplimiento queda preservada.

---

## Uso y Licencia

Los archivos de tokens de este repositorio son consumidos automáticamente por los entornos de despliegue de PointSav y la flota de Woodfine Management Corp. También se publican bajo la Licencia Apache, Versión 2.0, para uso externo, bifurcación, modificación y redistribución.

Consulte el archivo `LICENSE` en la raíz del repositorio para el texto legal completo de Apache 2.0 y `NOTICE` para la atribución requerida. Las marcas denominativas, logotipos y activos de identidad de marca de PointSav y Woodfine NO están licenciados bajo Apache 2.0 — consulte `TRADEMARK.md` para la política de marcas.

**[→ pointsav.com](https://pointsav.com)** &nbsp;·&nbsp; **[→ github.com/pointsav](https://github.com/pointsav)**

---

*© 2026 Woodfine Capital Projects Inc. — Distribuido bajo la Licencia Apache, Versión 2.0. PointSav Digital Systems™ es una marca registrada de Woodfine Capital Projects Inc.*

<!-- BEGIN: factory-release-engineering license-section -->
<!-- ================================================================== -->
<!-- Esta sección se genera desde factory-release-engineering.           -->
<!-- No editar aquí. Proponga cambios río arriba.                         -->
<!-- ================================================================== -->

## Licencia

Este repositorio se distribuye bajo la **Licencia Apache, Versión 2.0**.
Véase el archivo `LICENSE` en la raíz del repositorio para consultar el
texto legal completo, el cual es la versión autoritativa, y `NOTICE`
para la atribución requerida.

Copyright (c) 2026 Woodfine Capital Projects Inc.

La licencia Apache 2.0 concede derechos sobre el código fuente, los
tokens de diseño, las recetas de componentes y la documentación de este
repositorio. **No** concede derechos sobre las marcas registradas,
nombres comerciales o activos de identidad de marca de PointSav o
Woodfine — consulte `TRADEMARK.md` para la política de marcas.

<!-- ================================================================== -->
<!-- This section is generated from factory-release-engineering.         -->
<!-- Do not edit here. Propose changes upstream.                          -->
<!-- ================================================================== -->

## License

This repository is licensed under the **Apache License, Version 2.0**.
See the `LICENSE` file in the root of this repository for the full
legal text, which is authoritative, and `NOTICE` for required
attribution.

Copyright (c) 2026 Woodfine Capital Projects Inc.

The Apache 2.0 license grants rights to the source code, design tokens,
component recipes, and documentation in this repository. It does **not**
grant rights to PointSav or Woodfine trademarks, word marks, or brand
identity assets — see `TRADEMARK.md` for the trademark policy.
<!-- END: factory-release-engineering license-section -->


---

*Copyright © 2026 Woodfine Capital Projects Inc. Consulte [LICENSE](LICENSE) para los términos.*

*Woodfine Capital Projects™, Woodfine Management Corp™, PointSav Digital Systems™, Totebox Orchestration™ y Totebox Archive™ son marcas registradas de Woodfine Capital Projects Inc., utilizadas en Canadá, Estados Unidos, América Latina y Europa. Todas las demás marcas son propiedad de sus respectivos titulares.*

*→ English version: [README.md](./README.md)*
