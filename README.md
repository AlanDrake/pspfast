# PaintShop Pro 2023 Performance Patch
A native DLL patch that improves canvas rendering and UI responsiveness in Corel PaintShop Pro 2023 (64-bit).

It fixes inefficient canvas rendering, incremental-panning, brush-tool switching, palette rebuilding, scrollbar updates, OpenMP worker waiting, and other performance bottlenecks found in the stock application. Supported builds use a persistent Direct2D bitmap and GPU rendering cache, while unsupported builds remain unchanged and fall back to the original behavior.

The patch is delivered as a d2d1.dll proxy that forwards normal Direct2D calls to Windows and applies fixes only while PSP 2023 is running. It does not modify Corel’s files. Optional diagnostic switches allow individual fixes to be disabled or frame output to be logged and verified.

## Installation

1. Download the [dll wrapper](https://github.com/AlanDrake/pspfast/releases)
2. Close PaintShop Pro.
3. Copy `d2d1.dll` into the PaintShop Pro installation folder:

```text
C:\Program Files\Corel\Corel PaintShop Pro 2023 (64-bit)
```

The patch is applied only in memory when PaintShop Pro starts, it does not modify any Corel files.

To uninstall, delete `d2d1.dll` from the PaintShop Pro installation folder and restart the application.

## Diagnostic Switches

Create an empty file with the corresponding name in `%TEMP%` to activate a switch. For example:

```cmd
echo. > "%TEMP%\pspfast.off"
```

Delete the file to disable the switch.

| Switch | Effect |
|---|---|
| `pspfast.off` | Disables the canvas and UI fixes and uses stock PaintShop Pro behavior. |
| `pspfast.noincr` | Disables incremental rendering while panning. |
| `pspfast.novariance` | Disables the Brush Variance palette optimization. |
| `pspfast.notip` | Disables lazy creation of the brush-tip picker. |
| `pspfast.nocombo` | Disables the blend-mode combo-box optimization. |
| `pspfast.log.on` | Writes diagnostic information to `%TEMP%\pspfast.log`. |
| `pspfast.verify` | Compares incremental frames against full renders. This is slow and intended for debugging. |
| `pspfast.vsync` | Keeps the stock vertical-sync behavior. |
| `pspfast.ompspin` | Keeps the stock OpenMP worker-thread behavior. |

Switches are checked periodically while the application is running, so they can usually be changed without restarting PaintShop Pro.
