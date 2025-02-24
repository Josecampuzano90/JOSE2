import os
import json

class Inventario:
    def __init__(self, archivo="inventario.txt"):
        self.archivo = archivo
        self.productos = self.cargar_inventario()

    def cargar_inventario(self):
        """Carga el inventario desde un archivo."""
        if not os.path.exists(self.archivo):
            return {}
        try:
            with open(self.archivo, "r") as file:
                return json.load(file)
        except (FileNotFoundError, json.JSONDecodeError):
            print("Error al leer el archivo. Se creará un nuevo inventario.")
            return {}
        except PermissionError:
            print("No tienes permisos para leer el archivo.")
            return {}

    def guardar_inventario(self):
        """Guarda el inventario en un archivo."""
        try:
            with open(self.archivo, "w") as file:
                json.dump(self.productos, file, indent=4)
            print("Inventario guardado correctamente.")
        except PermissionError:
            print("No tienes permisos para escribir en el archivo.")
        except Exception as e:
            print(f"Error al guardar el inventario: {e}")

    def agregar_producto(self, nombre, cantidad):
        """Agrega un producto al inventario."""
        if nombre in self.productos:
            self.productos[nombre] += cantidad
        else:
            self.productos[nombre] = cantidad
        self.guardar_inventario()
        print(f"Producto '{nombre}' agregado/actualizado con éxito.")

    def eliminar_producto(self, nombre):
        """Elimina un producto del inventario."""
        if nombre in self.productos:
            del self.productos[nombre]
            self.guardar_inventario()
            print(f"Producto '{nombre}' eliminado con éxito.")
        else:
            print(f"El producto '{nombre}' no existe en el inventario.")

    def mostrar_inventario(self):
        """Muestra el inventario actual."""
        if not self.productos:
            print("El inventario está vacío.")
        else:
            print("Inventario Actual:")
            for nombre, cantidad in self.productos.items():
                print(f"- {nombre}: {cantidad}")

if __name__ == "__main__":
    inventario = Inventario()
    while True:
        print("\n1. Agregar producto")
        print("2. Eliminar producto")
        print("3. Mostrar inventario")
        print("4. Salir")
        opcion = input("Seleccione una opción: ")
        
        if opcion == "1":
            nombre = input("Nombre del producto: ")
            try:
                cantidad = int(input("Cantidad: "))
                inventario.agregar_producto(nombre, cantidad)
            except ValueError:
                print("Error: La cantidad debe ser un número entero.")
        elif opcion == "2":
            nombre = input("Nombre del producto a eliminar: ")
            inventario.eliminar_producto(nombre)
        elif opcion == "3":
            inventario.mostrar_inventario()
        elif opcion == "4":
            print("Saliendo del programa...")
            break
        else:
            print("Opción no válida. Intente de nuevo.")
