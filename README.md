- 👋 Hi, I’m @fabitofg
- 👀 I’m interested in ...Learn Programming
- 🌱 I’m currently learning ...in the C++ code
- 💞️ I’m looking to collaborate on ...Any student
- 📫 How to reach me ...by mail
- 😄 Pronouns: ...fabito
- ⚡ Fun fact: ...I'm new to this

<!---
fabitofg/fabitofg is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>  // Para transformar a minúsculas

using namespace std;

class Registro {
private:
    int dia;
    string tipo;
    int cantidad;

public:
    Registro(int dia, const string& tipo, int cantidad) : dia(dia), tipo(tipo), cantidad(cantidad) {}

    int getDia() const {
        return dia;
    }

    string getTipo() const {
        return tipo;
    }

    int getCantidad() const {
        return cantidad;
    }
};

class GestorRegistros {
private:
    vector<Registro> registros;

public:
    // Agregar nuevo registro al vector
    void agregarRegistro(int dia, const string& tipo, int cantidad) {
        registros.push_back(Registro(dia, tipo, cantidad));
    }

    // Mostrar los registros almacenados
    void mostrarRegistros() const {
        if (registros.empty()) {
            cout << "No hay registros para mostrar.\n";
            return;
        }

        for (const Registro& registro : registros) {
            cout << "Día: " << registro.getDia() << ", Tipo: " << registro.getTipo() << ", Cantidad: " << registro.getCantidad() << endl;
        }
    }

    // Contar el total de un tipo de registro en todo el mes
    int contarTotalMes(const string& tipo) const {
        int total = 0;
        for (const Registro& registro : registros) {
            if (registro.getTipo() == tipo) {
                total += registro.getCantidad();
            }
        }
        return total;
    }

    // Calcular los ingresos totales del mes
    double calcularIngresosMes() const {
        const double precioTarjeta = 0.35;
        const double precioOficio = 0.25;
        int totalTarjetas = contarTotalMes("tarjeta");
        int totalOficios = contarTotalMes("oficio");
        return (totalTarjetas * precioTarjeta) + (totalOficios * precioOficio);
    }
};

// Función para convertir string a minúsculas
string toLowerCase(string str) {
    transform(str.begin(), str.end(), str.begin(), ::tolower);
    return str;
}

int main() {
    GestorRegistros gestor;
    string tipo;
    int cantidad, dia;
    char continuar;

    do {
        cout << "Ingrese el día del mes: ";
        cin >> dia;
        cout << "Ingrese el tipo (tarjeta/oficio): ";
        cin >> tipo;
        tipo = toLowerCase(tipo);  // Convertir la entrada a minúsculas para evitar errores

        // Verificar que el tipo sea válido
        if (tipo != "tarjeta" && tipo != "oficio") {
            cout << "Tipo incorrecto. Debe ser 'tarjeta' o 'oficio'.\n";
            continue;  // Volver a solicitar los datos
        }

        cout << "Ingrese la cantidad entregada: ";
        cin >> cantidad;

        // Validar la cantidad
        if (cantidad <= 0) {
            cout << "La cantidad debe ser un número positivo.\n";
            continue;  // Volver a solicitar los datos
        }

        gestor.agregarRegistro(dia, tipo, cantidad);  // Agregar el registro

        cout << "¿Desea agregar otro registro? (s/n): ";
        cin >> continuar;
    } while (continuar == 's' || continuar == 'S');

    // Mostrar los registros
    cout << "\nRegistros diarios:\n";
    gestor.mostrarRegistros();

    // Mostrar los totales y los ingresos
    cout << "\nTotal de tarjetas entregadas en el mes: " << gestor.contarTotalMes("tarjeta") << endl;
    cout << "Total de oficios entregados en el mes: " << gestor.contarTotalMes("oficio") << endl;
    cout << "Ingresos totales del mes: $" << gestor.calcularIngresosMes() << endl;

    return 0;
}
