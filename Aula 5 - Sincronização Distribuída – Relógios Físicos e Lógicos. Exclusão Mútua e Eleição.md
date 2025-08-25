Relógios Físicos e Lógicos

Relógio físico: é o relógio real do computador (igual ao que marca horas). O problema é que em sistemas distribuídos cada máquina pode ter um horário diferente (diferença de milissegundos já atrapalha).

Relógio lógico: não se preocupa com a hora real, mas com a ordem dos eventos. Ele garante que se o evento A aconteceu antes de B, todos no sistema vão concordar com essa ordem, mesmo que os relógios físicos estejam diferentes. (Ex.: Relógio de Lamport).

Em resumo:

Físico = hora real (precisa de sincronização).

Lógico = apenas a ordem dos eventos.

Exclusão Mútua:

Vários processos podem tentar acessar um recurso compartilhado (como um arquivo ou impressora).

A exclusão mútua garante que só um processo por vez use esse recurso, evitando conflitos.

Como não existe uma memória central, é preciso protocolos de mensagens entre os processos para decidir quem entra na “sala crítica”.

Pense numa impressora: dois computadores mandam imprimir, mas só um imprime por vez.

Eleição:

Em um sistema distribuído, pode ser necessário escolher um coordenador/líder (um processo que vai organizar algo, como quem controla a exclusão mútua).

O algoritmo de eleição serve para os processos decidirem quem será o líder se o atual falhar.

Exemplos: Algoritmo do Bully, Algoritmo em Anel.

É como numa turma sem professor: os alunos fazem uma votação para escolher um representante.


👨‍👩‍👧‍👦 Situação:
4 pessoas (Pai, Mãe, Filho e Filha) querem usar o banheiro ao mesmo tempo. Só existe 1 pia e 1 vaso.

🔒 Exclusão Mútua

Como o banheiro só comporta 1 pessoa por vez, os outros precisam esperar.

Ou seja: só um processo entra na seção crítica (banheiro).

⏱ Relógios Físicos e Lógicos

Cada pessoa poderia usar o relógio de pulso (físico) para dizer quem chegou primeiro. Mas se os relógios não estão iguais, pode dar briga.

Então a família define uma ordem lógica: Pai → Mãe → Filho → Filha.

Essa ordem substitui o relógio físico, e todos aceitam a sequência.

👑 Eleição

Agora, alguém precisa coordenar a fila.

A família faz uma eleição baseada na ordem pré-definida. Exemplo:

O Pai é eleito coordenador.

Se o Pai não estiver em casa, passa para a Mãe.

Se a Mãe não estiver, passa para o Filho.

Se também não, sobra para a Filha.

👉 Isso garante que sempre exista um coordenador para organizar quem vai ao banheiro.

✅ Resumo da metáfora:

Exclusão mútua → só 1 no banheiro de cada vez.

Relógio lógico → ordem definida (Pai → Mãe → Filho → Filha), independente da hora real.

Eleição → se o Pai não puder ser coordenador, automaticamente passa para o próximo da ordem.
