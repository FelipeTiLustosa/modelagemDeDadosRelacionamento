## Modelagem de dados relacionameneto
Relacionamentos de **um para muitos**, **muitos para um**, **muitos para muitos** e **um para um** são conceitos fundamentais em modelagem de dados, e você pode implementá-los em Java sem usar frameworks como o Spring ou Hibernate. Esses relacionamentos são usados para conectar diferentes classes, representando como os objetos de uma classe se relacionam com objetos de outra.

Aqui vou te mostrar exemplos simples, sem frameworks, utilizando apenas classes Java.

### 1. Relacionamento **Um para Muitos**

No relacionamento **um para muitos**, um objeto de uma classe pode estar associado a vários objetos de outra classe. Um exemplo clássico é uma **Escola** que tem muitos **Alunos**.

#### Exemplo: Escola e Aluno

```java
import java.util.ArrayList;
import java.util.List;

// Classe Escola
public class Escola {
    private String nome;
    private List<Aluno> alunos;  // Uma escola pode ter muitos alunos

    public Escola(String nome) {
        this.nome = nome;
        this.alunos = new ArrayList<>();  // Inicializando a lista de alunos
    }

    // Adiciona aluno à escola
    public void adicionarAluno(Aluno aluno) {
        alunos.add(aluno);
    }

    // Lista os alunos da escola
    public List<Aluno> getAlunos() {
        return alunos;
    }

    // Exibe informações da escola
    public void exibirEscola() {
        System.out.println("Escola: " + nome);
        for (Aluno aluno : alunos) {
            System.out.println("Aluno: " + aluno.getNome());
        }
    }
}

// Classe Aluno
public class Aluno {
    private String nome;

    public Aluno(String nome) {
        this.nome = nome;
    }

    public String getNome() {
        return nome;
    }
}
```

#### Teste do relacionamento:
```java
public class Main {
    public static void main(String[] args) {
        Escola escola = new Escola("Escola ABC");
        
        // Criando alunos
        Aluno aluno1 = new Aluno("Carlos");
        Aluno aluno2 = new Aluno("Ana");
        
        // Adicionando alunos à escola
        escola.adicionarAluno(aluno1);
        escola.adicionarAluno(aluno2);

        // Exibindo informações da escola e seus alunos
        escola.exibirEscola();
    }
}
```

**Saída esperada:**
```
Escola: Escola ABC
Aluno: Carlos
Aluno: Ana
```

Aqui, temos um **relacionamento um para muitos**, onde **uma escola** pode ter **muitos alunos**.

---

### 2. Relacionamento **Muitos para Um**

O relacionamento **muitos para um** é o inverso do anterior: **muitos alunos podem pertencer a uma única escola**. Isso já está refletido no exemplo acima, porque um **Aluno** pertence a uma única **Escola**.

#### Modificando a Classe Aluno:
```java
public class Aluno {
    private String nome;
    private Escola escola;  // Um aluno pertence a uma escola

    public Aluno(String nome, Escola escola) {
        this.nome = nome;
        this.escola = escola;
    }

    public String getNome() {
        return nome;
    }

    public Escola getEscola() {
        return escola;
    }

    public void exibirAluno() {
        System.out.println("Aluno: " + nome + " pertence à escola: " + escola.getNome());
    }
}
```

#### Teste:
```java
public class Main {
    public static void main(String[] args) {
        Escola escola = new Escola("Escola ABC");
        
        // Criando alunos e associando-os a uma escola
        Aluno aluno1 = new Aluno("Carlos", escola);
        Aluno aluno2 = new Aluno("Ana", escola);

        aluno1.exibirAluno();
        aluno2.exibirAluno();
    }
}
```

**Saída esperada:**
```
Aluno: Carlos pertence à escola: Escola ABC
Aluno: Ana pertence à escola: Escola ABC
```

Agora, cada **Aluno** está associado a uma única **Escola**.

---

### 3. Relacionamento **Muitos para Muitos**

No relacionamento **muitos para muitos**, vários objetos de uma classe podem estar associados a vários objetos de outra classe. Um exemplo comum é **Estudantes e Cursos**: um estudante pode se matricular em vários cursos, e um curso pode ter vários estudantes.

#### Exemplo: Estudante e Curso

```java
import java.util.ArrayList;
import java.util.List;

public class Estudante {
    private String nome;
    private List<Curso> cursos;  // Um estudante pode ter vários cursos

    public Estudante(String nome) {
        this.nome = nome;
        this.cursos = new ArrayList<>();
    }

    public void matricularCurso(Curso curso) {
        cursos.add(curso);
        curso.adicionarEstudante(this);  // Adiciona estudante ao curso também
    }

    public List<Curso> getCursos() {
        return cursos;
    }

    public String getNome() {
        return nome;
    }
}

public class Curso {
    private String nome;
    private List<Estudante> estudantes;  // Um curso pode ter muitos estudantes

    public Curso(String nome) {
        this.nome = nome;
        this.estudantes = new ArrayList<>();
    }

    public void adicionarEstudante(Estudante estudante) {
        estudantes.add(estudante);
    }

    public List<Estudante> getEstudantes() {
        return estudantes;
    }

    public String getNome() {
        return nome;
    }
}
```

#### Teste do relacionamento:
```java
public class Main {
    public static void main(String[] args) {
        Estudante estudante1 = new Estudante("Carlos");
        Estudante estudante2 = new Estudante("Ana");

        Curso curso1 = new Curso("Matemática");
        Curso curso2 = new Curso("História");

        // Matriculando estudantes em cursos
        estudante1.matricularCurso(curso1);
        estudante1.matricularCurso(curso2);
        estudante2.matricularCurso(curso1);

        // Exibindo cursos de cada estudante
        System.out.println("Estudante: " + estudante1.getNome());
        for (Curso curso : estudante1.getCursos()) {
            System.out.println("Matriculado no curso: " + curso.getNome());
        }

        System.out.println("Estudante: " + estudante2.getNome());
        for (Curso curso : estudante2.getCursos()) {
            System.out.println("Matriculado no curso: " + curso.getNome());
        }

        // Exibindo estudantes de um curso
        System.out.println("Curso: " + curso1.getNome());
        for (Estudante estudante : curso1.getEstudantes()) {
            System.out.println("Estudante matriculado: " + estudante.getNome());
        }
    }
}
```

**Saída esperada:**
```
Estudante: Carlos
Matriculado no curso: Matemática
Matriculado no curso: História
Estudante: Ana
Matriculado no curso: Matemática
Curso: Matemática
Estudante matriculado: Carlos
Estudante matriculado: Ana
```

Aqui, vemos que:
- O estudante **Carlos** está matriculado em **Matemática** e **História**.
- O estudante **Ana** está matriculado em **Matemática**.
- O curso de **Matemática** tem **Carlos** e **Ana** como estudantes.

---

### 4. Relacionamento **Um para Um**

No relacionamento **um para um**, um objeto de uma classe está associado a exatamente um objeto de outra classe. Um exemplo pode ser um **Pessoa** que tem **Passaporte**: cada pessoa tem um único passaporte, e cada passaporte pertence a uma única pessoa.

#### Exemplo: Pessoa e Passaporte

```java
public class Pessoa {
    private String nome;
    private Passaporte passaporte;  // Uma pessoa tem um passaporte

    public Pessoa(String nome, Passaporte passaporte) {
        this.nome = nome;
        this.passaporte = passaporte;
    }

    public Passaporte getPassaporte() {
        return passaporte;
    }

    public String getNome() {
        return nome;
    }

    public void exibirPessoa() {
        System.out.println("Pessoa: " + nome + ", Passaporte: " + passaporte.getNumero());
    }
}

public class Passaporte {
    private String numero;

    public Passaporte(String numero) {
        this.numero = numero;
    }

    public String getNumero() {
        return numero;
    }
}
```

#### Teste:
```java
public class Main {
    public static void main(String[] args) {
        Passaporte passaporte = new Passaporte("123456789");
        Pessoa pessoa = new Pessoa("Carlos", passaporte);

        pessoa.exibirPessoa();
    }
}
```

**Saída esperada:**
```
Pessoa: Carlos, Passaporte: 123456789
```

Aqui, temos uma **Pessoa** associada a um único **Passaporte**, e o passaporte pertence a essa pessoa.

---

### Conclusão

Esses exemplos mostram os quatro tipos principais de relacionamentos entre classes em Java:
- **Um para muitos**: Uma escola pode ter muitos alunos.
- **Muitos para um**: Muitos alunos podem pertencer a uma única escola.
- **Muitos para muitos**: Estudantes podem se matricular em muitos cursos, e cursos podem ter muitos estudantes.
- **Um

 para um**: Uma pessoa tem um único passaporte, e cada passaporte pertence a uma única pessoa.

Sem frameworks, você pode usar listas e métodos para gerenciar esses relacionamentos manualmente, mas frameworks como o Hibernate facilitam muito esse tipo de implementação ao lidar automaticamente com bancos de dados.
