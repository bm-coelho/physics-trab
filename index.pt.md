---
layout: default
---

<script type="text/javascript" async
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/3.2.2/es5/tex-mml-chtml.js">
</script>



# Simulação de Fluido 2D no Método Euleriano

![Simulação de Fluido](/physics-trab/assets/evolution.gif)

Este projeto simula o fluxo de um fluido incompressível bidimensional utilizando a abordagem Euleriana e métodos de diferenças finitas. A simulação é baseada nas equações de Navier-Stokes, que descrevem o movimento de substâncias fluidas. Para esta demonstração, assumimos:

1. O fluido é Newtoniano, ou seja, a viscosidade é constante.
2. O fluido é incompressível, então a densidade permanece constante.
3. O fluido é isotérmico, tornando a temperatura irrelevante.
4. Nenhuma força externa é aplicada.
5. O sistema é bidimensional.
6. O fluido está contido em uma caixa onde todas as paredes não possuem velocidade, exceto a parede superior, que se move.

> 🌎 Este documento também está disponível em [Inglês](/index.html).

---

## Funcionalidades
- Solução numérica das equações de Navier-Stokes para o fluxo de fluido incompressível em 2D.
- Esquema de avanço temporal explícito para evolução no tempo.
- Métodos de diferenças finitas para discretização espacial.
- Solucionador iterativo de Gauss-Seidel para a equação de pressão de Poisson.
- Visualização dos campos de velocidade e da dinâmica do fluxo.

---

## Como Funciona

### Equações Governantes

O comportamento do fluido é governado pelas equações de Navier-Stokes:

1. **Conservação de Momento**:

$$
\rho \frac{\partial \vec{u}}{\partial t} = -\nabla p + \mu \nabla^2 \vec{u} + \vec{F},
$$

que considera a taxa de variação do momento devido à pressão, forças viscosas e forças externas.

2. **Equação de Continuidade**:

$$
\nabla \cdot \vec{u} = 0,
$$

que garante a condição de incompressibilidade.

Para um fluxo bidimensional sem força externa, as equações de Navier-Stokes se expandem em:

1. **Equação de momento em $x$**:

$$
\frac{\partial u}{\partial t} 
+ u \frac{\partial u}{\partial x} 
+ v \frac{\partial u}{\partial y} 
= 
-\frac{1}{\rho}\frac{\partial p}{\partial x} 
+ \frac{1}{\rho} \nu \left( \frac{\partial^2 u}{\partial x^2}  + \frac{\partial^2 u}{\partial y^2} \right).
$$


2. **Equação de momento em $y$**:

$$
\frac{\partial v}{\partial t} 
+ u \frac{\partial v}{\partial x} 
+ v \frac{\partial v}{\partial y} 
= 
-\frac{1}{\rho}\frac{\partial p}{\partial y} 
+ \frac{1}{\rho}\nu \left( \frac{\partial^2 v}{\partial x^2} + \frac{\partial^2 v}{\partial y^2} \right).
$$


3. **Equação de continuidade**:

$$
\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} = 0.
$$


---

### Variáveis e Parâmetros

<table>
  <thead>
    <tr>
      <th>Variável</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>\(\rho\)</td>
      <td>Densidade do fluido, medindo a massa por unidade de volume.</td>
    </tr>
    <tr>
      <td>\(\vec{u}\)</td>
      <td>Vetor velocidade \((u, v)\), onde \(u\) e \(v\) são as componentes nas direções \(x\) e \(y\).</td>
    </tr>
    <tr>
      <td>\(p\)</td>
      <td>Campo de pressão, representando forças compressivas.</td>
    </tr>
    <tr>
      <td>\(\mu\)</td>
      <td>Viscosidade dinâmica, que quantifica a resistência à deformação.</td>
    </tr>
    <tr>
      <td>\(\nu\)</td>
      <td>Viscosidade cinemática, \(\nu = \frac{\mu}{\rho}\).</td>
    </tr>
    <tr>
      <td>\(\vec{F}\)</td>
      <td>Forças externas, como gravidade.</td>
    </tr>
  </tbody>
</table>


---

### Estrutura da Simulação

1. **Inicialização**:
   - Configurar os campos iniciais de velocidade e pressão.
2. **Condições de Contorno**:
   - Aplicar condições de contorno para velocidade e pressão nas paredes.
3. **Equações de Momento**:
   - Calcular campos intermediários de velocidade sem considerar contribuições da pressão.
4. **Correção de Pressão**:
   - Resolver a equação de pressão de Poisson para garantir incompressibilidade.
   - Atualizar os campos de velocidade usando o gradiente de pressão.
5. **Iterar**:
   - Avançar a simulação repetindo os passos acima pelo número desejado de passos de tempo.

---

## Métodos Numéricos

### Discretização Espacial

As equações governantes são discretizadas utilizando métodos de diferenças finitas:

- **Derivadas de primeira ordem** (e.g., termos convectivos):

$$
\frac{\partial u}{\partial x} \approx \frac{u_{i+1,j} - u_{i-1,j}}{2 \Delta x}.
$$


- **Derivadas de segunda ordem** (e.g., termos difusivos):

$$
\frac{\partial^2 u}{\partial x^2} \approx \frac{u_{i+1,j} - 2u_{i,j} + u_{i-1,j}}{\Delta x^2}.
$$


- **Equação de Poisson de Pressão**:
  Resolvida iterativamente utilizando o método de Gauss-Seidel.

### Integração Temporal

A evolução no tempo é calculada utilizando avanço explícito, com o tamanho do passo ($\Delta t$) determinado por:

1. **Condição de CFL**:

$$
\Delta t < \frac{\Delta x}{\text{max}(|u|)}.
$$


2. **Restrição de estabilidade viscosa**:

$$
\Delta t < \frac{\rho \Delta x^2}{\mu}.
$$


O menor valor entre os dois é escolhido para garantir a estabilidade.

---

## Visualização

### Exemplos de Saídas

1. **Campo de Velocidade**:

<img src="/physics-trab/assets/velocity_field_scaled.png" alt="Campo de Velocidade" width="600" />

2. **Linhas de Corrente**:

<img src="/physics-trab/assets/streamlines.png" alt="Linhas de Corrente" width="600" />

3. **Distribuição de Pressão**:

<img src="/physics-trab/assets/pressure_contours.png" alt="Campo de Pressão" width="600" />

---

## Como Executar

### Pré-requisitos
- Python 3.8+ com as seguintes bibliotecas:
  - `numpy`
  - `matplotlib`
  - `tqdm`
  - `Pillow`

### Instruções
1. Clone o repositório:
   ```bash
   git clone https://github.com/your-repo/fluid-simulation.git
   cd fluid-simulation
   ```

2. Instale as dependências:
   ```bash
   pip install -r requirements.txt
   ```
   
3. Abra o Jupyter Notebook:
   ```bash
   jupyter notebook main.ipynb
   ```

4. Execute todas as células do notebook para rodar a simulação e gerar visualizações.

---

## Referências

1. Bitiușcă, L.-G. (2016). *Eulerian Fluid Simulator*. MSc thesis, Bournemouth University.
2. Saad, M. (2024 & 2019). *Computational Fluid Dynamics Lessons*. University of Utah. Disponível em [YouTube Playlist](https://www.youtube.com/playlist?list=PLEaLl6Sf-KICvBLrYFwt5h_LgedJyN59n).
3. Bridson, R. (2015). *Fluid Simulation for Computer Graphics*. CRC Press.
