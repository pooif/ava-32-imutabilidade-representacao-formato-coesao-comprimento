# 3.2 // Imutabilidade, Representação, Formato e Coesão // Comprimento

Use este link do GitHub Classroom para ter sua cópia alterável deste repositório: <https://classroom.github.com/a/ICynJx1V>

Implementar respeitando os fundamentos de Orientação a Objetos.

**Tópicos desta atividade:** imutabilidade, formatos de representação, formatos de entrada, coesão

---

Implementar o objeto `Comprimento`. Instâncias de `Comprimento` devem representar uma extensão ([distância entre dois pontos](https://pt.wikipedia.org/wiki/Comprimento)) a partir de várias unidades, sendo considerada inicialmente a unidade básica metro, conforme SI. É usada uma precisão de milimetros, portanto este é o atributo que determina o comprimento. Considere os Casos de Teste:

```java
// construtores:
Comprimento zero = new Comprimento();
System.out.println(zero.milimetros == 0);

// milimetros é constante, não deve compilar:
zero.milimetros = 10; // comente essa linha após fazê-la falhar

// construtor double metro:
Comprimento umMetro = new Comprimento(1.0);
System.out.println(umMetro.milimetros == 1000);

Comprimento umMetroMeio = new Comprimento(1.5);
System.out.println(umMetroMeio.milimetros == 1500);

Comprimento cemMetros = new Comprimento(100.0);
System.out.println(cemMetros.milimetros == 100000);

// construtor inteiro milimetros:
Comprimento umCentimetro = new Comprimento(100);
System.out.println(umCentimetro.milimetros == 100);

// comprimentos inválidos, negativo!
// Faça lançar exceção e abrace-as com try/catch
Comprimento invalido1 = new Comprimento(-1.0);
Comprimento invalido2 = new Comprimento(-10);

// métodos estáticos fábrica:
Comprimento umaPolegada = Comprimento.fromPolegadas(1.0);
System.out.println(umaPolegada.milimetros == 25);

Comprimento cincoPolegadas = Comprimento.fromPolegadas(5.0);
System.out.println(cincoPolegadas.milimetros == 127);

Comprimento dozeMilimetros = Comprimento.fromString("12mm");
System.out.println(dozeMilimetros.milimetros == 12);

Comprimento dozeCentimetros = Comprimento.fromString("12cm");
System.out.println(dozeCentimetros.milimetros == 120);

Comprimento dozePolegadas = Comprimento.fromString("12\"");
// seria 304.8mm, mas os mm devem ser truncados, não arredondados.
System.out.println(dozePolegadas.milimetros == 304);

Comprimento dozeMetros = Comprimento.fromString("12m");
System.out.println(dozeMetros.milimetros == 12000);

// qualquer string fora deste padrão [0-9](mm|cm|m|") deve ser rejeitada
// faça lançar exceção e abrace-as com try/catch
Comprimento.fromString("12");
Comprimento.fromString("12e");
Comprimento.fromString("12 m");
Comprimento.fromString("12M");

// consultas: (pode ser ajustado para o arredondamento, exceto de mm que é truncado)
System.out.println(cincoPolegadas.getCentimetros() == 12.7);
System.out.println(cincoPolegadas.getMetros() == 0.127);
System.out.println(cemMetros.getPolegadas() == 3937.0);
System.out.println(dozeMetros.getMilimetros() == 12000);

// toString
System.out.println(umMetro.toString()); // 1000mm
System.out.println(umMetro.toString().equals("1000mm"));
System.out.println(umMetroMeio.toString()); // 1500mm
System.out.println(umMetroMeio.toString().equals("1500mm"));
System.out.println(cemMetros.toString()); // 100000mm
System.out.println(cemMetros.toString().equals("100000mm"));

// Unidade é um enum declarado dentro da classe Comprimento com as seguintes constantes:
System.out.println(umMetro.toString(Comprimento.Unidade.POLEGADA)); // 39.37"
System.out.println(umMetro.toString(Comprimento.Unidade.POLEGADA).equals("39.37\""));
System.out.println(umMetroMeio.toString(Comprimento.Unidade.CENTIMETRO)); // 150cm
System.out.println(umMetroMeio.toString(Comprimento.Unidade.CENTIMETRO).equals("150cm"));
System.out.println(cemMetros.toString(Comprimento.Unidade.METRO)); // 100m
System.out.println(cemMetros.toString(Comprimento.Unidade.METRO).equals("100m"));
System.out.println(cemMetros.toString(Comprimento.Unidade.KILOMETRO)); // 0.1km
System.out.println(cemMetros.toString(Comprimento.Unidade.KILOMETRO).equals("0.1km"));

// operações: (Comprimento é imutável)
Comprimento doisMetrosMeio = umMetro.mais(umMetroMeio);
System.out.println(umMetro.milimetros == 1000);
System.out.println(umMetroMeio.milimetros == 1500);
System.out.println(doisMetrosMeio.milimetros == 2500);

Comprimento dezMetros = doisMetrosMeio.mais(7.5); // 2.5m + 7.5m
System.out.println(dezMetros.milimetros == 10000);

Comprimento dezMetrosComOitentaMilimetros = dezMetros.mais(80); // + 80mm
System.out.println(dezMetrosComOitentaMilimetros.milimetros == 10080);

Comprimento vinteMetros = dezMetros.dobro();
System.out.println(vinteMetros.milimetros == 20000);

Comprimento duzentosMetros = vinteMetros.vezes(10);
System.out.println(duzentosMetros.milimetros == 200000);
System.out.println(duzentosMetros.toString(Comprimento.Unidade.KILOMETRO)); // 0.2km
System.out.println(duzentosMetros.toString(Comprimento.Unidade.KILOMETRO).equals("0.2km"));

// 4 segmentos de 50m
Comprimento[] segmentos = duzentosMetros.segmentos(4);
System.out.println(segmentos[0].milimetros == 50000);
System.out.println(segmentos[1].milimetros == 50000);
System.out.println(segmentos[2].milimetros == 50000);
System.out.println(segmentos[3].milimetros == 50000);

Comprimento[] cincoPolegadasEmTresSegmentos = cincoPolegadas.segmentos(3);
System.out.println(cincoPolegadasEmTresSegmentos[0].milimetros == 42);
System.out.println(cincoPolegadasEmTresSegmentos[1].milimetros == 42);
// o acumulo do resto fica no último segmento
System.out.println(cincoPolegadasEmTresSegmentos[2].milimetros == 43);

// concatenar
Comprimento conc1 = Comprimento.fromSegmentos(segmentos);
System.out.println(conc1.milimetros == 200000);
System.out.println(conc1.equals(duzentosMetros));

// usar o varargs (Google Java+varargs)
Comprimento conc2 = Comprimento.fromSegmentos(umaPolegada, cincoPolegadas, dozePolegadas);
System.out.println(conc2.milimetros == 457);
System.out.println(conc2.getPolegadas() == 18.0);

// IMPLEMENTE E TESTE a subtração de comprimentos,
// considerando que não há comprimento negativo.
```



---
Obs.: os casos de teste não podem ser alterados, mas outros podem ser adicionados. Fique a vontade para adicionar códigos que imprimem ou separam os testes, por exemplo.
