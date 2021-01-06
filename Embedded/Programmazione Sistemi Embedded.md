#  Programmazione dei sistemi embedded

**Anno accademico** *2019*/*2020*

**Professore** <u>Cario</u>

**Esercitatore** <u>Paolo Folino</u>


[TOC]

___

## Introduzione al corso (<u>da lezione 1 a lezione 2</u>)

Sono sistemi embedded tutti quei sistemi <u>software **e** hardware</u> costruiti e programmati per un unico scopo. Perdono quindi, rispetto ai classici sistemi "*general purpose*", la possibilità di svolgere dei compiti generici o di poter comunque essere riprogrammati per svolgere compiti diversi da quello per cui sono concepiti

Normalmente, i sistemi generici, sfruttano l'uso di una **CPU** che preleva le istruzioni (fase di fetch), le decodifica (fase di decode) e quindi attraverso memoria, registri e A.L.U. le attua. Le CPU in genere sono pensate per *la programmazione concorrente e il multi-threading*

I <u>sistemi integrati</u> sfruttano invece i **microcontroller**, o **MCU**, tipicamente con un solo core. Le MCU hanno una *<u>memoria del programma</u>* (che contiene il flusso delle istruzioni da eseguire ciclicamente), delle *<u>interfacce</u>* di comunicazioni *<u>digitali e analogiche</u>* e un sistema di <u>*controllo del tempo*</u> (*timer*). Il tempo per i sistemi integrati è un fattore molto importante, poiché in genere questi realizzano un **sistema a tempo reale** ( o <u>real-time</u> ). 

### concorrenza 

La concorrenza nei sistemi real-time è un concetto complicato, deve essere soggetta a determinati comportamenti:

- Rispondere a sollecitazioni ed eventi esterni ( concorrenza reattiva )
- Rispettare lassi di tempo in maniera assoluta ( deadline )
- Gestire più attività in contemporanea ( multi-threading )

Per garantire il massimo dell'efficienza in questo contesto è quindi necessario che le MCU si interfaccino con una serie di componenti che devono, da sole, garantire tutte le condizioni:

- Timer di controllo
- Interfacce analogiche
- comunicazioni con altri dispositivi
- interfacce di intercettazione degli eventi
- driver 

### caratteristiche dei sistemi embedded

I sistemi integrati devono garantire una certa tolleranza nei guasti:

- correzione degli errori
- diagnostica del problema



Inoltre devono essere:

- poco costosi
- dalle dimensioni ridotte
- poco dispendiosi energicamente parlando
- supportare variazioni ambientali difficoltose, quali basse e alte temperatura



### In risposta alle esigenze

In risposta alle esigenze di cui sopra, nei sistemi embedded sono quindi considerate le seguenti opzioni:

- I microcontroller sono utilizzati a dispetto dei microprocessori
  - riducendo quindi costi, consumi e strutturando nativamente una gestione ad eventi concorrenti real time
- Vengono utilizzati linguaggi di programmazione a basso livello
  - come l'immortale C, a volte anche assembly, consentendo quindi di avere pieno controllo sulla struttura hardware, ottimizzando, alleggerendo e velocizzando il codice il più possibile in relazione alle componenti usate
- Utilizzando sistemi operativi specifici o ancora meglio nessun sistema operativo
  - qualunque struttura superflua non utilizzata potrebbe avere un maggiore dispendio di energia o comunque introdurre ritardi e complessità nella nostra applicazione.

### Scheduling

Essenzialmente uno schema di base dello scheduling è già introdotto dalla gestione delle interruzioni ad eventi: *ogni qualvolta un determinato evento è catturato dai sensori, viene gestito con una particolare sequenza di istruzioni ( subroutine )*

Ovviamente, casi più complessi vanno gestiti con uno scheduler apposito, in modo da condividere le risorse fisiche nel modo più veloce possibile e non andare in deadline con i processi: **non ci si può basare solo sul supporto hardware**, <u>i systemi embedded devono contare sulla collaborazione tra risorse, scheduling e bontà del software</u>

#### definizioni

- T~rilascio~= Tempo di richiesta di avvio del processo, che sia esso poi richiesto da un evento o da un task
- T~latenza~= Tempo che intercorre tra la richiesta di avvio del processo a quando realmente viene avviato
- T~risposta~= Tempo che intercorre tra la richiesta di avvio del processo a quando viene terminato
- T~Task~ = Tempo di esecuzione necessario del task
- T~ISR~= da '**I**nterrupt **S**ervice **R**outine' o <u>gestore degli interrupt</u>, il tempo in cui il gestore risponde 



> <u>**Approfondimenti**</u>
>
> Il gestore degli interrupt (**ISR**) è un software che l'hardware invoca in seguito ad un interrupt. ISR esamina un interrupt e determina come questo deve essere gestito, salva il contesto atuale e predispone per il metodo un contesto "pulito" su cui poter operare, deve inoltre garantire che nessuna informazione del contesto precedente sia persa. ISR deve essere molto veloce e cercare di non perdere tempo nel salvare o ripristinare informazioni inutili.



#### tipi di scheduling

Supponiamo questi jobs:

<div style="background-color: lightblue;width: 8%;">DEC</div>
<div style="background-color: lightcyan;width: 33%;">CHECK</div>
<div style="background-color: yellow;width: 28%;">REC</div>
<div style="background-color:greenyellow;width: 3%;">SW</div>
<div style="background-color: pink;width: 18%;">LCD</div>


- Lo schema di scheduling più banale è lo **static scheluding** 
  - **PRO** : 
    - semplice da implementare
  - **CONTRO** : 
    - l'ordine non cambia, nemmeno in vista di una situazione dinamica che richiederebbe casi diversi 
    - i task vengono eseguiti per intero
<div style="background-color: lightblue; width: 8%; display: inline-block;">DEC</div>
<div style="background-color: lightcyan;width: 33%; display: inline-block;">CHECK</div>
<div style="background-color: yellow;width: 28%; display: inline-block;">REC</div>
<div style="background-color:greenyellow;width: 3%; display: inline-block;">SW</div>
<div style="background-color: pink;width: 18%; display: inline-block;">LCD</div>

Gli scheduling dinamici tentano invece di dare un ordine in base alle proprietà dei jobs, proprietà che possono anche cambiare col passare del tempo. Possono essere di due tipi:

- **i Dynamic RTC (<u>R</u>un <u>T</u>o <u>C</u>ompletion) Schedule**
  - tipi di scheduling che iniziano e quindi portano a termine ogni task

- **Dynamic preemptive schelude**
  - si interscambiano in memoria, possono quindi essere interfogliati tra di loro, interrompendosi a vicenda



### Real Time Operating Systems (RTOS)

I sistemi real time sono quei sistemi che possono calcolare e garantire i tempi massimi di risposta per ogni task, includendo anche i tempi di ISR.

#### definizioni:

- **deadline**: lasso di tempo massimo in cui un task deve rispondere e terminare. Stretto vincolo temporale per i RTOS 

### Vincoli e problemi di sviluppo economici e non dei sistemi integrati

Bisogna essere progettisti di un buon sistema integrato ma bisogna farlo anche entro certi limiti di tempo ed economici, per questo è buona norma seguire una linea di sviluppo che non impieghi più risorse di quelle richieste ( prevedendo poi che la manutenzione del sistema è quella parte post-produzione che verrà affrontata nel giusto lasso di tempo )

#### consigli

1. iniziare con i requisiti del cliente 

2. progettare le basi del sistema per definirne le colonne portanti (i task, i moduli etc...)

3. Aggiungere i requisiti mancanti:

4. tolleranza ai guasti

5. vincoli di tempo

6. la documentazione

7. accorgimenti sulla sicurezza

8. Progettazione su carta in maniera dettagliata
9. Implementazione del codice, revisione e test
10. accorgimenti post produzione

#### Jack Ganssle's reasons

[Jack Ganssle](https://www.embedded.com/author/jack-ganssle/ "Jack Ganssle") ha stilato 10 motivazioni che possono portare un progetto di sistema embedded in problematiche.

1. Schedulazioni surreali

2. Bassa ricerca della qualità ( anche nel firmware )
3. Pianificazioni a basso costo
4. Programmazione "ottimistica" ( poca considerazione dei casi estremi e di errori )
5. Leader di progetto non validi
6. Interfacce di comunicazioni analogiche/digitali scadenti
7. Basi teoriche fragili (o basi scientifiche)
8. uso scorretto dei linguaggi di programmazione (c e c++ in particolar modo)
9. Fase di programmazione precoce
10. Risorse allocate al progetto insufficienti

#### Il basso consumo

Bisogna sempre operare nel tentativo di ridurre il più possibile il consumo energetico dei dispositivi

- ridurre il numero di istruzioni al necessario, otimizzando il codice il più possibile
- sfruttare il più possibile le caratteristiche fisiche del dispositivo, li dove fanno risparmiare delle operazioni 
- sfruttare tecniche e librerie già note e ottimizzate
- diminuire i tempi di latenza con degli scheduler efficaci 

Alcune librerie note che ottimizzano funzioni matematiche e numeriche:

- Numerical Algorithm Group (NAG) 
- Intel Math Kernel Library
- CMSIS-DSP

## Tecnologie studiate: ARM e ARM Cortex M4

