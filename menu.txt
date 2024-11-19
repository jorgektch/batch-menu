@echo off
:menu
cls
echo ==========================
echo           Menu
echo ==========================
echo Directorio actual: %cd%
echo ==========================
echo 1. Listado
echo 2. Cambiar Directorio
echo 3. Copiar Archivo
echo 4. Renombrar Archivo
echo 5. Eliminar Archivo
echo 6. Crear Archivo
echo 7. Crear Carpeta
echo 8. Eliminar Carpeta y todo su contenido
echo 0. Salir
echo ==========================
set /p "choice=Selecciona una opcion: "

if "%choice%"=="1" goto listado
if "%choice%"=="2" goto cambiarDirectorio
if "%choice%"=="3" goto copiarArchivo
if "%choice%"=="4" goto renombrarArchivo
if "%choice%"=="5" goto eliminarArchivo
if "%choice%"=="6" goto crearArchivo
if "%choice%"=="7" goto crearCarpeta
if "%choice%"=="8" goto eliminarCarpetaConTodo
if "%choice%"=="0" goto salir
echo Opcion invalida. Intentalo de nuevo.
pause
goto menu

:listado
cls
echo ==========================
echo Directorio actual: %cd%
echo ==========================
echo Listado de archivos y carpetas:
echo --------------------------
dir /T:C /T:W /T:A
echo --------------------------
pause
goto menu

:cambiarDirectorio
cls
echo ==========================
echo Directorio actual: %cd%
echo ==========================
set /p "ruta=Introduce la ruta del directorio: "
cd /d "%ruta%" 2>nul || (echo No se pudo cambiar al directorio especificado. Verifica la ruta e intentalo de nuevo. & pause)
goto menu

:copiarArchivo
cls
echo ==========================
echo Directorio actual: %cd%
echo ==========================
set /p "archivo=Introduce el nombre del archivo a copiar (en el directorio actual): "
if not exist "%cd%\%archivo%" (echo El archivo no existe en el directorio actual. & pause & goto menu)
set /p "destino=Introduce la ruta de destino: "
copy "%cd%\%archivo%" "%destino%" >nul && (echo Archivo copiado con exito.) || (echo Error al copiar el archivo. Verifica la ruta y permisos.)
pause
goto menu

:renombrarArchivo
cls
echo ==========================
echo Directorio actual: %cd%
echo ==========================
set /p "archivo=Introduce el nombre del archivo a renombrar (en el directorio actual): "
if not exist "%cd%\%archivo%" (echo El archivo no existe en el directorio actual. & pause & goto menu)
set /p "nuevoNombre=Introduce el nuevo nombre para el archivo: "
ren "%archivo%" "%nuevoNombre%" >nul && (echo Archivo renombrado con exito.) || (echo Error al renombrar el archivo.)
pause
goto menu

:eliminarArchivo
cls
echo ==========================
echo Directorio actual: %cd%
echo ==========================
set /p "archivo=Introduce el nombre del archivo a eliminar (en el directorio actual): "
if not exist "%archivo%" (echo El archivo no existe en el directorio actual. & pause & goto menu)
if exist "%archivo%\" (echo La entrada corresponde a un directorio, no a un archivo. & pause & goto menu)
del "%archivo%" >nul && (echo Archivo eliminado con exito.) || (echo Error al eliminar el archivo.)

pause
goto menu
:crearArchivo
cls
echo ==========================
echo Directorio actual: %cd%
echo ==========================
set /p "archivo=Introduce el nombre del archivo a crear (debe ser unico): "
if exist "%cd%\%archivo%" (echo Ya existe un archivo con ese nombre en el directorio actual. & pause & goto menu)
echo. > "%archivo%" && (echo Archivo creado con exito.) || (echo Error al crear el archivo.)
pause
goto menu

:crearCarpeta
cls
echo ==========================
echo Directorio actual: %cd%
echo ==========================
set /p "carpeta=Introduce el nombre de la carpeta a crear (debe ser unica): "
if exist "%cd%\%carpeta%" (echo Ya existe una carpeta con ese nombre en el directorio actual. & pause & goto menu)
mkdir "%carpeta%" >nul && (echo Carpeta creada con exito.) || (echo Error al crear la carpeta.)
pause
goto menu

:eliminarCarpetaConTodo
cls
echo ==========================
echo Directorio actual: %cd%
echo ==========================
set /p "directorio=Introduce el nombre de la carpeta que desee eliminar definitivamente (en el directorio actual): "
if not exist "%directorio%\" (echo La carpeta no existe en el directorio actual. & pause & goto menu)
echo Estas a punto de eliminar la carpeta "%directorio%" y todo su contenido.
set /p "confirmar=¿Estas seguro? (S/N): "
if /i "%confirmar%"=="S" (rd /s /q "%directorio%" && (echo Carpeta eliminada con exito. ) || (echo Ocurrió un error al intentar eliminar la carpeta.)) else (echo Operacion cancelada.)
pause
goto menu
pause
goto menu

:salir
exit
