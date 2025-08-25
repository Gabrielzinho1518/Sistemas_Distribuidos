# Relógios Físicos e Lógicos, Exclusão Mútua e Eleição em Sistemas Distribuídos  

## ⏱ Relógios Físicos e Lógicos  

- **Relógio físico**: é o relógio real do computador (igual ao que marca horas).  
  - Problema: em sistemas distribuídos cada máquina pode ter um horário diferente (diferença de milissegundos já atrapalha).  

- **Relógio lógico**: não se preocupa com a hora real, mas sim com a **ordem dos eventos**.  
  - Garante que se o evento **A aconteceu antes de B**, todos no sistema concordem com essa ordem.  
  - Exemplo: **Relógio de Lamport**.  

**Resumo:**  
- **Físico** = hora real (precisa de sincronização).  
- **Lógico** = apenas a ordem dos eventos.  

---

## 🔒 Exclusão Mútua  

- Vários processos podem tentar acessar um **recurso compartilhado** (como um arquivo ou impressora).  
- A exclusão mútua garante que **só um processo por vez** use esse recurso, evitando conflitos.  
- Como não existe uma **memória central**, são usados **protocolos de mensagens** para decidir quem entra na seção crítica.  

**Exemplo:**  
- Impressora → dois computadores mandam imprimir, mas só **um imprime por vez**.  

---

## 👑 Eleição  

- Em um sistema distribuído pode ser necessário escolher um **coordenador/líder**.  
- O algoritmo de eleição serve para os processos decidirem **quem será o líder** se o atual falhar. 

**Exemplo prático:**  
- É como numa turma sem professor: os alunos fazem uma **votação** para escolher um representante.  

---

## 👨‍👩‍👧‍👦 Metáfora do Banheiro  

**Situação:**  
4 pessoas (Pai, Mãe, Filho e Filha) querem usar o banheiro **ao mesmo tempo**.  
Mas só existe **1 pia** e **1 vaso**.  

### 🔒 Exclusão Mútua  
- O banheiro só comporta **1 pessoa por vez**.  
- Os outros precisam **esperar na fila**.  
- → Só um processo entra na **seção crítica** (banheiro).  

### ⏱ Relógios Físicos e Lógicos  
- Cada pessoa poderia usar o **relógio de pulso (físico)** para dizer quem chegou primeiro.  
  - Problema: os relógios podem estar **desajustados**.  
- Então a família define uma **ordem lógica fixa**:

- Essa ordem substitui o relógio físico, e todos aceitam a sequência.  

### 👑 Eleição  
- A família decide que alguém deve ser **coordenador da fila**.  
- Regra da eleição:  
1. O **Pai** é coordenador.  
2. Se o Pai não estiver, passa para a **Mãe**.  
3. Se a Mãe não estiver, passa para o **Filho**.  
4. Se também não, sobra para a **Filha**.  

👉 Assim, **sempre existe um coordenador** para organizar quem vai ao banheiro.  

---

## ✅ Resumo da Metáfora  

- **Exclusão mútua** → só 1 no banheiro de cada vez.  
- **Relógio lógico** → ordem definida (Pai → Mãe → Filho → Filha).  
- **Eleição** → se o coordenador atual não puder, passa para o próximo da ordem.



               [Fila da Família]

  ┌───────┐     ┌───────┐     ┌───────┐     ┌────────┐

      │  Pai  │ --> │  Mãe  │ --> │ Filho │ --> │ Filha  │

  └───────┘     └───────┘     └───────┘     └────────┘


  ^                                                
  |                                                      |
  |______________________________________________________|              

                      Ordem Lógica


🔒 Exclusão Mútua

[Banheiro]
   
   ┌─────────────┐
   
   │   Vaso +    │
   
   │    Pia      │
   
   └─────────────┘
      
       ▲
       
Só UMA pessoa entra por vez

### Código criado pelo GPT sobre o exemplo acima 
## Testado 

```java
// Exemplo simplificado de Exclusão Mútua, Relógio Lógico e Eleição
// Família no banheiro

import java.util.concurrent.locks.ReentrantLock;

class Banheiro {
    private final ReentrantLock lock = new ReentrantLock();

    // Exclusão mútua: apenas 1 pessoa por vez pode usar o banheiro
    public void usar(String pessoa) {
        lock.lock();
        try {
            System.out.println(pessoa + " entrou no banheiro 🚻");
            Thread.sleep(1000); // simulando tempo de uso
            System.out.println(pessoa + " saiu do banheiro ✅");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}

class Familia implements Runnable {
    private final String nome;
    private final Banheiro banheiro;

    public Familia(String nome, Banheiro banheiro) {
        this.nome = nome;
        this.banheiro = banheiro;
    }

    @Override
    public void run() {
        banheiro.usar(nome);
    }
}

public class Main {
    public static void main(String[] args) {

        Banheiro banheiro = new Banheiro();

        // Ordem lógica (Pai → Mãe → Filho → Filha)
        String[] ordemLogica = {"Pai", "Mãe", "Filho", "Filha"};

        // Eleição simples: o primeiro da lista é o coordenador
        String coordenador = ordemLogica[0];
        System.out.println("👑 Coordenador atual: " + coordenador);

        // Simulação de uso do banheiro na ordem lógica
        for (String pessoa : ordemLogica) {
            new Thread(new Familia(pessoa, banheiro)).start();
            try {
                Thread.sleep(1200); // controla a ordem de entrada
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        // Se o coordenador (Pai) "falhar"
        System.out.println("\n⚠️ O Pai não está disponível.");
        coordenador = ordemLogica[1];
        System.out.println("👉 Novo coordenador eleito: " + coordenador);
    }
}
