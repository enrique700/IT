# 🛠️ Comandos Útiles para Windows (Winget, Reparación, Mantenimiento)

Referencia rápida de comandos y scripts de PowerShell/CMD para instalación de programas, activación, reparación del sistema y preparación de equipos para clonación.

---

## 📑 Tabla de contenidos

- [📦 Instalación de aplicaciones con Winget](#-instalación-de-aplicaciones-con-winget)
- [⚙️ Instalación de Winget (si no está presente)](#️-instalación-de-winget-si-no-está-presente)
- [🪟 Activación de Windows y Office](#-activación-de-windows-y-office)
- [🖨️ Reparación de cola de impresión y escáner](#️-reparación-de-cola-de-impresión-y-escáner)
- [🧰 Reparación y estabilidad de Windows](#-reparación-y-estabilidad-de-windows)
- [🧹 Limpiar historial de "Ejecutar"](#-limpiar-historial-de-ejecutar)
- [⌨️ Forzar un solo idioma de teclado](#️-forzar-un-solo-idioma-de-teclado)
- [🔐 Quitar expiración de contraseña](#-quitar-expiración-de-contraseña)
- [🧨 Eliminar programas innecesarios (Debloat)](#-eliminar-programas-innecesarios-debloat)
- [🎯 Acceso directo de ping (CMD)](#-acceso-directo-de-ping-cmd)
- [💿 Preparar Windows para clonación](#-preparar-windows-para-clonación)
- [⏰ Apagado/Reinicio programado](#-apagadoreinicio-programado)

---

## 📦 Instalación de aplicaciones con Winget

**Actualizar todas las apps de Windows** (ejecutar en PowerShell):
```powershell
winget upgrade --all
```

**Instalar varios programas a la vez** (herramienta de Chris Titus Tech):
```powershell
iwr -useb https://christitus.com/win | iex
```

**Crear un usuario local en Windows** (crea el usuario `Usuario` con contraseña `CRD`):
```cmd
net user Usuario CRD /add
```

**Carpeta de MediaFire con archivos esenciales:**
📁 https://www.mediafire.com/folder/rw1ez1m0i74hx/Prog

---

## ⚙️ Instalación de Winget (si no está presente)

Ejecutar en PowerShell **como administrador**:

```powershell
$progressPreference = 'silentlyContinue'
Write-Host "Installing WinGet PowerShell module from PSGallery..."
Install-PackageProvider -Name NuGet -Force | Out-Null
Install-Module -Name Microsoft.WinGet.Client -Force -Repository PSGallery | Out-Null
Write-Host "Using Repair-WinGetPackageManager cmdlet to bootstrap WinGet..."
Repair-WinGetPackageManager -AllUsers
Write-Host "Done."
```

---

## 🪟 Activación de Windows y Office

- Link para descargar Office/Windows: https://massgrave.dev/genuine-installation-media
- Sitio principal: https://massgrave.dev/

**Pasos:**
1. Abre **PowerShell** (no CMD) como administrador — clic derecho en el menú de inicio → PowerShell o Terminal.
2. Copia y pega el siguiente comando y presiona Enter:
   ```powershell
   irm https://get.activated.win | iex
   ```
3. Verás las opciones de activación:
   - **[1] HWID** → activación de Windows
   - **[2] Ohook** → activación de Office

---

## 🖨️ Reparación de cola de impresión y escáner

**Detener y reiniciar el servicio de cola de impresión:**
```cmd
net stop spooler && net start spooler
```

**Reiniciar servicio de escaneo e imagen de Windows** (y el de impresión):
```cmd
net stop stisvc && net start stisvc
```

**Reiniciar aplicación de escaneo de terceros:**
```cmd
net stop DeviceAssociationService && net start DeviceAssociationService
```

**Reiniciar cola de impresión y eliminar rastros (trabajos atascados):**
```cmd
net stop spooler && del %systemroot%\System32\spool\printers\* /Q /F /S && net start spooler
```

**Acceder al menú antiguo de impresoras:**
```cmd
shell:::{A8A91A66-3A7D-4424-8D24-04E180695C7A}
```

---

## 🧰 Reparación y estabilidad de Windows

**Revisión y reparación de archivos corruptos:**
```cmd
sfc /scannow
```

**Revisión de salud del sistema (ejecutar en este orden exacto):**
```cmd
DISM /Online /Cleanup-Image /CheckHealth
DISM /Online /Cleanup-Image /ScanHealth
DISM /Online /Cleanup-Image /RestoreHealth
```

---

## 🧹 Limpiar historial de "Ejecutar"

```cmd
reg delete "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU" /f
```

---

## ⌨️ Forzar un solo idioma de teclado

Evita tener 2 distribuciones de teclado instaladas.

1. Abrir **CMD como administrador** y ejecutar:
   ```powershell
   Set-WinUserLanguageList en-US -Force
   ```
2. Cerrar y volver a abrir sesión en Windows.

---

## 🔐 Quitar expiración de contraseña

Abrir PowerShell como administrador. Sustituye `"usuario"` por el nombre real de la cuenta:

```powershell
Set-LocalUser -Name "usuario" -PasswordNeverExpires $true
```

---

## 🧨 Eliminar programas innecesarios (Debloat)

> ⚠️ **En pruebas.** Ayuda a optimizar Windows eliminando programas basura, pero debe hacerse con cuidado porque podría eliminar componentes clave del sistema.

Ejecutar en PowerShell como administrador:

```powershell
& ([scriptblock]::Create((irm "https://debloat.raphi.re/")))
```

Más detalles de qué elimina: https://github.com/Raphire/Win11Debloat

---

## 🎯 Acceso directo de ping (CMD)

Crea un acceso directo que ejecuta `cmd.exe /k "ping ... -t"` contra una máquina predefinida.

1. Clic derecho en el Escritorio → **Nuevo** → **Acceso directo**.
2. En "Escriba la ubicación del elemento" pon:
   ```cmd
   C:\Windows\System32\cmd.exe /k "ping NOMBRE_MAQUINA -t"
   ```
3. Siguiente → ponle un nombre → Finalizar.

Al abrir el acceso directo se abre CMD con el ping ya ejecutándose y la ventana queda abierta.

---

## 💿 Preparar Windows para clonación

**Limpiar basura del sistema:**
```cmd
cleanmgr
```

**Ejecutar Sysprep** (si es posible, sino se puede omitir este paso):
```cmd
C:\Windows\System32\Sysprep\sysprep.exe
```

**Cambiar el nombre del equipo con PowerShell:**
```powershell
Rename-Computer -NewName "NUEVO-NOMBRE" -Restart
```

**Desactivar BitLocker:**
> Panel de control → Sistema y seguridad → Cifrado de unidad BitLocker → Desactivar BitLocker

**Ver estado de BitLocker:**
```cmd
manage-bde -status
```

---

## ⏰ Apagado/Reinicio programado

**Agregar reinicio semanal los domingos a las 3am** (CMD como administrador):
```cmd
schtasks /create /tn "ReinicioSemanal" /tr "shutdown.exe /r /f /t 0" /sc weekly /d SUN /st 03:00 /ru SYSTEM
```

**Verificar que la tarea se agregó:**
```cmd
schtasks /query /tn "ReinicioSemanal" /v /fo list
```

**Eliminar la tarea programada:**
```cmd
schtasks /delete /tn "ReinicioSemanal" /f
```

### 📌 Notas

- Para cambiar la frecuencia, modifica `/sc daily` por `/sc weekly` (semanal) o `/sc monthly` (mensual).
- El parámetro `/r` reinicia, `/f` fuerza el cierre de apps, y `/t 0` elimina el tiempo de espera.
- La hora se especifica en formato 24hr.

**Valores de día para `/d`:**

| Día        | Valor |
|------------|-------|
| Domingo    | SUN   |
| Lunes      | MON   |
| Martes     | TUE   |
| Miércoles  | WED   |
| Jueves     | THU   |
| Viernes    | FRI   |
| Sábado     | SAT   |
