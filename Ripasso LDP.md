# LE BASI DEL C++
Il C++ è uno strumento che permette al programmatore di esprimere concetti, tecniche e idee.
# 1. Compilatore
- **Traduce** il *codice intermedio* (**.i**) in *codice macchina* (**.o** , solitamente assembly **.s**)
- **Verifica** *sintassi* e *correttezza* del codice sorgente
- **Segnala** *eventuali errori* stampandoli a schermo
Esso può essere eseguito su **riga di comando** tramite il comando `g++ (o gcc) -o exec source.cpp -| library`, oppure tramite un **IDE** (Integrated Development Environment)
## 1.1 Processo di compilazione
1) **Preprocessore**
	- **Traduce** il file sorgente (**.cpp**) in file intermedio (**.i**), leggendo per prima cosa le istruzioni che iniziano con `#` (es. `#include`, `#define`,...)
	- Ignora tutti i commenti
	- Gestisce le librerie necessarie da importare, e più in generale pezzi di codice che vengono inclusi in modo condizionato (es. in base al SO)
2) **Compilatore**
	- Ottimizza e analizza il codice preprocessato e traduce il file intermedio **.i ->** **.o** file oggetto 
3) **Linker**
	- collega gli oggetti e le librerie necessarie, risolve i simboli, produce l’eseguibile.

## 1.2 Librerie

Ogni libreria ha un file header (`.h` o `.hpp` nel caso di templates) nel quale sono dichiarate tutte le entità contenute in essa, come funzioni, classi, variabili globali, ...

Gli header files possono essere inclusi nei seguenti modi:
- Le **librerie di sistema**: fornite dal compilatore e usano `#include <>`
- Le **librerie personalizzate**: create dall'utente e incluse con `#include ""`
Le librerie vengono linkate dal preprocessore e inserite nel linker nello stesso eseguibile.
Questa strategia permette di ottenere file che portano con sè tutte le librerie necessarie.

Se più file includono le stesse librerie -> Rischio!: eccessivo spazio occupato per salvare le stesse librerie. Per questo motivo esistono le librerie dinamiche, che vengono salvate solo 1 volta in memoria e poi linkate dagli eseguibili.
Queste librerie sono chiamate SO (Shared Object) su Ubuntu, DLL (Dynamic Linking Library) su Windows.

# 2. ISTRUZIONI

## 2.1 Operatori

Ogni operatore ha 1,2,3 operandi e un risultato.
Gli operatori lasciano scegliere l'ordine di esecuzione dell'operazione al compilatore, rendendolo oscuro al programmatore.

Alcuni operatori hanno il ruolo di **_side effect_**, ovvero modificano gli operandi su cui operano (ad esempio: ++, --, %=, *=, ...)

 <u><b><i>SBAGLIATO</c></i></u> usare la stessa variabile più volte se su di essa sono applicati operatori side effect (e.g. `int v[i] = i++;`). Questo perché, come detto in precedenza, non è noto al programmatore l'ordine di esecuzione delle operazioni.

## 2.2 Istruzioni (statement)

Un'istruzione è una **parte di codice che specifica un'azione**. Essa termina con un "**;**". #Esempi: dichiarazioni, chiamate di funzione, cicli, ...
(**non contano** come istruzioni le **direttive del preprocessore** (#...))

#### 2.2.1 Dichiarazioni

<b><i>Introduce l'esistenza di un'entità al compilatore, senza allocare memoria o implementare l'entità.</i></b> Ogni nome DEVE essere dichiarato prima di poterlo usare. #Esempio : variabili, funzioni, ...
Una dichiarazione può avere impatto sulla memoria, ma non è certo.

#### 2.2.2 Definizioni
<b><i>Specifica completamente l'oggetto, riservandone memoria e implementandone il comportamento.</i></b> Una definizione è anche una dichiarazione.

> [!example] Esempio
> ```cpp
> double sqrt(double); //Dichiarazione
> double sqrt(double x) { return x*x; } //Dichiarazione e Definizione
> ```

Questa <u>distinzione</u> è <u>utile per sapere l'entità prima dell'implementazione</u>.

# 3. VARIABILI E TIPI

- **TIPO:** definisce una categoria di oggetti, specificandone la rappresentazione in memoria, il range dei valori e l'insieme delle operazioni applicabili
- **OGGETTO:** una regione di memoria allocata con un tipo (vector<'tipo'>, 'tipo' array[]{}, ...)
- **VALORE:** stato dell'oggetto in un certo istante
- **VARIABILE:** fornisce tipo e nome. è un oggetto con un valore

## 3.1 Convenzioni

- Nomi significativi, ma non troppo lunghi
- Evitare acronimi
- Ok nomi brevi, ma solo in ambito ristretto (es `i` per un ciclo for)

Utilizzo di snake_case per variabili e di PascalCase per classi/funzioni
[Google Style Guide](https://google.github.io/styleguide/cppguide.html)

Un **identificatore** in C++ è una sequenza di lettere, cifre e underscore, che non inizia con una cifra, non coincide con una parola chiave, e non utilizzza simboli come ad esempio %,!,",.... Sono case-sensitive (maiuscole/minuscole).

## 3.2 Type safety

Il motivo per cui implementiamo la tipizzazione degli oggetti è per poter ricevere degli avvisi nel caso in cui la conversione che stiamo cercando di fare sia problematica.

#Esempio`int -> float` è considerabile type safe (_widening_).
#Esempio`int -> bool` type safe, ma con possibile perdita di dati! (_narrowing_)

=> <b><i>narrowing:</b></i> *conversione di tipo che restringe il dominio dei valori e può causare perdita di informazione* perché il tipo di destinazione non può rappresentare tutti i valori possibili del tipo sorgente.
Per controllare di non incappare nel `type narrowing`, C++11 ha introdotto l'uso delle {}

> [!example] Esempio di type narrowing
> ```cpp
> double a = 43.2;
> int b {a}; // type narrowing => conversione potenzialmente pericolosa!
> ```

##### <code style="color : gold">OSS</code> NON è type safe utilizzare una variabile non inizializzata:

> [!example] Esempio
> ```cpp
> float somma;
> int totale = somma;
> ```

Non inizializzando rimangono i valori precedenti nella memoria (_questo non vale per variabili globali o statiche che vengono inizializzate a 0_)

## 3.3 Variabili statiche e globali

> [!example] Esempio
> ```cpp
> string program_name = "silly"; //Globale (statica)
> 
> void f() {
>	static int i; //Locale statica
>	int j; //Locale automatica
>}
> ```

1) **GLOBALI**
	- Statiche
	- Inizializzate prima del main
	-  ∃ fino al termine del programma
2) **STATICHE**
	- Inizializzate 1 volta
	- Mantengolo il loro valore al di fuori dello scope
	- Possono essere sia globali che locali
3) **LOCALI AUTOMATICHE**
	- Classiche variabili, ∃ fino alla fine dello scope


#### Lvalue/Rvalue
- **Rvalue**: riferimento al valore contenuto nell'oggetto (&&)
- **Lvalue**: identifica una locazione di memoria (ha un indirizzo)  (&)
- Alcune espressioni sono sia Lvalue che Rvalue (es. variabile - valore)

> [!example] Esempio
> ```cpp
> int a = 0;
> int array[6] {};
>
> for(int i = 0; i < 6; i++){ 
> 	array[i] = i+1; // array[i] -> Lvalue
> 	a += i+1;           // a -> Lvalue; i+1 -> Rvalue
>}
> ```

## 3.4 Costanti

Oggetti/espressioni con valore non mutabile. 

La sintassi è di 2 tipi: 
- `const` = **Immutabilità**. Oggetto non modificabile tramite quell'identificatore. Può essere valutato **sia a compile-time sia a run-time** 
- `constexpr` = **variabile valutabile a compile-time** (e quindi anche **const**), **con vincoli sintattici e semantici più rigidi**. Usato soprattutto per array, template, struct, switch.



# ABSTRACT MECHANISMS

## 4. Scope
Uno scope è una regione di testo dove un identificatore è accessibile e visibile. 5 tipi:
- **GLOBALE**: 
	- fuori da ogni altro scope 
	- rende accessibile all'interno di tutto il file + header file le variabili e/o funzioni definite al suo interno.
	- le **funzioni** sono spesso definite nello scope globale, ma è problematico farlo per le **variabili**:
		- **difficoltà nella protezione**: tutte le funzioni del file possono modificarle
		- rende quasi impossibile il **debugging** per tracciabilità 
- **LOCALE**: 
	- interno ad un blocco di {} di una funzione compresa la lista degli argomenti di quest'ultima
- di **CLASSE**: area interna classe
- di **STANTEMENT**: area interna ai cicli
- **NAMESPACE**: scope con un nome, permettono di organizzare classi, funzioni, ... usati spesso per le librerie

> [!example] Esempio
> ```cpp
> #include <iostream>
> 
> int global_var = 10;
> 
> namespace MyNamespace {
> 	int global_var = 20;
> 	class MyClass {
> 		private:
> 			int member_var = 30;
> 		public:
> 			void print(int local_var) {
> 			int global_var = 40;
> 			std::cout << "local_var: " << local_var << std::endl; //50
> 			std::cout << "MyClass::member_var: " << member_var << std::endl; //30
> 			std::cout << "MyNamespace::global_var: " << MyNamespace::global_var << std::endl; //20
> 			std::cout << "global_var: " << ::global_var << std::endl; //10
> 			std::cout << global_var << std::endl; //40
> 		}
> 	};
> }
> 
> int main() {
> 	int local_var = 50;
> 	MyNamespace::MyClass obj;
> 	obj.print(local_var);
> 	return 0;
> }
> ```

Purtroppo l'uso di namespace può appesantire il programma, bisogna sempre specificare MyNamespace. 
per risolvere si può usare la keyword `using`

> [!example] Esempio con `using`
> ```cpp
> using namespace MyNamespace;
> MyClass obj;
> obj.print(...);
> ```

## 5. Funzioni

prendono dei valori in input e ne ritornano altri in output.

> [!example] Esempio di funzione
> ```cpp
> int sumFunction(int a, int b) { return a+b; }
> ```

il passaggio di valori può essere fatto in diversi modi

### 5.1 Argomenti per valore 

copia dell'oggetto nel nuovo scope => i dati originali non vengono modificati dalla funzione, ma avviene una duplicazione dei dati in memoria => problemi di efficienza

### 5.2 Argomenti per reference

Evito la copia passando una reference al dato. 
Basta aggiungere **&** al tipo. Il resto del codice rimane invariato.

Dal punto di vista fisico, la funzione salva solo una quantità fissa di memoria e può gestire argomenti di grandi dimeninsioni.

> [!attention] Attenzione!
> **sto scrivendo sulla variabile originale!**

per proteggere la variabile passo l'argomento per `const` reference 

> [!example] Esempio const reference
> ```cpp
> print(const int& a);
> ```

### 5.3 Chiamata a funzione 

Gestita da una struttura dati chiamata *function activation record*: essa è uno stack con parametri, eventuali variabili locali e l'indirizzo di ritorno quando la funzione ha completato l'esecuzione.

5 tipi:
- **TEXT SEGMENT**: Codice del programma. Viene copiato dall'eseguibile. Spesso read-only
- **INITIALIZED DATA SEGMENT**: contiene variabili globali e statiche
- **UNINITIALIZED DATA SEGMENT**: iniz. a 0 dal sistema. Contiene variabili globali e statiche non inizializzate
- **STACK**: Activation record
- **HEAP**: regione di memoria gestita in modo dinamico, tramite le keywords `new` e `delete`

## 6. Classi

Sono utili per esprimere idee in codice.
C++ ha un forte focus sui tipi, è possibile usare:
- Tipi built-in (int, double, float, bool, char, ...)
- *UDT (User Defined Types)* (std::string, std::vector, ...)

Una classe comprende:
- Dati di tipo built-in
- Dati di tipo UDT
- Funzioni

All'interno di una classe si può usare il puntatore `this`, una reference all'oggetto
<code style="color : gold">OSS</code> `this` è immutabile, non può essere riassegnato


### 6.1 Interfaccia e Implementazione

L'interfaccia è *public* e definisce l'entità della classe, accessibile agli utenti.

L'**INTERFACCIA**:
- deve essere **COMPLETA**
- deve essere **MINIMALE**, tutto ciò che può essere una helper function deve essere helper function
- deve fornire **COSTRUTTORI**
- la **COPIA/SPOSTAMENTO** deve essere **SUPPORTATA o PROIBITA** (con costruttore di copia/move E con l'assegnamento di copia/move)
- deve utilizzare **TIPI ADEGUATI** per controllare gli argomenti
- deve identificare **FUNZIONI MEMBRO COSTANTI**
- deve liberare tutte le risorse nel **DISTRUTTORE**

L'**implementazione** è *private*, accessibile solo tramite l'interfaccia

(*Di default i membri di una classe sono privati*)

### 6.2 Stato e Invarianti

Lo stato di una classe descrive i valori correnti dei dati dell'oggetto.
l’invariante è una condizione che deve restare vera prima e dopo ogni operazione valida; non è lo stato, è il vincolo sullo stato.

L'uso delle Helper function per lettura e scrittura dati dell'oggetto è necessario per mantenere sempre valido lo stato dell'oggetto.

### 6.3 Definizioni delle funzioni membro

Le funzioni membro devono sempre essere dichiarate dentro la classe e possono essere definite dentro o fuori di essa.
Questo per semplificare l'organizzazione del codice e per non appesantire la definizione della classe con i dettagli implementativi. 

È possibile farlo in-line con funzioni brevi.
Questo tuttavia è solo un consiglio al compilatore che può decidere quale strada prendere.

> [!example] Esempio
> ```cpp
> class Date 
> {
> 	public:
> 		int getMonth() const;
> }
> 
> int Date::getMonth() { return month; }; //implementazione
> ```

### 6.4 Costruttori e Distruttori

Costruttore = funzione membro con lo stesso nome della classe
Servono spesso per inizializzare (costruire) gli oggetti della classe. Per questo c++ usa una sintassi apposta per iniz. i dati con uno stato di partenza valido:
> [!example] Esempio 
> ```cpp
> class Date 
> {
> 	public:
> 		//...
> 	private:
> 		int y, m, d;
> };
> 
> Date::Date(int yy, int mm, int dd) : y{yy}, m{mm}, d{dd} {...}; //(Member) Initializer list
> //Esempio
> Date next {2026, 1, 24};
> ```

<code style="color : gold">OSS 1:</code> Il costruttore di default viene disabilitato non appena si costruisce un altro costruttore. Per questo motivo spesso è necessario reimplementarlo

<code style="color : gold">OSS 2:</code> Posso aggiungere `explicit` nel file `.h`  per bloccare chiamate implicite al costruttore con annessi possibili cast impliciti

Il distruttore è necessario per liberare la memoria nel free store.
Uno di default viene creato dal compilatore se non ne viene scritto uno, ma va sovrascritto in tutti i casi in cui si usa direttamente l'allocazione dinamica della memoria
`~NomeClasse(){};`

### 6.5 Overloading degli operatori

Gli operatori sono degli elementi fondamentali nelle espressioni, come +, =, -, <<, ...
È possibile ridefinirli per i tipi UDT in  due modi:

- Funzione membro (`d.operator++()`)
- Helper function (`operator++(d)`) (*preferibile*)

### 6.6 Costruttore di copia

Chiamato quanto uso l'operatore `=`, per esempio `MyVector v2 = v1`.
Di default fa la copia membro a membro, questo però è un problema con il free store in quanto copia solo il puntatore alla memoria, che rimane invariata.

Il costruttore di copia permette di ridefinire questo comportamento.

```cpp
class vector{
	private:
		int sz;
		int* element;
	public:
		vector(const vector& v) : sz{v.sz} elem{new int[v.sz]} {
			std::copy(v.sz, v.sz + v.elem, elem);
		}
		
		vector& operator=(vector& v) {
			if(this = &v) return *this;
		
			int* p = new int[v.sz]; //Alloco nuova memoria
			std::copy(v.elem, v.elem + v.sz, p); // copio elementi vettore
			delete[] elem; // de-allocco vecchi spazio
			elem = p; // resetto puntatore
			v.sz = sz;
			return *this;
		}
}
```

Va gestito anche l'assegnamento di copia `operator =`, molto simile al costruttore di copia, ma elimina i dati del vecchio vettore.

<code style="color : gold">OSS</code> attenzione all'auto assegnamento `v=v`


## 6.7 Costruttore Move

è utile per quando voglio spostare dei dati senza effettuare una copia, ad esempio quando ritorno un valore da una funzione.

Rubiamo quindi tutti i dati ad una struttura dati invalidandola.

```cpp
class vector{
	private:
		int sz;
		double* element;
	
	public:
		// Costruttore di MOVE
		vector(const vector&& v) : sz{v.sz}, elem{v.element} {
			// Invalida a
			sz = 0;
			elem = nullptr;
		}
		
		vector& operator=(vector&& v2) {
			if(this == &v) return *this;
			
			delete[] elem; //de-allocchi lo spazio
			elem = v2.elem; // riassegno il puntatore
			sz = v2.sz; // riassegno size
			v.elem = nullptr; // invalida il vecchio puntatore di v
			v.sz = 0; // azzera il size di v
			return *this; // ritorna una self reference
		}
};
```

<code style="color : gold">OSS</code> La notazione `&&` è chiamata _rvalue reference_

Il compilatore sceglie quale costruttore usare tra copy e move. 

## 7. Gestione del codice

Quando si lavora su progetti più ampi è importante organizzare il codice in più file.
Esistono 2 tipi di file:

- Header file (`.h` o `.hpp`)
	- contiene solo le dichiarazioni
- File Sorgente (`.cpp`)
	- contiene le definizioni di quanto dichiarato nell'header

### 7.1 Header

Ogni header DEVE includere le **include guards**. 

> [!example] Esempio 
> ```cpp
> #ifndef RATIONAL_H
> #define RATIONAL_H
> 
> // ...
> 
> #endif // RATIONAL_H
> ```

Questo evita che il file venga incluso più volte

## 7.2 Cmake

Esistono vari sistemi di building (make, ninja, ...), spesso dipendenti dalla piattaforma su cui si lavora.

Cmake permette di standardizzare la compilazione. Utile per quando si hanno progetti molto grandi e si deve descrivere un progetto

- Supportato da molti IDE
- Specifica i target di compilazione e le librerie da linkare
- Gestito dal file CMakeLists.txt
- Ha un software che fornisce una GUI

Cmake permette di compilare selettivamente i moduli

```cpp
cmake_minimum_required(VERSION 2.84) # versione minima di CMake
set (CMAKE_CXX_STANDARD 11)
project(Progetto_tutor_autostrade VERSION 1.0.0)

find_package(Qt5 REQUIRED COMPONENTS Core) # Librerie esterne: find_package() le cerca nel sistema e le linka

include_directories(include) # Directory che cerca i file header rispetto alla posizione nel CMakeLists.txt

add_library(${Progetto_tutor_autostrade}) SHARED
										src/Simulator.cpp
										#...

add_executable(simulator src/mainSimulator.cpp
					)

target_link_libraries(main ${PROJECT_NAME})

#installa nel sistema la libreria e gli header
install(TARGETS ${PROJECT_NAME}
			LIBRARY DESTINATION lib
			INCLUDES DESTINATION include
)
```

## Git

Aiuta nella collaborazione di più programmatori allo stesso progetto.
Permette inoltre di tenere un record dei cambiamenti.

Tiene un database delle versioni per ogni computer locale, inoltre si può usare un server per sincronizzare le modifiche, gestire conflitti e eseguire automazioni.

## 8. Enumerazioni

Sono una serie di etichette ad un codice numerico, utili per la leggibilità del codice.

Crea i nomi all'interno di uno scope con lo stesso nome dell'enum.
Quindi si può accedervi con `Verdure::sedano`.

Il valore viene incrementato di 1 per ogni elemento, si può specificare quello iniziale oppure si possono specificare tutti manualmente.

```cpp
enum class Verdure{
	carote = 1, cipolle, sedano, patate, pomodori
};
```

## 9. POLIMORFISMO

**Abilità di associare comportamenti specifici diversi ad un'unica notazione**. 2 tipi:

- Polimorfismo statico a tempo di compilazione: Templates
- Polimorfismo dinamico a tempo di esecuzione: Ereditarietà + funzioni virtuali

## 10. Templates

Anche detti *generics*, è un meccanismo che permette di usare un tipo come un parametro da specificare solo a tempo di compilazione.

```cpp
vector<int>
vector<double>
vector<vector<int>>
```

Il template scompare nel codice compilato, il compilatore duplica la classe vector per ogni tipo che viene usato. (Per questo motivo è necessario sapere i tipi a tempo di compilazione)

### 10.1 Classi

Per creare questi template uso la keyword `template`

```cpp
// Costruttore di move tramite template
template<typename T>
class vector {
	private:
		int sz;
		T* elem;
	public:
		vector() : sz{0} elem{nullptr} {}
};
```

Per poter usare i template è necessario fornire al compilatore, nell'header, la definizione di funzione.
Per mantenere la separazione uso i file `.hpp`

```cpp
// file vector.h

#ifndef VECTOR_H
#define VECTOR_H

template<typename T>
class vector {
	public:
		void resize(int newsize);
};

#include "vector.hpp"

#endif
```

```cpp
// file vector.hpp

#ifndef VECTOR_HPP
#define VECTOR_HPP

template<typename T>
void vector<T>::resize(int newsize) {}
#endif
```

### 10.2 Funzioni 
Si può usare la stessa sintassi sulle funzioni, in questo caso la funzione viene chiama *algoritmo*

```cpp
template <typename T>
T myMin(T x, T y) {
	return (x < y) ? x : y;
}
```

Nei telmplate si può usare anche `int` o altri tipi, unica limitazione è che i loro valori devono essere noti a tempo di compilazione.

## 11. Ereditarietà

### 11.1 Classi astratte

Si definiscono classi astratte le classi il cui costruttore è definito come `protected`, non possono quindi essere utilizzate dall'utente, ma vanno usate le sue sottoclassi.

Classi astratte possono avere anche funzioni `virtual` che sono funzioni che obbligano le sottoclassi all'`override`.
Se si pone la funzione `= 0` diventa virtuale pura, quindi non definita nella classe base, ma solo in quelle derivate

```cpp
// Classe base
class A {
	public:
		virtual void print() = 0;
	protected:
		A() {}
};

// Classe derivata
class B : public A {
	void print() override { std::cout << "B\n"; }
};
```

#### Upcasting
- conversione da classe derivata -> classe base (tipo)
- sicura e implicita
```cpp
Derivata d;
Base* b = &d; //upcasting
```
#### Downcasting
- conversione da classe base -> classe derivata (tipo)
- potenzialmente pericolosa. Va controllata. 
- solo se oggetto a run-time è del tipo derivato
```cpp
Base b = new Derivata;
Derivata* d = dynamic_cast<Derivata*> &b; // Downcasting sicuro
```

### 11.2 Slicing (taglio)

Lo slicing è il fenomeno per cui, assegnando o passando per valore un oggetto derivato a una variabile di tipo base, viene copiata solo la parte base dell’oggetto, con perdita delle informazioni della parte derivata

Per evitare questo problema si utilizzano puntatori o reference al posto di passare oggetti per valore.

-        **Oggetto derivato → Oggetto base** (passaggio per valore) = **slicing**.
-        Solo i membri e funzioni comuni alla classe base vengono copiati.
-        I membri e funzioni specifici della classe derivata vengono **persi**.

<code style="color : gold">OSS</code> un altro caso dove si può ottenere slicing sono i costruttori di copia e `operator=`, per questo vanno disabilitati.

### 11.3 Virtual Pointer e Virtual Table

Ogni classe derivata da una classe base (NON ogni oggetto della classe) ha tra i suoi membri un `vptr` (virtual pointer), un puntatore che punta a `vtbl` (virtual table).
La virtual table associa le funzioni della classe derivata a quelle della classe base qualora a queste funzioni non venga fatto l'override, oppure associa la funzione alla funzione nuova.

# MEMORY MANAGMENT

## 12. Puntatori * e Reference &

### 12.1 Puntatori

Particolare tipo di variabile che contiene un INDIRIZZO di memoria.
Contiene l'informazione riguardo al tipo che viene puntato.

```cpp
double pi = 3,14159;
double* pi_address = &pi;
pi_address // indirizzo di pi
*pi_address // 3,14159
```

Esiste anche un `nullptr` utile per avere un valore non valido

Esiste il tipo `void*` che permette di saltare ogni check, punta a memoria raw.

Vanno fatte considerazioni sulla dimensione del dato puntato.

```cpp
char c = 'a';
int* pi = &c; // ERRORE, qui posso sovrascrivere memoria esterna alla variabile
			  // (char sono 8 bit, int sono 32)
```

### Puntatori vs Reference

Sia il puntatore che la reference possono essere usati per accedere al dato puntato, ma solo il puntatore può spostarsi nella memoria, la reference una volta assegnate rimane costante.

#### Shallow copy

```cpp
int i, j;
int* p1 = &i;
int* p2 = &j;
p1 = p2; // copia l'indirizzo di j. p1 punta a j. 
		 // i rimane invariato.
```

#### Deep copy

```cpp
int i, j;
int &r1 = i;
int &r2 = j;
r1 = r2; // copia valore di j dentro i
```

Inoltre NON si possono creare array di riferimenti.
E spesso i riferimenti vengono detti nomi alternativi di un oggetto o di una variabile.

## 12.3 Cast

Permettono la conversione esplicita tra tipi.

| Nome cast        | Effetto                                                                                        |
| ---------------- | ---------------------------------------------------------------------------------------------- |
| static_cast      | Conversione esplicita tra i tipi, ancora con alcuni check (no conversione da puntatore a dato) |
| reinterpret_cast | Conversione libera, anche da `int` a `double*`                                                 |
| const_cast       | Elimina la constness                                                                           |

# 13. Strutture dati

## 13.1 Array

**Sequenza omogenea di oggetti allocati in spazi contigui di memoria.**
Tutti oggetti dello stesso tipo, indicizzati con `[]` ad accesso casuale.
Dimensione costante e conosciuta a compile-time.

Quando definiamo un array ci viene ritornato un puntatore al primo elemento.
Tramite l'**aritmetica dei puntatori** ci si può spostare sull'array tramite **somma/sottrazione** degli interi.

> [!Example] Esempio
> ```cpp
> int array[5] = {1, 53, 3, 5, 24};
> int* p = array;   // p punta array[0]
> p++;              // p -> array[1]
> int* q = p + 2;   // q punta ad a[3]
> int diff = q - p; // diff == 2
> ```

## 13.2 Stringhe

Anche le stringhe in stile C sono simili a degli array di char.
Terminano con il carattere terminatore `'\0'`

Problema sia array che stringhe: si perde facilmente l'informazione sulla lunghezza della struttura dati.

> [!Example] Esempio
> ```cpp
> char lettere_animali[] = {'c', 'g', 't', '\0'};
> std::string animali[] = {"cane", "gatto", "topo"};
> 
> std::string* ps = animali;         // equivalente a &animali[0]
> char* pc = lettere_animali;
> 
> std::cout << ps << "\n";           //stampa l'indirizzo
> std::cout << *ps << "\n";          //stampa cane
> 
> std::cout << static_cast<void*>(pc + 1) << "\n";       //stampa l'indirizzo di 'g'. Uso static_cast<void*> perchè con char* c'è un overload che lo tratta come C-string 
> std::cout << *(pc + 1) << "\n";      //stampa g
> ```

## 14. Allocazione dinamica della memoria

In alcuni casi è fondamentale poter fornire la dimensione dell'array in esecuzione, dato che non sempre ne conosciamo la dimensione a tempo di compilazione.

Per fare questo si usa l'Heap, free store.
Per accedervi si usa la keyword `new` che ci ritorna solamente un puntatore alla memoria appena riservata.

Anche in questo caso non esiste controllo sui range, con il puntatore creato si può accedere a qualsiasi indirizzo di memoria, con il programma che termina automaticamente solo in alcuni casi (`segmentation error` - runtime - puntatore a zona di memoria non valida (non allocata, già liberata, fuori dai limiti))

Inoltre la memoria non viene inizializzata. Costruendo un array si prendono i dati precedentemente salvati in memoria per i tipi build-in. Per gli UDT viene chiamato il costruttore di default.

### 14.1 Garbage Collector

Non esiste il garbage collector, la memoria deve essere liberata dal programmatore per evitare **memory leak**.

Questo per una questione di maggiore performance e controllo.

È un grosso errore quello di de-allocare la memoria 2 volte, se richiamo `delete` sul puntatore, viene liberata della memoria che non era in nostro possesso.
Per questo motivo è opportuno impostare a `nullptr` il puntatore dopo il `delete`.

```cpp
double* p = new double[n]; //new o new[]

delete[] p; //delete o delete[]
p = nullptr;
```

## 15. Smart Pointer

Garantisce la gestione delle risorse quando voglio ritornare un puntatore, anche senza necessità di usare try/catch.

### 15.1 unique_ptr

- De-alloca la memoria uscendo dallo scope
- Permette i move
- Non permette le copie

```cpp
vector<int>* make_vec() {
	unique_ptr<vector<int>> p { new vector<int> };
	// Le eccezioni qui dentro vengono gestite e non generano problemi.
	return p.release();
}
```
- `make_vec()` alloca con `new`.
- `unique_ptr` protegge temporaneamente.
- `release()` restituisce il puntatore grezzo.
- Chi riceve quel puntatore **deve** chiamare `delete` quando ha finito.

Permette di evitare l'allocazione dinamica del vettore
La copia non è permessa perché si creerebbero 2 proprietari dello stesso puntatore, quindi 2 delete che eliminerebbero memoria non in nostro possesso.

```cpp
void no_good() {
	unique_ptr<X> p { new X };
	unique_ptr<X> q { p }; //ERRORE
} // Qui sia p che q eliminerebbero l'oggetto
```

### 15.2 shared_ptr

Può essere copiato e condiviso.

La memoria viene de-allocata quando l'ultimo shared pointer che detiene la memoria è distrutto.
Quindi tutti gli shared pointer devono conoscersi, occupando della memoria per fare ciò.


# THE STANDARD LIBRARY

## 16. Standard Template Library

È parte della libreria standard e fornisce:

- **Contenitori** (vector, map, list, string, ...)
- **Algoritmi generici** (sort, find, print, ...)

I contenitori hanno un accesso unificato, così da permettere agli algoritmi di operare in modo indipendente dal contenitore.

Utilizzando la STL l'unico scopo è quello di implementare funzioni aggiuntive ai contenitori di default.

### 16.1 Sequenze e Iteratori

Sequenza: lista di oggetti con un inizio e una fine.
Essa viene gestita tramite gli iteratori, che ci permettono di navigare la sequenza.

Iteratore begin: punta al primo elemento
Iteratore end: punta all'elemento successivo all'ultimo
Sequenza: `[begin, end)`

Un iteratore ha:

- Operatori == e !=
- Operatore * per accedere al dato puntato
- Operatore ++ per andare all'elemento successivo
- Operatore -- per andare all'elemento precedente
- Operatore += e -= per sommare e copiare la somma/diff. nella lvalue. 

```cpp
template<typename T>
typename vector<T>&::Iterator high(vector<T>& v) {
	typename vector<T>&::Iterator high = v.begin();
	for(typename vector<T>&::Iterator p = v.begin(); p != v.end(); ++p) {
		if(*high < *p) high = p;
	}
	return high;
}
```

In questo caso può essere particolarmente utile la keyword `auto`.
È una keyword che richiede al compilatore di impostare il tipo della variabile alla compilazione.
Va però utilizzata con cura, può portare ad ambiguità.

```cpp
template<typename T>
vector<T>&::Iterator high(vector<T>& v) {
	auto high = v.begin();
	for(auto p = v.begin(); p != v.end(); ++p) {
		if(*high < *p) high = p;
	}
	return high;
}
```

### 16.2 Contenitori STL

- È una sequenza di elementi $[begin, end)$
- Supporta le operazioni di copia
- Possiede iteratori `iterator` e `const_iterator`
	- che forniscono `*`, `++,` `==`, `!=` (`--`,`[]`, `+`, `-`)
- Fornisce  `insert()` ed `erase()`, `front()` e `back()`, `push_back()` e `pop_back()`, `size()`, ...
- Fornisce operatori di confronto: `==,` `!=,` `<`, `<=`, ...

## 17. Algoritmi STL

La libreria standard offre un gran numero di algoritmi, circa 80, includibili con `#include <algoritm>`
Sono tutti compatibili con i container STL

- Ricerca
- Ordinamento
- Conteggio
- Copia
- Fusione
- ...

### 17.1 If predicato

Permettono di passare delle funzioni come parametro per una funzione
in questo caso ci permettono di generalizzare ancora di più la funzione find.

Il predicato contiene informazioni sul tipo dell'output e i tipi dell'input, ma non informazioni sull'implementazione. ritorna un valore booleano, usato per valutare una proprietà di uno o più oggetti. Per implementare il predicato C++ mette a disposizione le lambda exprection e le function object.

```cpp
// In: iteratore di input
// Pred è un predicato
template<typename T>
In find_if(In first, In last, Pred predicate) {
	while(first != last && !predicate(*first)) ++first;
	return first;
}

bool odd(int x) { return x % 2; }
auto p = find_if(v.begin(), v.end(), odd);
```

Tuttavia non è quasi possibile passare variabili esterne, oltre a quella di default.
(Si potrebbe usare una variabile globale, ma è una pratica sconsigliata)

### 17.2 Function object

Le function object ovviano a questo problema, permettendo di passare altri parametri, e l'implementazione dell'algoritmo non deve neanche cambiare.

```cpp
class Larger_than {
	private:
		int v;
	public:
		//Salvo l'argomento
		Larger_than(int vv) : v{vv} {}
		//Confronto, overloan operator()
		bool operator()(int x) const { return x>v; }
};

void func(list<double>& v, int x) {
	auto p = find_if(v.begin(), v.end(), Larger_than(35));
	if (p != v.end()) { /*valore non trovato*/ }
	
	auto q = find_if(v.begin(), v.end(), Larger_than(x))
	if(q != v.end()) { /*valore trovato*/ }
}
```

spesso si usa anche con una sola variabile per standardizzare

### 17.3 Lambda Expression

Permette di definire un function object e crearne uno immediatamente da usare come argomento della funzione.
Sono *in-line function object*

> [!Example] Esempio
> ```cpp
> sort(vr.begin(), vr.end(), 
> 	[] (const Record& a, const Record& b) -> bool { return a.name < b.name; }
> );
> 
> double epsilon = 102;
> sort(vr.begin(), vr.end(), [epsilon] (const Record& a, const Record& b,) -> bool { return (a.name < epsilon) && (epsilon <= b.name);}
> );
> ```

Le variabili locali possono essere catturate in vari modi:
- `[&epsilon, zeta]`: epsilon per reference e zeta per copia
- `[&]`: tutte le variabili locali per reference
- `[=]`: tutte le variabili per copia
- `[&, epsilon]`: tutte le variabili per reference, tranne epsilon per copia

<code style="color : gold">OSS</code> Le variabili passate per copia sono *read_only*


## 18. Standard Exception

Eccezioni fornite dalla libreria standard, così da renderne più facile la gestione.
In particolare, se le nostre eccezioni derivano tutte da `std::exception`, è sufficiente fare un catch di questa eccezione per catturarle tutte.

image

`std::logic_error` Riporta errori di logica, quindi mancanza di pre-condizioni o violazioni di invarianti
`std::runtime_error` Riporta errori difficilmente prevedibili, dovuti ad esempio ad utenti.

### 18.1 Custom Exceptions

```cpp
class My_error : std::runtime_error {
  public:
	My_error(int x) : error_value{x} {}
	int error_value;
	const char* what() const override { return "My_error"; }
};

try { /* ... */ }
catch (std::exception& e) {
	std::cerr << e.what() << std::endl;
}
```

### 18.2 Gestione delle risorse dopo un'eccezione

L'esempio a noi meglio noto è l'array di `std::vector` che deve essere gestito durante un'eccezione.
Quando si lancia un'eccezione è possibile che la liberazione della memoria non avvenga.

Si può risolvere in 2 modi

-   Con un try catch, dove nel catch inserisco la`delete`
-   Utilizzando oggetti con un distruttore già implementato, che viene chiamato quando si chiude lo stack con il `return`

Ogni funzione può fornire delle garanzie:

-   Basic guarantee
    La funzione o ritorna il risultato oppure lancia un'eccezione, in ogni caso non crea leak
-   Strong guarantee
    Ritorna il risultato oppure i valori passati come parametri ritorneranno ad essere uguali al valore precedente alla chiamata.
    Questo si dice anche "commit or rollback".
-   No throw guarantee
    Garantisce che la funzione non lancia eccezioni.
