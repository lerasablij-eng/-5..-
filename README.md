#include "Header.h"
#include <iostream>
#include <cmath>
#include <string>

#ifndef M_PI
#define M_PI 3.14159265358979323846
#endif

// Конструктор за замовчуванням
Triangle::Triangle() {
    a = b = c = 0;
    A = B = C = 0;
}

// Конструктор для рівностороннього трикутника
Triangle::Triangle(double side) {
    a = b = c = side;
    calculateAngles();
}

// Конструктор для будь-якого трикутника
Triangle::Triangle(double a_val, double b_val, double c_val) {
    a = a_val;
    b = b_val;
    c = c_val;
    calculateAngles();
}

// Приватний метод для обчислення кутів
void Triangle::calculateAngles() {
    if (a > 0 && b > 0 && c > 0) {
        // Обчислення кутів за теоремою косинусів
        A = acos((b * b + c * c - a * a) / (2 * b * c)) * 180 / M_PI;
        B = acos((a * a + c * c - b * b) / (2 * a * c)) * 180 / M_PI;
        C = 180 - A - B;  // сума кутів трикутника = 180°
    }
}

// Встановлення сторін
void Triangle::setSides(double a_val, double b_val, double c_val) {
    a = a_val;
    b = b_val;
    c = c_val;
    calculateAngles();  // переобчислюємо кути при зміні сторін
}

// Виведення сторін
void Triangle::getSides() {
    std::cout << "Сторони: a=" << a << ", b=" << b << ", c=" << c << std::endl;
}

// Виведення кутів
void Triangle::getAngles() {
    std::cout << "Кути: A=" << A << "°, B=" << B << "°, C=" << C << "°" << std::endl;
}

// Обчислення периметра
double Triangle::perimeter() {
    return a + b + c;  // периметр = сума всіх сторін
}

// Обчислення площі (формула Герона)
double Triangle::area() {
    double p = perimeter() / 2;  // півпериметр
    return sqrt(p * (p - a) * (p - b) * (p - c));  // формула Герона
}

// Обчислення висот
void Triangle::heights() {
    double S = area();  // площа трикутника
    std::cout << "Висоти:" << std::endl;
    std::cout << "h_a = " << 2 * S / a << std::endl;  // висота до сторони a
    std::cout << "h_b = " << 2 * S / b << std::endl;  // висота до сторони b
    std::cout << "h_c = " << 2 * S / c << std::endl;  // висота до сторони c
}

// Визначення типу трикутника
std::string Triangle::type() {
    if (a == b && b == c)
        return "Рівносторонній";
    else if (a == b || b == c || a == c)
        return "Рівнобедрений";
    else if (fabs(a * a + b * b - c * c) < 1e-6 ||
        fabs(a * a + c * c - b * b) < 1e-6 ||
        fabs(b * b + c * c - a * a) < 1e-6)
        return "Прямокутний";
    else
        return "Різносторонній";
}

// Виведення всієї інформації
void Triangle::showInfo() {
    getSides();        // виводимо сторони
    getAngles();       // виводимо кути
    std::cout << "Периметр: " << perimeter() << std::endl;
    std::cout << "Площа: " << area() << std::endl;
    heights();         // виводимо висоти
    std::cout << "Тип: " << type() << std::endl;  // виводимо тип трикутника
}

#include "Header.h"
#include <windows.h>

int main() {
    SetConsoleOutputCP(1251);  // встановлення української кодування
    SetConsoleCP(1251);

    std::cout << "Приклади трикутників:\n\n";

    // Трикутник за замовчуванням
    Triangle t1;
    std::cout << "Трикутник за замовчуванням:\n";
    t1.showInfo();

    // Рівносторонній трикутник
    std::cout << "\nРівносторонній трикутник:\n";
    Triangle t2(5);
    t2.showInfo();

    // Прямокутний трикутник
    std::cout << "\nПрямокутний трикутник:\n";
    Triangle t3(3, 4, 5);
    t3.showInfo();

    return 0;
}

#ifndef TRIANGLELIB_H
#define TRIANGLELIB_H

#include <iostream>
#include <cmath>
#include <string>

class Triangle {
private:
    double a, b, c;  // сторони трикутника
    double A, B, C;  // кути трикутника (в градусах)
    void calculateAngles();  // метод для обчислення кутів

public:
    // Конструктори
    Triangle();
    Triangle(double side);
    Triangle(double a, double b, double c);

    // Методи
    void setSides(double a, double b, double c);
    void getSides();
    void getAngles();
    double perimeter();
    double area();
    void heights();
    std::string type();
    void showInfo();
};

#endif
