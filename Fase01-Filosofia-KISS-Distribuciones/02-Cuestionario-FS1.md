# Cuestionario-1: Filosofía Open Source, KISS y Distribuciones

*2026-09-07 13:22*

**tags:** [[]]

---

### Pregunta:

1. **La suite ofimática LibreOffice está bajo la licencia LGPLv3, mientras que Apache OpenOffice utiliza la Apache License 2.0.
¿Qué implicación técnica y de desarrollo tiene esta diferencia de licencias en la adopción de código entre ambos proyectos?**

### Respuesta:

- La licencia LGPLv3 tiene restricciones de coypleft débil, mientras que Apache License 2.0 es una licencia libre no recíproca (permisiva).
- Esto implica que LibreOffice puede incorporar mejoras hechas por Apache OpenOffice, pero Apache OpenOffice no puede incorporar las mejoras hechas por LibreOffice.
- Esta asimetría, sumada a una comunidad más activa, ha provocado que la mayoría de las distribuciones adopten LibreOffice como su suite ofimática predeterminada.

---

### Pregunta:

2. **¿Cuál es la diferencia fundamental entre el modelo de licenciamiento Copyleft (como la licencia GPL) y una licencia Permisiva (como la BSD de 3 cláusulas)?**
**¿Qué libertad otorga esta última a las empresas que desarrollan software comercial?**

### Respuesta:

- Las licencias de tipo Copyleft (como la GPL) protegen la libertad del software a futuro obligando a que cualquier obra derivada también sea compartida bajo las mismas condiciones (principio recíproco).
- En cambio, las licencias permisivas (como la BSD de 3 cláusulas) no obligan a mantener los términos de uso en obras modificadas.
- Esto otorga la libertad a desarrolladores o empresas de cerrar el código modificado y distribuirlo o comercializarlo de forma privativa sin revelar sus cambios.

---

### Pregunta:

3. **La Free Software Foundation lanzó la licencia GNU Affero General Public License (AGPL).**
**¿Qué vacío legal o técnico de la GPL estándar vino a solucionar en la era de los servicios web (SaaS).**

### Respuesta:

- La AGPL cierra una "brecha" de la GPL estándar en entornos de servidor.
- Bajo la GPL tradicional, si un desarrollador modifica el software pero solo ofrece el acceso a través de la red (Software como Servicio o SaaS), no está "redistribuyendo" físicamente el binario y, por ende, no estaba obligado a compartir sus modificaciones.
- La GNU AGPL estipula que si el programa se ejecuta en red para dar servicio a terceros, el código fuente con todos los cambios realizados debe estar disponible para descarga.

---

### Pregunta:

4. **Si una empresa requiere la estabilidad y el soporte a largo plazo de Red Hat Enterprise Linux (RHEL)  pero carece de presupuesto para pagar suscripciones, ¿qué distribución histórica de la familia Red Hat recompila el código fuente libre de RHEL para ofrecerlo de forma gratuita?**

### Respuesta:

- La dsitribución que cumple con este propósito es CentOS (*Community Enterprise OS*).
- CentOS utiliza el códgo fuente disponible de Red Hat Enterprise Linux para compilar una distribución idéntica y de uso completamente gratuito, prescindiendo únicamente del soporte comercial de Red Hat.

---

### Pregunta:
5. **Explica en qué consiste el modelo de actualización Rolling Release característico de Arch Linux y contrástalo con el ciclo de lanzamientos fijos y versiones LTS de distribuciones como Ubuntu.**

### Respuesta:

- *Rolling Release:* no tiene "versiones de sistema" fijas. El sistema se actualiza de forma continua y fluida mediante un único comando, entregando inmediatamente los últimos kernels, controladores y aplicaciones en cuanto son liberados de forma estable por sus desarrolladores originales (upstream).

- *LTS:* Las distribuciones de lanzamiento fijo publican versiones cerradas periódicamente (cada 6 meses en Ubuntu, con ediciones de soporte a largo plazo o LTS cada 2 años que tienen 5 años de soporte). Requieren actualizaciones completas del sistema o reinstalaciones cada cierto tiempo para migrar de una versión a otra.

---

### Pregunta:

6. **¿Qué es el AUR? Explica la diferencia operativa para el usuario entre instalar un paquete oficial usando `pacman` e instalar un paquete comunitario desde el AUR.**

### Respuesta:

- Los repositorios oficiales contienen paquetes binarios precompilados que se instalan y actualizan directamente con el gestor `pacman` en segundos.
- El *AUR* es un repositorio comunitario que no contiene programas precompilados, sino scripts de construcción llamados PKGBUILDs. El usuario debe adquirir estos scripts (usualmente clonándolos con `git clone`), verificar que el PKGBUILD no sea malicioso, compilar el software en su propia máquina usando la herramienta `makepkg`, y finalmente instalar el paquete resultante mediante `pacman -U`.

---

### Pregunta:

7. **Si estás escribiendo una guía técnica para documentar tu sistema Arch Linux y quieres liberarla bajo Creative Commons, pero deseas exigir que se te reconozca la autoría (atribución), que no se use para fines comerciales y que las obras derivadas hereden exactamente las mismas condiciones, ¿qué variante de licencia CC debes aplicar?**

### Respuesta:

- La licencia correspondiente es CC BY-NC-SA, esta combinación modular se compone de:
  - *BY (Atribución):* Requiere nombrar al autor original.
  - *NC (No Comercial):* Prohíbe la explotación comercial de la obra.
  - *SA (Compartir igual / Share-Alike):* Hereda el principio del copyleft, obligando a que cualquier obra derivada sea compartida bajo esta misma licencia.

---

**Referencias:**
- [[01-Teoria-FS1]]

