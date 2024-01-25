
## Tipos de Teste de Performace
### Smoke Testing
O Smoke Testing, também conhecido como teste de sanidade, é uma estratégia de teste de software que compreende um conjunto não exaustivo de testes que visam assegurar que as funcionalidades mais importantes de um sistema estão funcionando corretamente. O objetivo é identificar problemas graves que possam impedir o progresso do desenvolvimento ou do teste.

No contexto do k6, uma ferramenta de teste de carga de código aberto, o Smoke Testing pode ser usado para verificar se um sistema pode suportar a carga mínima esperada.

Aqui está um exemplo de como você pode configurar um Smoke Test no k6:
```javascript
import http from 'k6/http';
import { sleep } from 'k6';

export let options = {
    vus: 1, // 1 usuário virtual
    duration: '1m' // duração do teste de 1 minuto
};

export default function () {
    http.get('http://test.k6.io');
    sleep(1);
}
```

Neste exemplo, estamos simulando um único usuário virtual (vus: 1) acessando o site http://test.k6.io por um minuto (duration: '1m'). Este é um exemplo simples de um Smoke Test, onde estamos apenas verificando se o site pode lidar com uma carga mínima.

## Performace Testing

O Performance Testing é uma estratégia de teste de software que visa determinar a velocidade, responsividade e estabilidade de um sistema sob uma carga de trabalho específica. O objetivo é identificar e eliminar quaisquer gargalos de desempenho para garantir que o sistema possa atender aos requisitos de desempenho.

Quantidade de Tráfego: É essencial determinar a quantidade de tráfego que o sistema deve suportar durante o teste. Isso pode ser definido através do número de usuários virtuais (VUs) e suas interações com o sistema.

Condições Normais e de Pico: Os testes de desempenho devem ser realizados em condições normais de uso, bem como em condições de pico. Isso significa simular tanto a carga média esperada quanto a carga máxima que o sistema pode enfrentar.

Garantir Funcionamento: O objetivo dos testes de desempenho é garantir que o sistema funcione corretamente sob carga. Isso envolve verificar se todas as funcionalidades estão operando conforme o esperado e se não há problemas de desempenho que possam afetar a experiência do usuário.

``` javascript
export let options = {
    stages: [
        { duration: '5m', target: 20 }, // simula 20 usuários virtuais por 30 segundos
        { duration: '10m', target: 100 }, // aumenta para 100 usuários virtuais ao longo de 1 minuto
        { duration: '5m', target: 0 }, // reduz para 0 usuários
    ],
};

export default function () {
    http.get('http://test.k6.io');
    sleep(1);
}
```

Neste exemplo, estamos simulando um aumento gradual de usuários virtuais acessando o site http://test.k6.io. Começamos com 20 usuários por 30 segundos, aumentamos para 100 usuários ao longo de 1 minuto e, em seguida, reduzimos para 0 usuários. Este é um exemplo de um Performance Test, onde estamos verificando como o site se comporta sob diferentes cargas de trabalho.


## Stress Testing
O `Stress Testing` é uma estratégia de teste que visa avaliar a robustez de um sistema sob condições extremas. O objetivo é identificar o ponto de ruptura do sistema e garantir que ele possa se recuperar após um estresse extremo. 

No contexto do k6, uma ferramenta de teste de carga de código aberto, o `Stress Testing` pode ser usado para responder às seguintes perguntas:

1. **Como seu sistema se comporta em condições extremas?** O `Stress Testing` pode ajudar a identificar como o sistema se comporta sob cargas de trabalho extremamente altas, como um grande número de usuários simultâneos.

2. **Qual é a capacidade máxima do seu sistema em termos de usuários ou taxa de transferência?** O `Stress Testing` pode ajudar a determinar o número máximo de usuários que o sistema pode suportar antes de começar a apresentar problemas de desempenho.

3. **O ponto de ruptura do seu sistema?** O `Stress Testing` pode ajudar a identificar o ponto em que o sistema deixa de funcionar corretamente, seja em termos de número de usuários, taxa de transferência ou outro indicador de desempenho.

4. **O sistema se recupera sem intervenção manual após o término do teste de estresse?** Após o `Stress Testing`, é importante verificar se o sistema pode se recuperar e voltar ao normal sem a necessidade de intervenção manual.

Aqui está um exemplo de como você pode configurar um `Stress Test` no k6:

```javascript
export let options = {
    stages: [
        { duration: '2m', target: 100 }, // simula 100 usuários virtuais por 2 minutos
        { duration: '5m', target: 200 }, // aumenta para 200 usuários virtuais ao longo de 5 minutos, 
        { duration: '2m', target: 100 }, // aumenta para 100 usuários virtuais ao longo de 2 minutos, 
        { duration: '2m', target: 500 }, // aumenta para 500 usuários virtuais ao longo de 2 minutos, 
        { duration: '5m', target: 1000 }, // aumenta para 1000 usuários virtuais ao longo de 5 minutos, 
        { duration: '5m', target: 400 }, // aumenta para 400 usuários virtuais ao longo de 5 minutos, 
        { duration: '10m', target: 0 }, // reduz para 0 usuários virtuais ao longo de 10 minutos, 
    ],
};

export default function () {
    http.get('http://test.k6.io');
    sleep(1);
}
```
Neste exemplo, estamos simulando um aumento variado de usuários virtuais acessando o site http://test.k6.io. Começamos com 100 usuários por 2 minutos, aumentamos para 200 usuários ao longo de 5 minutos, voltamos a 100 usuários por mais 2 minutos, depois aumentamos para 500 usuários por 2 minutos, aumentamos ainda mais para 1000 usuários ao longo de 5 minutos, reduzimos para 400 usuários por 5 minutos e, finalmente, reduzimos para 0 usuários ao longo de 10 minutos. Este é um exemplo de um Stress Test, onde estamos verificando como o site se comporta sob condições variadas e extremas.


## Soak Testing

O Soak Testing, também conhecido como teste de resistência, é uma estratégia de teste que visa avaliar a confiabilidade e a estabilidade de um sistema durante um longo período de tempo. O objetivo é identificar problemas que podem não ser evidentes em um uso normal, mas que podem surgir durante um uso prolongado, como vazamentos de memória ou problemas de desempenho.

No contexto do k6, uma ferramenta de teste de carga de código aberto, o Soak Testing pode ser usado para simular um uso contínuo e intenso do sistema.

Aqui está um exemplo de como você pode configurar um Soak Test no k6:
``` javascript
export const options = {
    stages: [
        { duration: '2m', target: 400}, // aumenta para 400 usuários virtuais em 2 minutos
        { duration: '3h56m', target: 400}, // mantém 400 usuários virtuais por quase 4 horas
        { duration: '2m', target: 0}, // reduz para 0 usuários em 2 minutos
    ]
}
```

Neste exemplo, estamos simulando 400 usuários virtuais acessando o site de forma contínua por quase 4 horas. Este é um exemplo de um Soak Test, onde estamos verificando a confiabilidade do sistema durante um longo período de tempo.

## Ciclo de Vida do Teste
O ciclo de vida de um teste no k6 é composto por quatro fases principais:

```javascript
 // 1. Inicialização
 // 2. Configuração
 // 3. Execução
 // 4. Desmontagem
```

#### Inicialização
A fase de inicialização é responsável por carregar arquivos locais e realizar chamadas de módulos. Esta fase é executada apenas uma vez.

```javascript
 // 1. Inicialização
 import sleep from 'k6';
 import http from 'k6/http';
```

#### Configuração
A fase de configuração é onde os dados são compartilhados entre todos os usuários virtuais (VUs). Esta fase também é chamada apenas uma vez.

```javascript
 // 2. Configuração
export const options = {
    vus: 20,
    duration: '10s'
}
```

#### Execução 
A fase de execução é onde os testes realmente ocorrem. Cada VU executa a função default de forma independente.

```javascript
 // 3. Execução
export default function(){
    console.log('Testando K6');
    sleep(1);
}
```

#### Desmontagem
A fase de desmontagem ocorre após a conclusão dos testes. Neste momento, você pode estar enviando dados para aplicações externas ou realizando outras operações de limpeza.

```javascript
 // 4. Desmontagem
 export function teardown(data){
    console.log(data)
 }
```

## Realizando Chamadas HTTP
Para realizar chamadas HTTP com o k6, é necessário importar o módulo `http`, que é um módulo nativo do k6 para a execução de requisições HTTP.

```javascript
import http from 'k6/http';

export default function (){
    http.get('http://test.k6.io');
}
```

Para executar o teste, use o comando `k6 run nome_do_arquivo.js`. Isso deve retornar uma mensagem no console com os resultados do teste, incluindo várias métricas sobre as solicitações HTTP realizadas.

```powershell
  execution: local
     script: realizacao_chamadas_http.js
     output: -

  scenarios: (100.00%) 1 scenario, 1 max VUs, 10m30s max duration (incl. graceful stop):
           * default: 1 iterations for each of 1 VUs (maxDuration: 10m0s, gracefulStop: 30s)

     data_received..................: 17 kB 25 kB/s
     data_sent......................: 542 B 779 B/s
     http_req_blocked...............: avg=219.33ms min=163.71ms med=219.33ms max=274.94ms p(90)=263.82ms p(95)=269.38ms
     http_req_connecting............: avg=128.22ms min=127.71ms med=128.22ms max=128.73ms p(90)=128.62ms p(95)=128.68ms
     http_req_duration..............: avg=128.37ms min=126.59ms med=128.37ms max=130.14ms p(90)=129.78ms p(95)=129.96ms
       { expected_response:true }...: avg=128.37ms min=126.59ms med=128.37ms max=130.14ms p(90)=129.78ms p(95)=129.96ms
     http_req_failed................: 0.00% ✓ 0        ✗ 2
     http_req_receiving.............: avg=790.45µs min=717.7µs  med=790.45µs max=863.2µs  p(90)=848.65µs p(95)=855.92µs
     http_req_sending...............: avg=0s       min=0s       med=0s       max=0s       p(90)=0s       p(95)=0s
     http_req_tls_handshaking.......: avg=73.1ms   min=0s       med=73.1ms   max=146.21ms p(90)=131.59ms p(95)=138.9ms
     http_req_waiting...............: avg=127.58ms min=125.73ms med=127.58ms max=129.42ms p(90)=129.05ms p(95)=129.24ms
     http_reqs......................: 2     2.876016/s
     iteration_duration.............: avg=695.4ms  min=695.4ms  med=695.4ms  max=695.4ms  p(90)=695.4ms  p(95)=695.4ms
     iterations.....................: 1     1.438008/s
```

Vale ressaltar que, se você executar o teste sem fornecer configurações de número de usuários, o k6 executará o teste apenas uma vez, com 1 usuário virtual.

## Verificações (Checks)
As verificações no k6 são usadas para validar expressões booleanas durante a execução dos testes. Elas são úteis para garantir que as respostas das requisições HTTP estejam conforme o esperado.

```javascript
import http from 'k6/http';
import { check } from 'k6';

export const options = {
    vus: 1,
    duration: '5s'
}

export default function (){
    const res = http.get('http://test.k6.io');
    check(res, {
       'status is 200': (r) => r.status === 200
    });
}
```

Ao executar o teste, você verá uma saída semelhante a esta:

```powershell
  execution: local
     script: realizacao_chamadas_http.js
     output: -

  scenarios: (100.00%) 1 scenario, 1 max VUs, 35s max duration (incl. graceful stop):
           * default: 1 looping VUs for 5s (gracefulStop: 30s)

     ✓ status is 200

     checks.........................: 100.00% ✓ 18       ✗ 0
     data_received..................: 219 kB  44 kB/s
     data_sent......................: 4.0 kB  792 B/s
     http_req_blocked...............: avg=11.34ms  min=0s       med=0s       max=274.99ms p(90)=0s       p(95)=33.32ms
     http_req_connecting............: avg=7.15ms   min=0s       med=0s       max=129.3ms  p(90)=0s       p(95)=32.1ms
     http_req_duration..............: avg=127.88ms min=126.72ms med=127.88ms max=129.12ms p(90)=128.71ms p(95)=128.82ms
       { expected_response:true }...: avg=127.88ms min=126.72ms med=127.88ms max=129.12ms p(90)=128.71ms p(95)=128.82ms
     http_req_failed................: 0.00%   ✓ 0        ✗ 36
     http_req_receiving.............: avg=226.05µs min=0s       med=0s       max=1ms      p(90)=847.85µs p(95)=997.67µs
     http_req_sending...............: avg=24.63µs  min=0s       med=0s       max=874.1µs  p(90)=0s       p(95)=3.2µs
     http_req_tls_handshaking.......: avg=4.07ms   min=0s       med=0s       max=146.58ms p(90)=0s       p(95)=0s
     http_req_waiting...............: avg=127.63ms min=126.43ms med=127.63ms max=128.72ms p(90)=128.35ms p(95)=128.42ms
     http_reqs......................: 36      7.169725/s
     iteration_duration.............: avg=278.94ms min=255.44ms med=256.17ms max=665.09ms p(90)=257.29ms p(95)=318.73ms
     iterations.....................: 18      3.584862/s
     vus............................: 1       min=1      max=1
     vus_max........................: 1       min=1      max=1

running (05.0s), 0/1 VUs, 18 complete and 0 interrupted iterations
default ✓ [======================================] 1 VUs  5s
```

Neste exemplo, a verificação `'status is 200'` garante que o status da resposta HTTP seja 200. Se a verificação falhar, o k6 registrará o erro.

## Métricas
O k6 fornece várias métricas integradas que ajudam a entender o desempenho do seu teste. As métricas são classificadas em quatro tipos:

1. **Contadores**: Realizam somas e incrementos.
2. **Medidores (Gauges)**: Usados para rastrear os valores mínimo, máximo e mais recente.
3. **Taxas (Rates)**: Rastreiam a frequência com a qual um valor diferente de zero ocorre.
4. **Tendências (Trends)**: Usadas para calcular a tendência probabilística, incluindo moda, média e mediana.

Abaixo, temos exemplos de métricas exibidas ao final de um teste:

```powershell
  execution: local
     script: realizacao_chamadas_http.js
     output: -

  scenarios: (100.00%) 1 scenario, 1 max VUs, 35s max duration (incl. graceful stop):
           * default: 1 looping VUs for 5s (gracefulStop: 30s)

     ✓ status is 200

     checks.........................: 100.00% ✓ 18       ✗ 0
     data_received..................: 219 kB  44 kB/s
     data_sent......................: 4.0 kB  792 B/s
     http_req_blocked...............: avg=11.34ms  min=0s       med=0s       max=274.99ms p(90)=0s       p(95)=33.32ms
     http_req_connecting............: avg=7.15ms   min=0s       med=0s       max=129.3ms  p(90)=0s       p(95)=32.1ms
     http_req_duration..............: avg=127.88ms min=126.72ms med=127.88ms max=129.12ms p(90)=128.71ms p(95)=128.82ms
       { expected_response:true }...: avg=127.88ms min=126.72ms med=127.88ms max=129.12ms p(90)=128.71ms p(95)=128.82ms
     http_req_failed................: 0.00%   ✓ 0        ✗ 36
     http_req_receiving.............: avg=226.05µs min=0s       med=0s       max=1ms      p(90)=847.85µs p(95)=997.67µs
     http_req_sending...............: avg=24.63µs  min=0s       med=0s       max=874.1µs  p(90)=0s       p(95)=3.2µs
     http_req_tls_handshaking.......: avg=4.07ms   min=0s       med=0s       max=146.58ms p(90)=0s       p(95)=0s
     http_req_waiting...............: avg=127.63ms min=126.43ms med=127.63ms max=128.72ms p(90)=128.35ms p(95)=128.42ms
     http_reqs......................: 36      7.169725/s
     iteration_duration.............: avg=278.94ms min=255.44ms med=256.17ms max=665.09ms p(90)=257.29ms p(95)=318.73ms
     iterations.....................: 18      3.584862/s
     vus............................: 1       min=1      max=1
     vus_max........................: 1       min=1      max=1

running (05.0s), 0/1 VUs, 18 complete and 0 interrupted iterations
default ✓ [======================================] 1 VUs  5s
```

Essas métricas fornecem uma visão detalhada do desempenho do seu teste, incluindo a quantidade de dados recebidos/enviados, a duração das requisições HTTP, a taxa de falhas e muito mais.
