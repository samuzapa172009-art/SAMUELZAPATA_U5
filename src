from collections import Counter
from pathlib import Path
import csv
import os
import re
import statistics
import matplotlib.pyplot as plt


# ==========================================================
# FUNCIONES PARA PROCESAR TEXTO
# ==========================================================

def obtener_palabras(texto):
    """Extrae palabras y evita diferencias por mayúsculas y puntuación."""
    return re.findall(r"\b\w+\b", texto.lower())


def resumen(path):
    """Muestra un resumen general del archivo de texto."""
    try:
        with open(path, "r", encoding="utf-8") as archivo:
            texto = archivo.read()

    except (OSError, UnicodeError) as error:
        print(f"No se pudo leer el archivo: {error}")
        return

    lineas = texto.splitlines()
    palabras = obtener_palabras(texto)

    caracteres = len(texto)
    caracteres_sin_espacios = sum(
        1 for caracter in texto if not caracter.isspace()
    )

    print("\n========== RESUMEN ==========")
    print(texto)
    print("\n========== ESTADÍSTICAS ==========")
    print(f"Líneas de texto: {len(lineas)}")
    print(f"Caracteres totales: {caracteres}")
    print(f"Palabras: {len(palabras)}")
    print(f"Caracteres sin espacios: {caracteres_sin_espacios}")


def palabras_frecuentes(path):
    """Muestra las cinco palabras más frecuentes."""
    try:
        with open(path, "r", encoding="utf-8") as archivo:
            texto = archivo.read()

    except (OSError, UnicodeError) as error:
        print(f"No se pudo leer el archivo: {error}")
        return

    palabras = obtener_palabras(texto)
    contador = Counter(palabras)

    if not contador:
        print("El archivo no contiene palabras.")
        return

    print("\n=== CINCO PALABRAS MÁS FRECUENTES ===")

    for palabra, frecuencia in contador.most_common(5):
        print(f"{palabra}: {frecuencia} veces")


def grafico_10palabras(path):
    """Gráfico 1: diez palabras más frecuentes."""
    try:
        with open(path, "r", encoding="utf-8") as archivo:
            texto = archivo.read()

    except (OSError, UnicodeError) as error:
        print(f"No se pudo leer el archivo: {error}")
        return

    contador = Counter(obtener_palabras(texto))
    top_10 = contador.most_common(10)

    if not top_10:
        print("No hay palabras para graficar.")
        return

    palabras = [item[0] for item in top_10]
    frecuencias = [item[1] for item in top_10]

    # La palabra más frecuente quedará arriba.
    palabras.reverse()
    frecuencias.reverse()

    plt.figure(figsize=(10, 6))
    plt.barh(palabras, frecuencias, color="skyblue")
    plt.xlabel("Frecuencia")
    plt.ylabel("Palabras")
    plt.title("Gráfico 1 - Diez palabras más frecuentes")
    plt.grid(axis="x", linestyle="--", alpha=0.7)
    plt.tight_layout()
    plt.show()


def grafico_longitud_lineas(path):
    """Gráfico 2: distribución de la longitud de las líneas."""
    try:
        with open(path, "r", encoding="utf-8") as archivo:
            lineas = archivo.readlines()

    except (OSError, UnicodeError) as error:
        print(f"No se pudo leer el archivo: {error}")
        return

    if not lineas:
        print("El archivo está vacío.")
        return

    longitudes = [len(linea.rstrip("\r\n")) for linea in lineas]

    max_len = max(longitudes)
    limite_superior = max(50, ((max_len // 50) + 1) * 50)
    bins = list(range(0, limite_superior + 1, 50))

    plt.figure(figsize=(10, 6))
    plt.hist(
        longitudes,
        bins=bins,
        color="salmon",
        edgecolor="black",
        alpha=0.7
    )
    plt.xlabel("Longitud de la línea (caracteres)")
    plt.ylabel("Cantidad de líneas")
    plt.title("Gráfico 2 - Longitud de las líneas")
    plt.grid(axis="y", linestyle="--", alpha=0.7)
    plt.tight_layout()
    plt.show()


# ==========================================================
# FUNCIONES PARA LEER ARCHIVOS CSV
# ==========================================================

def cargar_csv(path):
    """Lee un CSV y devuelve sus encabezados y todas sus filas."""
    try:
        with open(path, "r", encoding="utf-8-sig", newline="") as archivo:
            lector = csv.reader(archivo, delimiter=",")
            encabezado = next(lector, None)

            if not encabezado:
                print("El archivo CSV está vacío.")
                return None, None

            filas = list(lector)

        return encabezado, filas

    except (OSError, UnicodeError, csv.Error) as error:
        print(f"No se pudo leer el archivo CSV: {error}")
        return None, None


def seleccionar_columna(encabezado, mensaje):
    """Solicita un índice válido de columna."""
    print("\n=== COLUMNAS DISPONIBLES ===")

    for indice, columna in enumerate(encabezado):
        print(f"[{indice}] {columna}")

    try:
        indice = int(input(mensaje))

        if indice < 0 or indice >= len(encabezado):
            print("El índice no corresponde a ninguna columna.")
            return None

        return indice

    except ValueError:
        print("Debe ingresar un número entero.")
        return None


def vista_previa_csv(path):
    """Muestra las primeras diez y las últimas cinco filas."""
    encabezado, filas = cargar_csv(path)

    if encabezado is None:
        return

    print("\n=== ENCABEZADOS ===")
    print(encabezado)

    print("\n=== PRIMERAS DIEZ FILAS ===")

    for i, fila in enumerate(filas[:10], start=1):
        print(f"Fila {i}: {fila}")

    print("\n=== ÚLTIMAS CINCO FILAS ===")

    for i, fila in enumerate(filas[-5:], start=1):
        print(f"Fila {i}: {fila}")


def estadisticas_descriptivas(path):
    """Calcula estadísticas básicas para las columnas numéricas."""
    encabezado, filas = cargar_csv(path)

    if encabezado is None:
        return

    print("\n=== ESTADÍSTICAS DESCRIPTIVAS ===")

    hay_datos = False

    for indice, columna in enumerate(encabezado):
        valores = []

        for fila in filas:
            if indice >= len(fila):
                continue

            try:
                valor = float(fila[indice])

                # Evita incluir valores infinitos o NaN.
                if not (valor == valor and abs(valor) != float("inf")):
                    continue

                valores.append(valor)

            except ValueError:
                continue

        if not valores:
            continue

        hay_datos = True

        print(f"\nColumna: {columna}")
        print(f"Cantidad de datos: {len(valores)}")
        print(f"Mínimo: {min(valores):.2f}")
        print(f"Máximo: {max(valores):.2f}")
        print(f"Media: {statistics.mean(valores):.2f}")
        print(f"Mediana: {statistics.median(valores):.2f}")

        if len(valores) >= 2:
            print(f"Desviación estándar: {statistics.stdev(valores):.2f}")

    if not hay_datos:
        print("No se encontraron columnas con datos numéricos válidos.")


# ==========================================================
# GRÁFICO 3: EVOLUCIÓN TEMPORAL
# ==========================================================

def grafico_evolucion_temporal(path):
    """Representa la evolución de una variable mediante líneas."""
    encabezado, filas = cargar_csv(path)

    if encabezado is None:
        return

    col_x = seleccionar_columna(
        encabezado, "Índice de la columna para el eje X: "
    )

    if col_x is None:
        return

    col_y = seleccionar_columna(
        encabezado, "Índice de la columna numérica para el eje Y: "
    )

    if col_y is None:
        return

    x_vals = []
    y_vals = []

    for fila in filas:
        if max(col_x, col_y) >= len(fila):
            continue

        try:
            valor_y = float(fila[col_y])

            if not (
                valor_y == valor_y
                and abs(valor_y) != float("inf")
            ):
                continue

            x_vals.append(fila[col_x])
            y_vals.append(valor_y)

        except ValueError:
            # Se ignoran las filas que contienen datos no numéricos.
            continue

    if not x_vals:
        print("No hay datos válidos para graficar.")
        return

    plt.figure(figsize=(12, 6))
    plt.plot(x_vals, y_vals, marker="o", linestyle="-")
    plt.xlabel(encabezado[col_x])
    plt.ylabel(encabezado[col_y])
    plt.title(
        f"Evolución temporal: {encabezado[col_y]} "
        f"vs. {encabezado[col_x]}"
    )
    plt.xticks(rotation=45, ha="right")
    plt.grid(True, linestyle="--", alpha=0.7)
    plt.tight_layout()
    plt.show()


# ==========================================================
# GRÁFICO 4: COMPARACIÓN CATEGÓRICA
# ==========================================================

def grafico_comparacion_categorica(path):
    """Representa las categorías más frecuentes en un gráfico circular."""
    encabezado, filas = cargar_csv(path)

    if encabezado is None:
        return

    col_cat = seleccionar_columna(
        encabezado, "Índice de la columna categórica: "
    )

    if col_cat is None:
        return

    categorias = [
        fila[col_cat].strip()
        for fila in filas
        if col_cat < len(fila) and fila[col_cat].strip()
    ]

    if not categorias:
        print("No hay categorías para graficar.")
        return

    contador = Counter(categorias)
    top_categorias = contador.most_common(8)

    labels = [item[0] for item in top_categorias]
    sizes = [item[1] for item in top_categorias]

    # Agrupa las categorías restantes en una sola porción.
    restantes = sum(contador.values()) - sum(sizes)

    if restantes > 0:
        labels.append("Otras")
        sizes.append(restantes)

    plt.figure(figsize=(8, 8))
    plt.pie(
        sizes,
        labels=labels,
        autopct="%1.1f%%",
        startangle=140
    )
    plt.title(f"Comparación categórica: {encabezado[col_cat]}")
    plt.tight_layout()
    plt.show()


# ==========================================================
# GRÁFICO 5: CORRELACIÓN ENTRE VARIABLES
# ==========================================================

def grafico_correlacion_variables(path):
    """Representa dos variables numéricas mediante dispersión."""
    encabezado, filas = cargar_csv(path)

    if encabezado is None:
        return

    col_x = seleccionar_columna(
        encabezado, "Índice de la primera columna numérica (X): "
    )

    if col_x is None:
        return

    col_y = seleccionar_columna(
        encabezado, "Índice de la segunda columna numérica (Y): "
    )

    if col_y is None:
        return

    x_vals = []
    y_vals = []

    for fila in filas:
        if max(col_x, col_y) >= len(fila):
            continue

        try:
            x = float(fila[col_x])
            y = float(fila[col_y])

            if not all(
                valor == valor and abs(valor) != float("inf")
                for valor in (x, y)
            ):
                continue

            x_vals.append(x)
            y_vals.append(y)

        except ValueError:
            continue

    if not x_vals:
        print("No hay suficientes datos numéricos válidos.")
        return

    plt.figure(figsize=(10, 6))
    plt.scatter(x_vals, y_vals, alpha=0.7, edgecolors="black")
    plt.xlabel(encabezado[col_x])
    plt.ylabel(encabezado[col_y])
    plt.title(
        f"Relación entre {encabezado[col_x]} "
        f"y {encabezado[col_y]}"
    )
    plt.grid(True, linestyle="--", alpha=0.7)
    plt.tight_layout()
    plt.show()


# ==========================================================
# MENÚ PRINCIPAL
# ==========================================================

def pedir_archivo():
    """Solicita el nombre completo del archivo y comprueba que exista."""
    print("\nArchivos de la carpeta actual:")
    print(*os.listdir(), sep="\n")

    nombre = input(
        "\nIngrese el nombre del archivo, incluida su extensión: "
    ).strip()

    path = Path(nombre)

    if not path.is_file():
        print("El archivo no existe en la ruta indicada.")
        return None

    return path


def menu_texto():
    """Menú de análisis de archivos de texto."""
    path = pedir_archivo()

    if path is None:
        return

    while True:
        print("\n========== ANÁLISIS DE TEXTO ==========")
        print("1. Resumen del archivo")
        print("2. Palabras frecuentes")
        print("3. Gráfico de palabras y longitud de líneas")
        print("4. Volver al menú principal")

        try:
            opcion = int(input("Seleccione una opción: "))
        except ValueError:
            print("Ingrese un número entero.")
            continue

        if opcion == 1:
            resumen(path)

        elif opcion == 2:
            palabras_frecuentes(path)

        elif opcion == 3:
            grafico_10palabras(path)
            grafico_longitud_lineas(path)

        elif opcion == 4:
            break

        else:
            print("Opción no válida.")


def menu_csv():
    """Menú de análisis de archivos CSV."""
    path = pedir_archivo()

    if path is None:
        return

    while True:
        print("\n========== ANÁLISIS DE CSV ==========")
        print("1. Vista previa de datos")
        print("2. Estadísticas descriptivas")
        print("3. Evolución temporal")
        print("4. Comparación categórica")
        print("5. Correlación de variables")
        print("6. Volver al menú principal")

        try:
            opcion = int(input("Seleccione una opción: "))
        except ValueError:
            print("Ingrese un número entero.")
            continue

        if opcion == 1:
            vista_previa_csv(path)

        elif opcion == 2:
            estadisticas_descriptivas(path)

        elif opcion == 3:
            grafico_evolucion_temporal(path)

        elif opcion == 4:
            grafico_comparacion_categorica(path)

        elif opcion == 5:
            grafico_correlacion_variables(path)

        elif opcion == 6:
            break

        else:
            print("Opción no válida.")


def menu_principal():
    """Controla la navegación general del programa."""
    while True:
        print("\n========== MENÚ PRINCIPAL ==========")
        print("1. Procesamiento de textos")
        print("2. Análisis de datasets CSV")
        print("3. Salir")

        try:
            opcion = int(input("Seleccione una opción: "))
        except ValueError:
            print("Ingrese un número entero.")
            continue

        if opcion == 1:
            menu_texto()

        elif opcion == 2:
            menu_csv()

        elif opcion == 3:
            print("Programa finalizado.")
            break

        else:
            print("Opción no válida.")


# Ejecutar el programa
if __name__ == "__main__":
    menu_principal()
