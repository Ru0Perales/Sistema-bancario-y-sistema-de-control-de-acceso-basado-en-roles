// verificador de predicados
#include <iostream>
#include <vector>
#include <functional>
#include <string>
#include <map>
#include <algorithm>
using namespace std;

// TIPO BASE
using Dominio  = vector<int>;
using Pred1    = function<bool(int)>;
using Pred2    = function<bool(int,int)>;
using Resultado = pair<bool, string>;

// CTFDR UNIVERSAL
bool cuantUniversal(const Dominio& D, Pred1 P) {
    for (int x : D)
        if (!P(x)) return false;
    return true;
}

// CTFDR EXISTENCIAL
bool cuantExistencial(const Dominio& D, Pred1 P) {
    for (int x : D)
        if (P(x)) return true;
    return false;
}

// UNICIDAD EXISTENCIAL
bool cuantUnicidad(const Dominio& D, Pred1 P) {
    int cuenta = 0;
    for (int x : D)
        if (P(x)) ++cuenta;
    return cuenta == 1;
}
// ESTRUCTURA REQUISITO
struct requisito {
    string id;
    string descripcion;
    function<bool()> formula;
};

// VERIFICADOR DE CONJUNTO DE REQUISITOS
void verificarRequisitos(const vector<requisito>& reqs) {
    int ok = 0, fail = 0;
    cout << "\n===* VERIFICACION *==="  << endl;
    cout << string(40, '-') << endl;
    for (const auto& r : reqs) {
        bool resultado = r.formula();
        cout << '[' << r.id << "] " << r.descripcion << endl;
        cout << "    Estado : " << (resultado ? "CONSISTENTE"
                                              : "INCONSISTENTE") << endl;
        resultado ? ++ok : ++fail;
    }
    cout << string(40, '-') << endl;
    cout << "Resultado: " << ok << "/" << (ok+fail)
         << " consistentes" << endl;
}
// MAIN - BANCARIO Y ROLES
int main() {
    // Dominio: IDs de cuentas {1,2,3}
    Dominio cuentas = {1, 2, 3};

    // Datos de ejemplo
    map<int,double> saldo  = {{1, 1500.0}, {2, 300.0}, {3, -50.0}};
    map<int,bool>   bloq   = {{1, false},  {2, true},  {3, false}};
    map<int,int>    titular= {{1, 101},    {2, 102},   {3, 103}};

    // Predicados del dominio
    Pred1 Activa  = [&](int a){ return !bloq.at(a); };
    Pred1 SaldoOK = [&](int a){ return saldo.at(a) >= 0; };
    Pred1 TieneTitular = [&](int a){ return titular.count(a) > 0; };

    // Requisitos formalizados
    vector<requisito> reqs = {
        // RB1: ∀a (Cuenta(a) → ∃! c EsTitular(c,a))
        {"RB1", "Toda cuenta tiene exactamente un titular",
         [&]{ return cuantUniversal(cuentas, TieneTitular); }},

        // RB2: ∀a (Cuenta(a) → saldo(a) ≥ 0) [simplificado]
        {"RB2", "No hay sobregiro en ninguna cuenta",
         [&]{ return cuantUniversal(cuentas, SaldoOK); }},

        // RB3: ∃a Bloqueada(a) → no hay transacciones pendientes
        {"RB3", "Existe al menos una cuenta activa",
         [&]{ return cuantExistencial(cuentas, Activa); }},
    };

    verificarRequisitos(reqs);
    return 0;
}
