# PrograB-sica
Progra Básica
class Usuario:
    def __init__(self, nombre, correo, pin):
        self.nombre = nombre
        self.correo = correo
        self.pin = pin
        self.casas = []

    def agregar_casa(self, casa):
        self.casas.append(casa)

    def __str__(self):
        return f"{self.nombre} ({self.correo})"


class Casa:
    def __init__(self, nombre):
        self.nombre = nombre
        self.habitaciones = []

    def agregar_habitacion(self, habitacion):
        self.habitaciones.append(habitacion)

    def __str__(self):
        return f"{self.nombre}"


class Habitacion:
    def __init__(self, nombre):
        self.nombre = nombre
        self.dispositivos = []

    def agregar_dispositivo(self, dispositivo):
        self.dispositivos.append(dispositivo)

    def __str__(self):
        return f"{self.nombre}"


class Dispositivo:
    def __init__(self, nombre, tipo):
        self.nombre = nombre
        self.estado = "Apagado"
        self.programacion = None
        self.tipo = tipo

    def establecer_estado(self, estado):
        self.estado = estado

    def establecer_programacion(self, programacion):
        self.programacion = programacion

    def __str__(self):
        return f"{self.nombre} ({self.estado})"


def registrar_usuario():
    nombre = input("Ingresa tu nombre: ")
    correo = input("Ingresa tu correo electrónico: ")
    pin = input("Ingresa un PIN: ")

    return Usuario(nombre, correo, pin)


def iniciar_sesion(usuarios):
    correo = input("Ingresa tu correo electrónico: ")
    pin = input("Ingresa tu PIN: ")

    for usuario in usuarios:
        if usuario.correo == correo and usuario.pin == pin:
            return usuario

    return None


def crear_casa(usuario):
    nombre_casa = input("Ingresa el nombre de la casa: ")
    casa = Casa(nombre_casa)
    usuario.agregar_casa(casa)
    return casa


def seleccionar_habitacion(casa):
    print("Habitaciones disponibles:")
    for i, habitacion in enumerate(casa.habitaciones, start=1):
        print(f"{i}. {habitacion}")

    eleccion_habitacion = int(input("Ingresa el número de la habitación: ")) - 1

    if 0 <= eleccion_habitacion < len(casa.habitaciones):
        return casa.habitaciones[eleccion_habitacion]
    else:
        return None


def seleccionar_dispositivo(habitacion):
    print("Dispositivos disponibles:")
    for i, dispositivo in enumerate(habitacion.dispositivos, start=1):
        print(f"{i}. {dispositivo}")

    eleccion_dispositivo = int(input("Ingresa el número del dispositivo: ")) - 1

    if 0 <= eleccion_dispositivo < len(habitacion.dispositivos):
        return habitacion.dispositivos[eleccion_dispositivo]
    else:
        return None


def main():
    usuarios = []
    usuario_actual = None

    while True:
        print("\n===== Aplicación Smart Home =====")
        if usuario_actual:
            print(f"Bienvenido, {usuario_actual.nombre}!")
            print("1. Ver Casas")
            print("2. Registrar Nueva Casa")
            print("3. Controlar Dispositivos")
            print("4. Cerrar Sesión")
            opcion = input("Ingresa tu opción (1/2/3/4): ")

            if opcion == "1":
                if not usuario_actual.casas:
                    print("No tienes casas registradas.")
                else:
                    print("===== Tus Casas =====")
                    for i, casa in enumerate(usuario_actual.casas, start=1):
                        print(f"{i}. {casa}")

            elif opcion == "2":
                nueva_casa = crear_casa(usuario_actual)
                print(f"Casa '{nueva_casa.nombre}' registrada exitosamente!")

            elif opcion == "3":
                if not usuario_actual.casas:
                    print("No tienes casas registradas.")
                else:
                    print("===== Tus Casas =====")
                    for i, casa in enumerate(usuario_actual.casas, start=1):
                        print(f"{i}. {casa}")

                    eleccion_casa = int(input("Ingresa el número de la casa: ")) - 1

                    if 0 <= eleccion_casa < len(usuario_actual.casas):
                        casa_seleccionada = usuario_actual.casas[eleccion_casa]
                        habitacion_seleccionada = seleccionar_habitacion(casa_seleccionada)

                        if habitacion_seleccionada:
                            dispositivo_seleccionado = seleccionar_dispositivo(habitacion_seleccionada)

                            if dispositivo_seleccionado:
                                print(f"Controlando dispositivo '{dispositivo_seleccionado.nombre}' en la habitación '{habitacion_seleccionada.nombre}'")
                                print("1. Encender")
                                print("2. Apagar")
                                print("3. Establecer Programación")
                                opcion_dispositivo = input("Ingresa tu opción (1/2/3): ")

                                if opcion_dispositivo == "1":
                                    dispositivo_seleccionado.establecer_estado("Encendido")
                                    print(f"Dispositivo '{dispositivo_seleccionado.nombre}' encendido.")

                                elif opcion_dispositivo == "2":
                                    dispositivo_seleccionado.establecer_estado("Apagado")
                                    print(f"Dispositivo '{dispositivo_seleccionado.nombre}' apagado.")

                                elif opcion_dispositivo == "3":
                                    programacion = input("Ingresa la programación (ej. 'Lunes a Viernes, 8:00-18:00'): ")
                                    dispositivo_seleccionado.establecer_programacion(programacion)
                                    print(f"Programación del dispositivo '{dispositivo_seleccionado.nombre}' establecida: {programacion}")

                                else:
                                    print("Opción inválida. Por favor, intenta nuevamente.")
                            else:
                                print("Dispositivo inválido. Por favor, intenta nuevamente.")
                        else:
                            print("Habitación inválida. Por favor, intenta nuevamente.")
                    else:
                        print("Casa inválida. Por favor, intenta nuevamente.")

            elif opcion == "4":
                usuario_actual = None
                print("Sesión cerrada exitosamente.")
            else:
                print("Opción inválida. Por favor, intenta nuevamente.")

        else:
            print("1. Registrarse")
            print("2. Iniciar Sesión")
            print("3. Salir")
            opcion = input("Ingresa tu opción (1/2/3): ")

            if opcion == "1":
                nuevo_usuario = registrar_usuario()
                usuarios.append(nuevo_usuario)
                print(f"Usuario '{nuevo_usuario.nombre}' registrado exitosamente!")

            elif opcion == "2":
                usuario_actual = iniciar_sesion(usuarios)
                if usuario_actual:
                    print(f"Bienvenido de nuevo, {usuario_actual.nombre}!")
                else:
                    print("Correo o PIN inválido. Por favor, intenta nuevamente.")

            elif opcion == "3":
                print("¡Hasta luego!")
                break

            else:
                print("Opción inválida. Por favor, intenta nuevamente.")


if __name__ == "__main__":
    main()
