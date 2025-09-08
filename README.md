# @ismael1361/animation

Uma coleção de funções e classes utilitárias, fortemente tipadas, construídas com TypeScript para acelerar o desenvolvimento de projetos em JavaScript/TypeScript.

## Instalação

```bash
npm install @ismael1361/animation
# ou
yarn add @ismael1361/animation
```

---

## Indice

- [@ismael1361/animation](#ismael1361animation)
  - [Instalação](#instalação)
  - [Indice](#indice)
  - [Animation](#animation)
    - [`Animation.create: <S extends AnimationState = {}>(animation: AnimationFn<S>, state?: S) => AnimationProps<S>`](#animationcreate-s-extends-animationstate--animation-animationfns-state-s--animationpropss)
    - [Propriedades da Instância](#propriedades-da-instância)
      - [`.state: { [K in keyof S]: SharedValue<S[K]>; }`](#state--k-in-keyof-s-sharedvaluesk-)
      - [`.start(): void`](#start-void)
      - [`.clear(): void`](#clear-void)
      - [`.pause(): void`](#pause-void)
      - [`.resume(): void`](#resume-void)
      - [`.play(): void`](#play-void)
      - [`.stop(): void`](#stop-void)
      - [`.restart(): void`](#restart-void)
    - [Métodos](#métodos)
      - [`Animation.timeSincePreviousFrame(): InputGenerator<number>`](#animationtimesincepreviousframe-inputgeneratornumber)
      - [`Animation.timing(value: SharedValue<number> | TimingCallback, config?: TimingConfig): InputGenerator`](#animationtimingvalue-sharedvaluenumber--timingcallback-config-timingconfig-inputgenerator)
      - [`Animation.wait(duration?: number): InputGenerator`](#animationwaitduration-number-inputgenerator)
      - [`Animation.waitUntil(value: SharedValue<boolean>, invert?: boolean): InputGenerator`](#animationwaituntilvalue-sharedvalueboolean-invert-boolean-inputgenerator)
      - [`Animation.delay(duration?: number, animation?: Input | undefined): InputGenerator`](#animationdelayduration-number-animation-input--undefined-inputgenerator)
      - [`Animation.parallel(...animations: Inputs): InputGenerator`](#animationparallelanimations-inputs-inputgenerator)
      - [`Animation.all(...animations: Inputs): InputGenerator`](#animationallanimations-inputs-inputgenerator)
      - [`Animation.any(...animations: Inputs): InputGenerator`](#animationanyanimations-inputs-inputgenerator)
      - [`Animation.chain(...animations: Inputs): InputGenerator`](#animationchainanimations-inputs-inputgenerator)
      - [`Animation.stagger(delayMs: number, ...animations: Inputs): InputGenerator`](#animationstaggerdelayms-number-animations-inputs-inputgenerator)
      - [`Animation.sequence(delayMs: number, ...animations: Inputs): InputGenerator`](#animationsequencedelayms-number-animations-inputs-inputgenerator)
      - [`Animation.loop(...args: [factory: LoopCallback] | [iterations: number, factory: LoopCallback]): InputGenerator`](#animationloopargs-factory-loopcallback--iterations-number-factory-loopcallback-inputgenerator)
  - [Easing](#easing)
    - [`Easing.linear(t: number): number`](#easinglineart-number-number)
    - [`Easing.ease(t: number): number`](#easingeaset-number-number)
    - [`Easing.quad(t: number): number`](#easingquadt-number-number)
    - [`Easing.cubic(t: number): number`](#easingcubict-number-number)
    - [`Easing.poly(n: number): EasingFunction`](#easingpolyn-number-easingfunction)
    - [`Easing.sin(t: number): number`](#easingsint-number-number)
    - [`Easing.circle(t: number): number`](#easingcirclet-number-number)
    - [`Easing.exp(t: number): number`](#easingexpt-number-number)
    - [`Easing.elastic(bounciness?: number): EasingFunction`](#easingelasticbounciness-number-easingfunction)
    - [`Easing.back(s?: number): EasingFunction`](#easingbacks-number-easingfunction)
    - [`Easing.bounce(t: number): number`](#easingbouncet-number-number)
    - [`Easing.bezier(x1: number, y1: number, x2: number, y2: number): { factory: () => EasingFunction; }`](#easingbezierx1-number-y1-number-x2-number-y2-number--factory---easingfunction-)
    - [`Easing.bezierFn(x1: number, y1: number, x2: number, y2: number): EasingFunction`](#easingbezierfnx1-number-y1-number-x2-number-y2-number-easingfunction)
    - [`Easing.in(easing: EasingFunction): EasingFunction`](#easingineasing-easingfunction-easingfunction)
    - [`Easing.out(easing: EasingFunction): EasingFunction`](#easingouteasing-easingfunction-easingfunction)
    - [`Easing.inOut(easing: EasingFunction): EasingFunction`](#easinginouteasing-easingfunction-easingfunction)
    - [`Easing.steps(n?: number, roundToNextStep?: boolean | undefined): EasingFunction`](#easingstepsn-number-roundtonextstep-boolean--undefined-easingfunction)
  - [SharedValue](#sharedvalue)
    - [`class SharedValue<T>`](#class-sharedvaluet)
      - [Propriedades da Instância](#propriedades-da-instância-1)
        - [`.value: T`](#value-t)
        - [`.on(event: 'value' | 'change', callback: (value: T) => void): void`](#onevent-value--change-callback-value-t--void-void)
        - [`.off(event: 'value' | 'change', callback: (value: T) => void): void`](#offevent-value--change-callback-value-t--void-void)
        - [`.clear(): void`](#clear-void-1)
    - [`class SharedValues<S>`](#class-sharedvaluess)
      - [Propriedades da Instância](#propriedades-da-instância-2)
        - [`.values: S`](#values-s)
        - [`.current: { [K in keyof S]: SharedValue<S[K]>; }`](#current--k-in-keyof-s-sharedvaluesk-)
        - [`.on(event: 'value' | 'change', callback: (key: keyof S, value: S[keyof S]) => void): void`](#onevent-value--change-callback-key-keyof-s-value-skeyof-s--void-void)
        - [`.off(event: 'value' | 'change', callback: (key: keyof S, value: S[keyof S]) => void): void`](#offevent-value--change-callback-key-keyof-s-value-skeyof-s--void-void)
        - [`.initialize(): void`](#initialize-void)
        - [`.destroy(): void`](#destroy-void)
        - [`.clear(): void`](#clear-void-2)
    - [`sharedValues: <S>(state: S) => SharedValues<S>`](#sharedvalues-sstate-s--sharedvaluess)

## Animation

### `Animation.create: <S extends AnimationState = {}>(animation: AnimationFn<S>, state?: S) => AnimationProps<S>`

Cria e gerencia um loop de animação baseado em um gerador, fornecendo controles como play, pause e stop.

Esta função é o coração do sistema de animação. Ela recebe a lógica da animação (um gerador) e um estado inicial. O estado é convertido em `SharedValue`s reativos que podem ser manipulados pelas funções de animação (`timing`, `wait`, etc.) dentro do gerador. O objeto retornado permite iniciar, pausar, parar e reiniciar a animação.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

// 1. Defina o estado inicial que sua animação irá manipular.
const initialState = {
  progress: 0,
};

// 2. Crie a animação usando a função `create`.
// A função geradora recebe o estado como `SharedValue`s.
const { state, start, pause } = Animation.create(function* (state) {
  console.log("Animação iniciada!");

  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.wait(500); // Pausa por 500ms.

  // Anima de volta para 0.
  yield* Animation.timing(state.progress, { to: 0, duration: 1000 });

  console.log("Animação concluída!");
});

state.progress.on("change", (value)=>{
  console.log(`Progresso: ${value}`);
});

// 3. Inicie a animação.
start();

// Você pode controlar a animação a qualquer momento.
// setTimeout(() => pause(), 1200);
```

---

### Propriedades da Instância

#### `.state: { [K in keyof S]: SharedValue<S[K]>; }`

Um objeto contendo os SharedValues reativos do estado da animação. Você pode usar isso para ler o estado atual da sua animação de fora do gerador.

**Exemplo:**
```typescript
const myAnimation = Animation.create(..., { progress: 0 });
// Em um loop de renderização ou efeito:
console.log(myAnimation.state.progress.value);
```

#### `.start(): void`

Inicia a animação do começo. Se já estiver em execução, ela será reiniciada.

**Exemplo:**
```typescript
const myAnimation = Animation.create(..., { progress: 0 });
myAnimation.start();
```

#### `.clear(): void`

Limpa quaisquer recursos ou listeners criados pela animação (ex: via `onClear`).

**Exemplo:**
```typescript
const myAnimation = Animation.create(..., { progress: 0 });
myAnimation.clear();
```

#### `.pause(): void`

Pausa a animação em seu estado atual.

**Exemplo:**
```typescript
const myAnimation = Animation.create(..., { progress: 0 });
myAnimation.pause();
```

#### `.resume(): void`

Retoma uma animação que foi pausada.

**Exemplo:**
```typescript
const myAnimation = Animation.create(..., { progress: 0 });
myAnimation.pause();
myAnimation.resume();
```

#### `.play(): void`

Um atalho para `resume()`. Retoma uma animação pausada.

**Exemplo:**
```typescript
const myAnimation = Animation.create(..., { progress: 0 });
myAnimation.pause();
myAnimation.play();
```

#### `.stop(): void`

Para a animação completamente, limpa seus recursos e redefine seu estado.

**Exemplo:**
```typescript
const myAnimation = Animation.create(..., { progress: 0 });
myAnimation.stop();
```

#### `.restart(): void`

Um atalho para `stop()` seguido de `start()`. Reinicia a animação.

**Exemplo:**
```typescript
const myAnimation = Animation.create(..., { progress: 0 });
myAnimation.restart();
```

---

### Métodos

#### `Animation.timeSincePreviousFrame(): InputGenerator<number>`

Obtém o tempo decorrido (em milissegundos) desde o quadro de animação anterior. Usado dentro de um gerador de animação para controlar o fluxo de tempo.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.wait(500); // Pausa por 500ms.

  // Anima de volta para 0.
  yield* Animation.timing(state.progress, { to: 0, duration: 1000 });

  console.log("Animação concluída!");
});

animation.start();
```

#### `Animation.timing(value: SharedValue<number> | TimingCallback, config?: TimingConfig): InputGenerator`

Anima propriedade de um `SharedValue<number>` ou executa uma função de retorno de chamada com o valor animado.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.wait(500); // Pausa por 500ms.

  // Anima de volta para 0.
  yield* Animation.timing(state.progress, { to: 0, duration: 1000 });

  yield* Animation.wait(500); // Pausa por 500ms.

  // Usando uma função de retorno de chamada.
  yield* Animation.timing((value) => {
    console.log(`Progresso: ${value}`);
    state.progress = value;
  }, { to: 0, duration: 1000 });

  console.log("Animação concluída!");
});

animation.start();
```

#### `Animation.wait(duration?: number): InputGenerator`

Pausa a execução da animação por uma determinada duração.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.wait(500); // Pausa por 500ms.

  // Anima de volta para 0.
  yield* Animation.timing(state.progress, { to: 0, duration: 1000 });

  console.log("Animação concluída!");
});

animation.start();
```

#### `Animation.waitUntil(value: SharedValue<boolean>, invert?: boolean): InputGenerator`

Pausa a execução da animação até que uma condição em um `SharedValue<boolean>` seja atendida.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.waitUntil(state.progress, true); // Pausa enquanto 'progress' for menor que 1.

  // Anima de volta para 0.
  yield* Animation.timing(state.progress, { to: 0, duration: 1000 });

  console.log("Animação concluída!");
});

animation.start();
```

#### `Animation.delay(duration?: number, animation?: Input | undefined): InputGenerator`

Cria uma pausa e, opcionalmente, executa outra animação em seguida. É um atalho para combinar `wait` com outra animação.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.delay(500, Animation.timing(state.progress, { to: 0, duration: 1000 }));

  console.log("Animação concluída!");
});

animation.start();
```

#### `Animation.parallel(...animations: Inputs): InputGenerator`

Executa múltiplas animações (geradores) em paralelo. A execução termina quando todas as animações filhas tiverem sido concluídas.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.parallel(
    Animation.timing(state.progress, { to: 0, duration: 1000 }),
    Animation.wait(500)
  );

  console.log("Animação concluída!");
});

animation.start();
```

#### `Animation.all(...animations: Inputs): InputGenerator`

Um alias para `parallel`. Executa múltiplas animações em paralelo.A execução termina quando todas as animações filhas tiverem sido concluídas.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.all(
    Animation.timing(state.progress, { to: 0, duration: 1000 }),
    Animation.wait(500)
  );

  console.log("Animação concluída!");
});

animation.start();
```

#### `Animation.any(...animations: Inputs): InputGenerator`

Executa múltiplas animações (geradores) em paralelo e termina assim que a primeira delas for concluída. As outras animações são interrompidas.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.any(
    Animation.timing(state.progress, { to: 0, duration: 1000 }),
    Animation.wait(500)
  );

  console.log("Animação concluída!");
});

animation.start();
```

#### `Animation.chain(...animations: Inputs): InputGenerator`

Executa múltiplas animações (geradores) em sequência, uma após a outra.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.chain(
    Animation.timing(state.progress, { to: 0, duration: 1000 }),
    Animation.wait(500)
  );

  console.log("Animação concluída!");
});

animation.start();
```

#### `Animation.stagger(delayMs: number, ...animations: Inputs): InputGenerator`

Executa múltiplas animações em paralelo, mas com um atraso escalonado entre o início de cada uma.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.stagger(500,
    Animation.timing(state.progress, { to: 0, duration: 1000 }),
    Animation.wait(500)
  );

  console.log("Animação concluída!");
});

animation.start();
```

#### `Animation.sequence(delayMs: number, ...animations: Inputs): InputGenerator`

Executa múltiplas animações em sequência, com um atraso definido entre o fim de uma e o início da próxima.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.sequence(500,
    Animation.timing(state.progress, { to: 0, duration: 1000 }),
    Animation.wait(500)
  );

  console.log("Animação concluída!");
});

animation.start();
```

#### `Animation.loop(...args: [factory: LoopCallback] | [iterations: number, factory: LoopCallback]): InputGenerator`

Executa uma animação (gerador) repetidamente.

**Exemplo:**
```typescript
import { Animation } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000 });

  yield* Animation.loop(
    Animation.timing(state.progress, { to: 0, duration: 1000 })
  );

  console.log("Animação concluída!");
});

animation.start();
```

---

## Easing

### `Easing.linear(t: number): number`

Função linear, `f(t) = t`. A posição se correlaciona um-para-um com o tempo decorrido.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.linear });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.ease(t: number): number`

Uma interação inercial simples, semelhante a um objeto acelerando lentamente.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.ease });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.quad(t: number): number`

Função quadrática, `f(t) = t * t`. A posição é igual ao quadrado do tempo decorrido.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.quad });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.cubic(t: number): number`

Função cúbica, `f(t) = t * t * t`. A posição é igual ao cubo do tempo decorrido.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.cubic });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.poly(n: number): EasingFunction`

Cria uma função de potência. A posição é igual à N-ésima potência do tempo decorrido.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.poly(3) });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.sin(t: number): number`

Função sinusoidal.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.sin });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.circle(t: number): number`

Função circular.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.circle });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.exp(t: number): number`

Função exponencial.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.exp });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.elastic(bounciness?: number): EasingFunction`

Cria uma interação elástica simples, como uma mola oscilando. O `bounciness` (elasticidade) padrão é 1. Um valor 0 não ultrapassa o limite, e um valor N > 1 ultrapassará o limite aproximadamente N vezes.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.elastic(1.5) });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.back(s?: number): EasingFunction`

Cria um efeito onde o objeto recua um pouco antes de avançar.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.back(2) });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.bounce(t: number): number`

Fornece um efeito de "quicar" simples.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.bounce });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.bezier(x1: number, y1: number, x2: number, y2: number): { factory: () => EasingFunction; }`

Cria uma curva de Bézier cúbica, equivalente à `transition-timing-function` do CSS.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.bezier(0.25, 0.1, 0.25, 1).factory() });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.bezierFn(x1: number, y1: number, x2: number, y2: number): EasingFunction`

A implementação base para a curva de Bézier cúbica.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.bezierFn(0.25, 0.1, 0.25, 1) });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.in(easing: EasingFunction): EasingFunction`

Modificador que executa uma função de easing na sua forma original (aceleração no início).

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.in(Easing.sin) });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.out(easing: EasingFunction): EasingFunction`

Modificador que executa uma função de easing de forma invertida (desaceleração no final).

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.out(Easing.sin) });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.inOut(easing: EasingFunction): EasingFunction`

Modificador que torna qualquer função de easing simétrica. A função acelera na primeira metade da duração e desacelera na segunda metade.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.inOut(Easing.sin) });

  console.log("Animação concluída!");
});

animation.start();
```

### `Easing.steps(n?: number, roundToNextStep?: boolean | undefined): EasingFunction`

Cria uma função de easing que avança em degraus discretos.

**Exemplo:**
```typescript
import { Animation, Easing } from '@ismael1361/utils';

const animation = Animation.create(function* (state) {
  // Anima o valor 'progress' de 0 para 1 em 1 segundo.
  yield* Animation.timing(state.progress, { to: 1, duration: 1000, easing: Easing.steps(5) });

  console.log("Animação concluída!");
});

animation.start();
```

---


## SharedValue

### `class SharedValue<T>`

Uma classe que encapsula um valor, permitindo que ele seja "observável" e reativo. Emite eventos quando seu valor é alterado, sendo a base para a reatividade nas animações.

**Exemplo:**
```typescript
import { SharedValue } from '@ismael1361/utils';

const opacity = new SharedValue(0);

opacity.on('change', (newValue) => {
  console.log(`A opacidade mudou para: ${newValue}`);
});

// Define um novo valor, o que dispara o evento 'change'
opacity.value = 0.5; // Logs: "A opacidade mudou para: 0.5"

// Acessa o valor atual
console.log(opacity.value); // Logs: 0.5

// Reseta para o valor inicial
opacity.clear(); // Logs: "A opacidade mudou para: 0"
console.log(opacity.value); // Logs: 0
```

---

#### Propriedades da Instância

##### `.value: T`

Obtém o valor atual. Define um novo valor. Se o novo valor for diferente do atual, emite os eventos 'value' e 'change'.

##### `.on(event: 'value' | 'change', callback: (value: T) => void): void`

Adiciona um listener para um evento especifico.

##### `.off(event: 'value' | 'change', callback: (value: T) => void): void`

Remove um listener para um evento especifico.

##### `.clear(): void`

Limpa o valor e emite o evento 'change' com o valor inicial.

---

### `class SharedValues<S>`

Gerencia um grupo de instâncias de `SharedValue` como um único objeto de estado.

Esta classe é um contêiner que pega um objeto de estado inicial e cria um `SharedValue` para cada uma de suas propriedades. Ela observa mudanças em qualquer um dos valores internos e emite eventos agregados, tornando mais fácil reagir a mudanças no estado geral da animação.

**Exemplo:**
```typescript
import { SharedValues } from '@ismael1361/utils';

const stateManager = new SharedValues({ x: 0, y: 100, opacity: 1 });

// Ouve por mudanças em qualquer propriedade.
// O evento é otimizado com requestAnimationFrame.
stateManager.on('change', (newState) => {
  console.log('O estado completo mudou:', newState);
  // Exemplo de saída: { x: 50, y: 100, opacity: 1 }
});

// Ouve por mudanças em uma propriedade específica.
stateManager.on('value', (key, value) => {
   console.log(`A propriedade '${key}' mudou para ${value}`);
   // Exemplo de saída: "A propriedade 'x' mudou para 50"
});

// Modifica um valor individual, o que dispara os eventos.
stateManager.current.x.value = 50;

// Obtém um snapshot dos valores atuais.
console.log(stateManager.values); // { x: 50, y: 100, opacity: 1 }

// Reseta todos os valores para o estado inicial.
stateManager.clear();
console.log(stateManager.values); // { x: 0, y: 100, opacity: 1 }
```

---

#### Propriedades da Instância

##### `.values: S`

Obtém um snapshot dos valores atuais.

##### `.current: { [K in keyof S]: SharedValue<S[K]>; }`

Obtém um objeto com instâncias de `SharedValue` para cada propriedade do estado.

##### `.on(event: 'value' | 'change', callback: (key: keyof S, value: S[keyof S]) => void): void`

Adiciona um listener para um evento especifico.

##### `.off(event: 'value' | 'change', callback: (key: keyof S, value: S[keyof S]) => void): void`

Remove um listener para um evento especifico.

##### `.initialize(): void`

Reinicia todos os valores para o estado inicial.

##### `.destroy(): void`

Limpa todos os listeners.

##### `.clear(): void`

Limpa todos os valores e emite o evento 'change' com o estado inicial.

### `sharedValues: <S>(state: S) => SharedValues<S>`

Função de fábrica para criar e retornar uma nova instância de `SharedValues`.

É um atalho conveniente para `new SharedValues(state)`.

**Exemplo:**
```typescript
import { sharedValues } from '@ismael1361/utils';

const initialState = {
  x: 0,
  y: 0,
};

const position = sharedValues(initialState);

position.on('change', (newPosition) => {
  console.log('Nova posição:', newPosition);
  // => Nova posição: { x: 10, y: 0 }
});

// Modifica um valor individual
position.current.x.value = 10;

// Obtém um snapshot dos valores atuais
console.log(position.values); // { x: 10, y: 0 }
```