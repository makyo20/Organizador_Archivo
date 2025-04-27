import os
import shutil

# Ruta de la carpeta que quieres organizar
carpeta_origen = 'ejemplos'  # Puedes cambiarla si quieres otra carpeta

# Diccionario que define carpetas por tipo de archivo
tipos_archivos = {
    'imagenes': ['.jpg', '.jpeg', '.png', '.gif'],
    'documentos': ['.pdf', '.docx', '.txt'],
    'hojas_calculo': ['.xls', '.xlsx', '.csv'],
    'videos': ['.mp4', '.mov', '.avi'],
}

# Crear las carpetas si no existen
for carpeta in tipos_archivos.keys():
    ruta_carpeta = os.path.join(carpeta_origen, carpeta)
    if not os.path.exists(ruta_carpeta):
        os.makedirs(ruta_carpeta)

# Crear también carpeta 'Otros' para archivos desconocidos
ruta_otros = os.path.join(carpeta_origen, 'Otros')
if not os.path.exists(ruta_otros):
    os.makedirs(ruta_otros)

# Inicializar contador de archivos movidos
archivos_movidos = 0

# Recorrer todos los archivos en la carpeta origen
for nombre_archivo in os.listdir(carpeta_origen):
    ruta_archivo = os.path.join(carpeta_origen, nombre_archivo)

    # Verificar si es un archivo (no una carpeta)
    if os.path.isfile(ruta_archivo):
        # Obtener la extensión del archivo
        _, extension = os.path.splitext(nombre_archivo)
        extension = extension.lower()

        # Buscar en qué categoría entra
        movido = False
        for categoria, extensiones in tipos_archivos.items():
            if extension in extensiones:
                ruta_destino = os.path.join(carpeta_origen, categoria, nombre_archivo)
                shutil.move(ruta_archivo, ruta_destino)
                print(f'Moviendo {nombre_archivo} a {categoria}')
                archivos_movidos += 1
                movido = True
                break

        # Si no encaja en ninguna categoría, moverlo a 'Otros'
        if not movido:
            ruta_destino = os.path.join(carpeta_origen, 'Otros', nombre_archivo)
            shutil.move(ruta_archivo, ruta_destino)
            print(f'Moviendo {nombre_archivo} a Otros')
            archivos_movidos += 1

# Mensaje final
if archivos_movidos == 0:
    print('\nNo había archivos para organizar.')
else:
    print(f'\n¡Organización completa! 🎉 Se movieron {archivos_movidos} archivos.')
