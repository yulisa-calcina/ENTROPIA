#include <iostream>
#include <fstream>
#include <string>
#include <unordered_map>
#include <cmath>

double calcularEntropiaShannon(const std::string& texto) {
    std::unordered_map<std::string, int> frecuenciaPalabras;
    int totalPalabras = 0;

    std::string palabraActual = "";
    for (char c : texto) {
        if (std::isalpha(c)) {
            palabraActual += std::tolower(c);
        } else if (!palabraActual.empty()) {
            frecuenciaPalabras[palabraActual]++;
            totalPalabras++;
            palabraActual = "";
        }
    }

    double entropia = 0.0;
    for (const auto& par : frecuenciaPalabras) {
        double probabilidad = static_cast<double>(par.second) / totalPalabras;
        entropia -= probabilidad * std::log2(probabilidad);
    }

    return entropia;
}

int main() {
    std::ifstream archivo("nnnnn.txt");
    if (archivo.is_open()) {
        std::string texto((std::istreambuf_iterator<char>(archivo)), std::istreambuf_iterator<char>());
        archivo.close();

        double entropia = calcularEntropiaShannon(texto);
        std::cout << "Entropia por palabra: " << entropia << std::endl;
    } else {
        std::cerr << "Error al abrir el archivo." << std::endl;
    }

    return 0;
}
