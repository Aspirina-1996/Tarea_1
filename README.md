# Tarea_1

# Listapara almacenar estudiantes
estudiantes = []

def agregar_estudiante(estudiantes_list, id, nombre, edad, notas): #pide al usuario los datos del estudiante que desea agregar 5
    """
    Agrega un estudiante a la lista.
    Parámetros:
        estudiantes_list (list): lista de estudiantes
        id (int): ID del estudiante
        nombre (str): nombre del estudiante
        edad (int): edad del estudiante
        notas (tuple): tupla con 3 notas (float)
    Retorna:
        None
    """
    estudiante = {
        'id': id,
        'nombre': nombre,
        'edad': edad,
        'notas': notas
    } #LISTA DE DATOS QUE DEBE TENER CADA ESTUDIANTE 
    estudiantes_list.append(estudiante)

def mostrar_estudiantes(estudiantes_list):#para los estudiantes ya ingresados 
    """
    Muestra la lista de estudiantes con sus datos.
    """
    if not estudiantes_list:
        print("No hay estudiantes registrados.")
        return
    for est in estudiantes_list:
        print(f"ID: {est['id']}, Nombre: {est['nombre']}, Edad: {est['edad']}, Notas: {est['notas']}")

def buscar_estudiante_por_id(estudiantes_list, id):#buscar el usuario
    """
    Busca un estudiante por su ID.
    Retorna el estudiante o None si no se encuentra.
    """
    for est in estudiantes_list:
        if est['id'] == id:
            return est
    return None

def calcular_promedio(notas):
    """
    Calcula el promedio de una tupla de notas.
    """
    try:
        promedio = sum(notas) / len(notas)
        return promedio
    except ZeroDivisionError:
        return 0

def listar_aprobados_reprobados(estudiantes_list, nota_aprobacion=60):
    """
    Lista estudiantes aprobados y reprobados según promedio.
    Retorna dos listas: aprobados, reprobados
    """
    aprobados = []
    reprobados = []
    for est in estudiantes_list:
        promedio = calcular_promedio(est['notas'])
        if promedio >= nota_aprobacion:
            aprobados.append(est)
        else:
            reprobados.append(est)
    return aprobados, reprobados

def ordenar_por_nota(estudiantes_list):
    """
    Ordena la lista de estudiantes por promedio de notas usando burbuja.
    """
    n = len(estudiantes_list)
    for i in range(n):
        for j in range(0, n - i - 1):
            if calcular_promedio(estudiantes_list[j]['notas']) > calcular_promedio(estudiantes_list[j + 1]['notas']):
                estudiantes_list[j], estudiantes_list[j + 1] = estudiantes_list[j + 1], estudiantes_list[j]

def exportar_datos(estudiantes_list, filename="estudiantes.csv"):
    """
    Exporta los datos de estudiantes a un archivo CSV.
    """
    try:
        with open(filename, mode='w', newline='', encoding='utf-8') as file:
            writer = csv.writer(file)
            writer.writerow(['ID', 'Nombre', 'Edad', 'Nota1', 'Nota2', 'Nota3'])
            for est in estudiantes_list:
                writer.writerow([est['id'], est['nombre'], est['edad'], *est['notas']])
        print(f"Datos exportados exitosamente a {filename}")
    except Exception as e:
        print(f"Error al exportar datos: {e}")

def es_impar_bitwise(nota):
    """
    Ejemplo de operación bit a bit para determinar si una nota es impar.
    Retorna True si impar, False si par.
    """
    return (int(nota) & 1) == 1

def validar_entero(mensaje):
    """
    Valida que el usuario ingrese un entero.
    """
    while True:
        try:
            valor = int(input(mensaje))
            return valor
        except ValueError:
            print("Entrada inválida. Por favor ingrese un número de ID.")

def validar_float(mensaje):
    """
    Valida que el usuario ingrese un número flotante.
    """
    while True:
        try:
            valor = float(input(mensaje))
            return valor
        except ValueError:
            print("Entrada inválida. Por favor ingrese un número válido.")

def menu():
    """
    Menú principal del sistema.
    """
    while True:
        print("\nSistema de Gestión de Estudiantes")
        print("1. Agregar estudiante")
        print("2. Mostrar estudiantes")
        print("3. Buscar estudiante por ID")
        print("4. Calcular promedio de notas")
        print("5. Listar aprobados y reprobados")
        print("6. Ordenar estudiantes por nota (burbuja)")
        print("7. Exportar datos")
        print("8. Salir")

        opcion = validar_entero("Seleccione una opción: ")

        if opcion == 1:
            id = validar_entero("Ingrese ID del estudiante: ")
            nombre = input("Ingrese nombre del estudiante: ")
            edad = validar_entero("Ingrese edad del estudiante: ")
            notas = ()
            for i in range(1, 4):
                nota = validar_float(f"Ingrese nota {i}: ")
                notas += (nota,)
            agregar_estudiante(estudiantes, id, nombre, edad, notas)
            print("Estudiante agregado exitosamente.")

        elif opcion == 2:
            mostrar_estudiantes(estudiantes)

        elif opcion == 3:
            id = validar_entero("Ingrese ID del estudiante a buscar: ")
            est = buscar_estudiante_por_id(estudiantes, id)
            if est:
                print(f"Estudiante encontrado: ID: {est['id']}, Nombre: {est['nombre']}, Edad: {est['edad']}, Notas: {est['notas']}")
                # Ejemplo de uso de operaciones lógicas and, or, not
                if est['edad'] > 18 and calcular_promedio(est['notas']) >= 60:
                    print("El estudiante es mayor de edad y está aprobado.")
                else:
                    print("El estudiante no cumple con ambas condiciones (mayor de edad y aprobado).")
                # Ejemplo bit a bit para la primera nota
                if es_impar_bitwise(est['notas'][0]):
                    print("La primera nota es impar (verificado con operación bit a bit).")
                else:
                    print("La primera nota es par (verificado con operación bit a bit).")
            else:
                print("Estudiante no encontrado.")

        elif opcion == 4:
            id = validar_entero("Ingrese ID del estudiante para calcular promedio: ")
            est = buscar_estudiante_por_id(estudiantes, id)
            if est:
                promedio = calcular_promedio(est['notas'])
                print(f"El promedio de notas del estudiante {est['nombre']} es: {promedio:.2f}")
            else:
                print("Estudiante no encontrado.")

        elif opcion == 5:
            aprobados, reprobados = listar_aprobados_reprobados(estudiantes)
            print("\nEstudiantes Aprobados:")
            mostrar_estudiantes(aprobados)
            print("\nEstudiantes Reprobados:")
            mostrar_estudiantes(reprobados)

        elif opcion == 6:
            ordenar_por_nota(estudiantes)
            print("Estudiantes ordenados por promedio de notas.")

        elif opcion == 7:
            exportar_datos(estudiantes)

        elif opcion == 8:
            print("Saliendo del sistema...")
            break

        else:
            print("Opción inválida. Por favor seleccione una opción válida.")

if __name__ == "__main__":
    menu()
