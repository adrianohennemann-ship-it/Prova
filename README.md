// Programa para criar provas personalizadas

class Questao {
    constructor(numero, enunciado, tipo, alternativas = null, respostaCorreta = null) {
        this.numero = numero;
        this.enunciado = enunciado;
        this.tipo = tipo;
        this.alternativas = alternativas;
        this.respostaCorreta = respostaCorreta;
    }

    exibir() {
        console.log(`\n${this.numero}. ${this.enunciado}`);
        if (this.tipo === 'objetiva' && this.alternativas) {
            for (let letra in this.alternativas) {
                console.log(`   ${letra}) ${this.alternativas[letra]}`);
            }
        } else if (this.tipo === 'dissertativa') {
            console.log('   (Questão dissertativa)');
            console.log('   Resposta: ___________________________________');
        }
    }
}

class Prova {
    constructor(professor, escola, assunto, questoes, pesos, modoGeral) {
        this.professor = professor;
        this.escola = escola;
        this.assunto = assunto;
        this.questoes = questoes;
        this.pesos = pesos;
        this.modoGeral = modoGeral;
    }

    exibirProva() {
        console.log('\n' + '='.repeat(60));
        console.log(`PROVA ELABORADA POR: ${this.professor}`);
        console.log(`ESCOLA: ${this.escola}`);
        console.log(`ASSUNTO: ${this.assunto}`);
        console.log(`MODO GERAL: ${this.modoGeral === 'objetiva' ? 'Objetiva' : 'Dissertativa'}`);
        console.log('='.repeat(60));
        
        let notaTotal = 0;
        for (let peso of this.pesos) {
            notaTotal += peso;
        }
        console.log(`Valor total da prova: ${notaTotal} pontos\n`);
        
        for (let questao of this.questoes) {
            questao.exibir();
        }
        
        console.log('\n' + '='.repeat(60));
        console.log('BOA PROVA!');
    }
}

// Função para gerar questão baseada no conteúdo
function gerarQuestao(numero, tipo, conteudo) {
    const questoesObjetivas = [
        `Com base no conteúdo: "${conteudo.substring(0, 50)}...", qual é o principal conceito abordado?`,
        `De acordo com o material, qual das alternativas melhor define o tema central?`,
        `Sobre o conteúdo apresentado, é CORRETO afirmar que:`,
        `Considerando o texto, qual fator é determinante para o entendimento do assunto?`,
        `Analisando o conteúdo, podemos concluir que:`
    ];
    
    const questoesDissertativas = [
        `Disserte sobre os principais pontos abordados no conteúdo: "${conteudo.substring(0, 50)}..."`,
        `Explique detalhadamente a importância do tema apresentado no contexto atual.`,
        `Analise criticamente o conteúdo, destacando seus aspectos mais relevantes.`,
        `Desenvolva um texto dissertativo sobre as implicações práticas do assunto estudado.`,
        `Compare e contraste as diferentes perspectivas apresentadas no material.`
    ];
    
    let enunciado;
    let alternativas = null;
    let respostaCorreta = null;
    
    if (tipo === 'objetiva') {
        const indice = (numero - 1) % questoesObjetivas.length;
        enunciado = questoesObjetivas[indice];
        
        // Gerar 5 alternativas
        alternativas = {
            'a': `${conteudo.split(' ')[0] || 'Conceito A'} - Primeira interpretação do tema`,
            'b': `${conteudo.split(' ')[1] || 'Conceito B'} - Segunda abordagem do conteúdo`,
            'c': `${conteudo.split(' ')[2] || 'Conceito C'} - Terceira perspectiva analisada`,
            'd': 'Todas as alternativas anteriores estão corretas',
            'e': 'Nenhuma das alternativas anteriores está correta'
        };
        respostaCorreta = 'c'; // Resposta padrão para exemplo
    } else {
        const indice = (numero - 1) % questoesDissertativas.length;
        enunciado = questoesDissertativas[indice];
    }
    
    return new Questao(numero, enunciado, tipo, alternativas, respostaCorreta);
}

// Função principal interativa
function criarProva() {
    const readline = require('readline');
    const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout
    });
    
    console.log('='.repeat(60));
    console.log('SISTEMA DE CRIAÇÃO DE PROVAS');
    console.log('='.repeat(60));
    
    rl.question('Nome do professor: ', (professor) => {
        rl.question('Nome da escola: ', (escola) => {
            rl.question('Assunto abordado: ', (assunto) => {
                rl.question('Quantidade de questões: ', (qtdQuestoes) => {
                    const numQuestoes = parseInt(qtdQuestoes);
                    
                    rl.question('Modo geral da prova (objetiva/dissertativa): ', (modoGeral) => {
                        const modo = modoGeral.toLowerCase();
                        if (modo !== 'objetiva' && modo !== 'dissertativa') {
                            console.log('Modo inválido! Usando modo misto.');
                        }
                        
                        console.log('\nAgora, informe o peso de cada questão:');
                        const pesos = [];
                        let contador = 0;
                        
                        function perguntarPeso() {
                            if (contador < numQuestoes) {
                                rl.question(`Peso da questão ${contador + 1}: `, (peso) => {
                                    pesos.push(parseFloat(peso));
                                    contador++;
                                    perguntarPeso();
                                });
                            } else {
                                console.log('\n' + '='.repeat(60));
                                console.log('DIGITE O CONTEÚDO DA PROVA:');
                                console.log('(O sistema usará este texto para gerar as questões)');
                                console.log('='.repeat(60));
                                
                                rl.question('Conteúdo: ', (conteudo) => {
                                    // Gerar questões baseadas no conteúdo
                                    const questoes = [];
                                    for (let i = 0; i < numQuestoes; i++) {
                                        const tipoQuestao = modo === 'objetiva' ? 'objetiva' : 
                                                          modo === 'dissertativa' ? 'dissertativa' :
                                                          (i % 2 === 0 ? 'objetiva' : 'dissertativa');
                                        const questao = gerarQuestao(i + 1, tipoQuestao, conteudo);
                                        questoes.push(questao);
                                    }
                                    
                                    const prova = new Prova(professor, escola, assunto, questoes, pesos, modo);
                                    prova.exibirProva();
                                    
                                    rl.close();
                                });
                            }
                        }
                        
                        perguntarPeso();
                    });
                });
            });
        });
    });
}

// Executar o programa
if (typeof require !== 'undefined' && require.main === module) {
    criarProva();
}
