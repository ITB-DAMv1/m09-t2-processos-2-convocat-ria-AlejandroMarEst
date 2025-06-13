[GitHub exercicis](https://github.com/AlejandroMarEst/M09T2Processos2aConvocatoria-MartinAlejandro)
# T2. 2ona Convocatòria: Recull d’activitats
## 1. Cerca un mínim de 5 aplicacions o tecnologies que fan servir programació distribuïda. Explica que fan i per què necessiten ser distribuïdes.

- Google Search
  
  Què fa: Permet als usuaris buscar informació a través de milers de milions de pàgines web.

  Per què és distribuïda: Per processar i indexar una quantitat tan gran de dades, Google utilitza milers de servidors repartits arreu del món. La distribució permet respondre ràpidament a les cerques i repartir la càrrega de treball.

- Netflix
  
  Què fa: Ofereix streaming de pel·lícules i sèries a milions d'usuaris a nivell global.

  Per què és distribuïda: Necessita servir contingut de manera ràpida i eficient. Per això utilitza una xarxa distribuïda de servidors (CDN) per apropar els vídeos als usuaris i garantir una reproducció fluida.

- Bitcoin i Blockchain

  Què fa: Permet transaccions digitals segures sense necessitat d’un intermediari central.

  Per què és distribuïda: La cadena de blocs està mantinguda per nodes independents distribuïts per tot el món. Això assegura la confiança, la seguretat i la resistència a fallades o manipulacions.

- Amazon Web Services (AWS)

  Què fa: Proporciona serveis de computació al núvol, com emmagatzematge, servidors i bases de dades.
  
  Per què és distribuïda: Reparteix les seves operacions en diferents centres de dades globals per assegurar alta disponibilitat, escalabilitat i tolerància a errors.

- Sistemes de videojocs multijugador en línia (ex: Fortnite, Minecraft)

  Què fan: Permeten que milers o milions de jugadors interactuïn en temps real des de diferents llocs del món.

  Per què són distribuïts: Els servidors de joc es distribueixen per regions per minimitzar la latència i gestionar grans volums de connexions simultànies.
## 2. Explica de quines formes s’aplica la concurrència en les CPU’s de més d’un nucli actualment. Raona quins són els avantatges d’aplicar cada tipus.
### 1. Paral·lelisme a nivell de fils (multithreading)

**Com funciona:**  
Cada nucli pot executar diversos fils d’execució (threads).

**Exemple:**  
Un nucli pot tenir 2 fils que treballen simultàniament compartint alguns recursos.

**Avantatges:**
- Millora l’ús dels recursos del nucli, aprofitant cicles de CPU que quedarien buits.
- Redueix el temps de resposta de programes que fan moltes tasques (ex: navegadors, jocs, aplicacions interactives).
- Permet l'execució més fluida de múltiples processos lleugers alhora.

### 2. Paral·lelisme a nivell de processos o tasques

**Com funciona:**  
Diversos nuclis físics poden executar processos o fils diferents al mateix temps. És molt útil per programes que poden dividir-se en parts independents.

**Exemple:**  
En un programa de càlcul científic, es divideixen els càlculs en parts i s’assignen a diferents nuclis.

**Avantatges:**
- Augmenta molt el rendiment per a aplicacions intensives (com processament d’imatge, simulacions o compilació de codi).
- Permet executar múltiples programes alhora sense que s’alenteixi el sistema.
- Escalabilitat: afegir més nuclis sol millorar directament la velocitat si el programari està preparat.

## 3. Pel que fa a programació paral·lela i programació asíncrona:
### A. Enumera i explica les diferències entre elles:
**Programació Paral·lela vs Programació Asíncrona**

**Diferències entre programació paral·lela i programació asíncrona**

| Aspecte | Programació Paral·lela | Programació Asíncrona |
|---------|------------------------|------------------------|
| **Objectiu principal** | Executar múltiples tasques simultàniament per reduir el temps total de càlcul. | Gestionar tasques que poden esperar (com E/S o xarxa) sense bloquejar el fil principal. |
| **Execució** | Diversos fils o processos s’executen al mateix temps sobre múltiples nuclis. | Les operacions s'inicien i el control torna al programa; quan l’operació acaba, es continua la seva execució. |
| **Ús típic** | Computació intensiva, càlculs, renderització, simulacions. | E/S intensiva, xarxa, operacions que poden bloquejar, UI. |
| **Hardware** | Sol aprofitar el maquinari multi-nucli o multi-processador. | Pot funcionar bé amb un sol nucli; es basa més en el model d’esdeveniments i callbacks/promeses. |
| **Dificultat** | Complexitat de sincronització entre fils/processos. | Gestió de callbacks, promeses, async/await. |
| **Exemples** | Renderització 3D, càlculs científics, processament massiu de dades. | Aplicacions web, aplicacions d'escriptori amb UI, serveis de xarxa. |

### B. Explica cada pas del cicle de vida d’aquest mètode asíncron:

**Pas 1 — Entrada al mètode**

- Es crida el mètode `GetUrlContentLengthAsync()`.
- Es crea un nou objecte `HttpClient`.

**Pas 2 — Inici de la tasca asíncrona**

- Es crida `GetStringAsync()` sobre el `HttpClient`.
- Aquesta crida inicia la descàrrega de la pàgina web de forma asíncrona.
- `GetStringAsync()` retorna immediatament un `Task<string>` sense bloquejar el fil actual.

**Pas 3 — Flux normal (encara sense await)**

- Encara no hi ha cap `await`, així que el flux de codi continua sense esperar la descàrrega.
- El mètode continua l'execució normalment.

**Pas 4 — Execució de treball independent**

- Es crida el mètode `DoIndependentWork()`.

**Pas 5 — Fi del treball independent**

- `DoIndependentWork()` finalitza.
- El flux arriba a la línia on es fa `await getStringTask`.

**Pas 6 — Suspensió amb await**

- Si el `Task` (`getStringTask`) encara no ha finalitzat:
  - El mètode `GetUrlContentLengthAsync()` es suspèn.
  - El control retorna al codi que va cridar `GetUrlContentLengthAsync()`.
  - Quan la descàrrega asíncrona acabi, el sistema reprendrà el mètode on estava suspès.

**Pas 7 — Reanudació de l'execució**

- Quan `getStringTask` completa la descàrrega:
  - L'execució del mètode suspès es reprèn després de l'`await`.
  - El resultat de la descàrrega s’assigna a `contents`.

**Pas 8 — Finalització**

- Es calcula `contents.Length` per obtenir la longitud de la resposta.
- El valor es retorna com a resultat final de `GetUrlContentLengthAsync()`.

### C. De la següent llista d’aplicacions digues quines faries servir únicament programació paral·lela i quina programació asíncrona. Raona la teva resposta:

- **Processament de lots d’imatges: aplicació que ha de processar una quantitat X d’imatges el més eficientment possible.**  
  
  Programació paral·lela  
  Per què: Cada imatge es pot processar de forma independent, per tant es pot dividir la feina entre múltiples nuclis o fils i fer-ho simultàniament.  
  Objectiu: Maximitzar el rendiment i reduir el temps total de processament.

- **Aplicació d’escriptori per a usuaris: aplicació amb una UI que ha de ser fluida.**  
  
  Programació asíncrona  
  Per què: La IU ha de seguir responent mentre es fan operacions com carregar arxius o consultar dades. La programació asíncrona permet que aquestes operacions s’executin en segon pla sense bloquejar la interfície.  
  Objectiu: Millorar l’experiència d’usuari evitant congelaments.

- **Aplicació de missatgeria en temps real.**  
  
  Programació asíncrona  
  Per què: Les operacions de xarxa (com enviar i rebre missatges) poden trigar i no han de bloquejar la resta de la interfície. Amb programació asíncrona es pot mantenir la resposta del sistema en temps real.  
  Objectiu: Reactivitat i fluïdesa mentre s’esperen respostes del servidor.

- **Renderització de gràfics en 3D: es renderitza blocs petits de la imatge i s’ajunten en una sola.**  
  
  Programació paral·lela  
  Per què: Es poden renderitzar múltiples blocs/píxels simultàniament, repartint la feina entre diversos fils o nuclis, ja que cada part del frame pot calcular-se de manera independent.  
  Objectiu: Millorar el rendiment i la velocitat de renderització.
  
## 4. Implementa un programa que llista tots els programes que s’estan executant en el teu Sistema Operatiu, mostra els fils que fa servir amb els seus ID’s. Guarda-ho en un arxiu de text (txt) i seguidament guarda el recompte total de processos oberts i del total de fils.

[Exercici 4](https://github.com/AlejandroMarEst/M09T2Processos2aConvocatoria-MartinAlejandro/tree/main/Exercici4)

## 5. Una bateria auxiliar d’una capacitat d’energia M mil·liamperes, pot carregar un dispositiu al mateix temps. El dia del tall de llum es va fer servir fins a quedar-se sense buida. Implementa un programa que segons els dispositius que ha de carregar ens digui el temps que triga a buidar-se i quantes càrregues ha realitzat. Donem per fet que el temps de càrrega és immediat. 

[Exercici 5](https://github.com/AlejandroMarEst/M09T2Processos2aConvocatoria-MartinAlejandro/tree/main/Exercici5)

## 6. El fill del pastisser ha entrat a treballar al negoci. Ha vist com funciona la producció i veu que té molt marge de millora en el que fa el temps de produir un sol pastís. El seu pare no ho veu clar, ell sempre treballa de forma seqüencial, fins que no acaba una tasca no comença l’altre. El fill, en canvi, creu que hi ha passos de la recepta que es poden fer en paral·lel.  
Realitza dues simulacions i mesura el temps de producció de cada una d’elles seguint els següent quadre de tasques:

[Exercici 5](https://github.com/AlejandroMarEst/M09T2Processos2aConvocatoria-MartinAlejandro/tree/main/Exercici6)

## 7. Analitza el següent codi, explica que pretén fer i determina si té errors o punts de millora per un ús correcte dels Threads. 
- Enumera i explica els erros i les millores trobades. 
- Reescriu el codi de forma correcte

``` C#
using System;

namespace SensorRace
{
    class Program
    {
        public static int[] Readings;

        public static int GlobalMax = int.MinValue;
        public static int GlobalMin = int.MaxValue;

        static void Main(string[] args)
        {
            Console.Write("Introdueix el nombre de sensors: ");
            int sensors = int.Parse(Console.ReadLine());
            StopWatch sW = new Stopwacth(); (Stopwatch)

            Readings = new int[sensors];
            Thread[] threads = new Thread[sensors];

            Random rng = new Random();

            for (int i = 0; i < sensors; i++){
                int id = i;

                threads[i] = new Thread(() =>
                {
                    for (int j = 0; j < 100000; j++){
                        int value = rng.Next(-20, 51);
                        sW.StarNew(); (No s’inicia on s’hauria d’iniciar)

                        Readings[id] = value;

                        if (value > GlobalMax)
                            GlobalMax = value;

                        if (value < GlobalMin)
                            GlobalMin = value;
                    }
                });

                threads[i].Start();
            }
            Console.WriteLine($"Final – Max: {GlobalMax}, Min: {GlobalMin}");
            Console.WriteLine($"Total Process time: {sW.Restart()}");
        }
    }
}
```
