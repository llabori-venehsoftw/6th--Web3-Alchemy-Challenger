# Solution al desafio #6 Universidad de Alchemy

Objetivos:

En este tutorial aprenderemos los siguientes elementos b谩sicos para el staking:

1.  Construyendo con Scaffold-Eth
    - Hacking juntos frontends
    - Creaci贸n de "backends" Solidity
2.  Transferencia de ETH de monederos a contratos inteligentes y viceversa
3.  Utilizaci贸n de modificadores de Solidity

Empecemos por entender c贸mo funciona Scaffold-Eth.

## Iniciando 馃殌

Estas instrucciones te permitir谩n tener una copia del proyecto corriendo en tu m谩quina local para
prop贸sitos de desarrollo y pruebas.

Ver **Despliegue** para saber c贸mo desplegar el proyecto.

### Prerrequisitos 馃搵

Descargar Scaffold-Eth

En este tutorial, vamos a utilizar el entorno de desarrollo Scaffold-Eth para elaborar nuestros 
contratos inteligentes y armar nuestra interfaz de usuario frontend.

    馃摌

    隆Antes de empezar, quiero transmitir algunos detalles importantes a tener en cuenta!

    Scaffold-Eth es impresionante en la abstracci贸n de las configuraciones del entorno y las dependencias
    de front-end que hace que una herramienta poderosa.

    Si bien hay un mont贸n de funcionalidades que son manejados por Scaffold-Eth de forma autom谩tica, es
    importante es importante sumergirse en el c贸digo para entender c贸mo algunas de estas caracter铆sticas se generan cuando se tiene una m谩s s贸lida del entorno de desarrollo en su conjunto.

Vamos a empezar por bifurcar el repositorio de c贸digo base de [Challenge #1](https://speedrunethereum.com/challenge/decentralized-staking) de [SpeedRunEthereum](https://speedrunethereum.com/):

```
git clone https://github.com/scaffold-eth/scaffold-eth-challenges.git 6-StakingApp

cd 6-StakingApp

git checkout challenge-1-decentralized-staking

yarn install
```

Si usted ha seguido a lo largo de con 茅xito, usted ser谩 capaz de una nueva carpeta titulada 6-StakingApp 
en su base de base.

Despu茅s de ejecutar los comandos anteriores, nos queda una gran carpeta llena de archivos.

Incluso antes de pasar al c贸digo, deber铆amos familiarizarnos con d贸nde se almacenan los archivos clave 
en Scaffold-Eth para que sepamos d贸nde centrarnos.
 
![Alt text](https://www.github.com/assets.digitalocean.com/articles/alligator/boo.svg "un t铆tulo")

En este tutorial, trabajaremos principalmente con Staker.sol y App.jsx. 
 

### Instalaci贸n 馃敡

A continuaci贸n, necesitar谩s tres terminales separadas para los tres comandos siguientes:

Inicia tu frontend React:

```
yarn start
```
Veriamos algo similar a:

```
Compiled successfully!

You can now view @scaffold-eth/react-app in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://192.168.125.128:3000

Note that the development build is not optimized.
To create a production build, use npm run build.
```

Inicia tu backend Hardhat:

```
yarn chain
```
Veriamos algo similar a:

```
llabori@Xubuntu64Bits-virtual-machine:~/BlockChains/AlchemyUniversity/6-StakingApp$ yarn chain
yarn run v1.22.19
$ yarn workspace @scaffold-eth/hardhat chain
$ hardhat node --network hardhat --no-deploy
Started HTTP and WebSocket JSON-RPC server at http://127.0.0.1:8545/

web3_clientVersion
eth_chainId
eth_accounts
eth_chainId
eth_accounts
eth_chainId
eth_estimateGas
eth_chainId
eth_getTransactionCount
eth_blockNumber
eth_chainId
eth_feeHistory
eth_sendTransaction
  Contract deployment: <UnrecognizedContract>
  Contract address:    0x5fbdb2315678afecb367f032d93f642f64180aa3
  Transaction:         0x969048d3d9d28fe79e4acc6c4a78e29e962d37301144c6ab104af437fd7915bf
  From:                0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266
  Value:               0 ETH
  Gas used:            87965 of 87965
  Block #1:            0xc0ae22a97dfc48578cd35a7367af67492a0aaafa3670817cd37b6fd0b73df901

eth_chainId
eth_getTransactionByHash
eth_chainId
eth_getTransactionReceipt
eth_accounts
eth_chainId (2)
eth_estimateGas
eth_chainId
eth_getTransactionCount
eth_feeHistory
eth_sendTransaction
  Contract deployment: <UnrecognizedContract>
  Contract address:    0xe7f1725e7734ce288f8367e1bb143e90bb3f0512
  Transaction:         0xa9dbae5562a285cc3846ac44d75a53f5317aa0d62fee48364e7126dbaf8bfcbf
  From:                0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266
  Value:               0 ETH
  Gas used:            109206 of 109206
  Block #2:            0x454dbbfbe9c2e5639dc32e2082a0efdb9db8f246e9e030e4556b36f57f8a1755

eth_chainId
eth_getTransactionByHash
eth_chainId
eth_getTransactionReceipt
```

Compila, despliega y publica todos los contratos en tu archivo packages/contracts:

```
yarn deploy
```
Veriamos algo similar a:

```
llabori@Xubuntu64Bits-virtual-machine:~/BlockChains/AlchemyUniversity/6-StakingApp$ yarn deploy
yarn run v1.22.19
$ yarn workspace @scaffold-eth/hardhat deploy
$ hardhat deploy --export-all ../react-app/src/contracts/hardhat_contracts.json
Downloading compiler 0.8.4
Compiling 3 files with 0.8.4
Compilation finished successfully
deploying "ExampleExternalContract" (tx: 0x969048d3d9d28fe79e4acc6c4a78e29e962d37301144c6ab104af437fd7915bf)...: deployed at 0x5FbDB2315678afecb367f032d93F642f64180aa3 with 87965 gas
deploying "Staker" (tx: 0xa9dbae5562a285cc3846ac44d75a53f5317aa0d62fee48364e7126dbaf8bfcbf)...: deployed at 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512 with 109206 gas
$ hardhat run scripts/publish.js
鉁?  Published contracts to the subgraph package.
Done in 130.58s.
```


    Cada vez que actualices tus contratos, ejecuta yarn deploy --reset para "refrescar" tus contratos en Scaffold-Eth.

Muy bien. Ahora deber铆as poder ver la interfaz de usuario de este repositorio en http://localhost:3000/


_Familiar铆zate con Scaffold-Eth_
 
Aunque s茅 que te mueres de ganas de empezar con el c贸digo, 隆s贸lo hay que ocuparse de unos pocos detalles 
m谩s!

En nuestra vista por defecto, tenemos dos pesta帽as- Staker UI & Debug Contracts. 


![Alt text](https://www.github.com/assets.digitalocean.com/articles/alligator/boo.svg "un t铆tulo")


Vaya adelante y cambie hacia adelante y hacia atr谩s en su frontend para echar un vistazo a las diferentes caracter铆sticas.

Staker UI contiene todos los componentes frontales con los que interactuaremos principalmente.

Si haces clic en los botones proporcionados, te dar谩s cuenta de que la mayor铆a de ellos no est谩n del todo 
conectados todav铆a e inmediatamente te encontrar谩s con errores.

    馃摌

    Echa un vistazo a Staker UI. 隆Te dar谩s cuenta de que es dolorosamente espartano! Eso es lo que vamos a eliminar.

Desde cualquier interacci贸n en la cadena de Ethereum requiere testnet ETH, necesitar谩 testnet local ETH 
para empezar a hackear lejos.

Primero, coge la direcci贸n de tu cartera localhost.

Haga clic en el bot贸n "Copiar" en la esquina superior derecha


![Alt text](https://www.github.com/assets.digitalocean.com/articles/alligator/boo.svg "un t铆tulo")


A continuaci贸n, dir铆gete a la esquina inferior izquierda. Aqu铆 podr谩s acceder al grifo local.

    Copia y pega tu direcci贸n en el campo abierto o haz clic en el icono "Cartera".
    Pega tu direcci贸n en la vista ampliada
    Env铆ate ETH de prueba


![Alt text](https://www.github.com/assets.digitalocean.com/articles/alligator/boo.svg "un t铆tulo")


Despu茅s de recargar tu monedero local, 隆podr谩s interactuar con tus contratos!

La segunda pesta帽a, Depurar Contratos, es otra pantalla de frontend que contiene uno de los superpoderes 
de Scaffold-Eth.

Una vez que despliegues tus contratos y lo configures para leer los datos del contrato correctamente, 
generar谩 autom谩ticamente una interfaz de usuario b谩sica que te permitir谩 interactuar con las funciones de 
tu contrato.

Por ejemplo, en el ejemplo de abajo, podemos leer y escribir informaci贸n a trav茅s de nuestro contrato 
inteligente simplemente introduciendo par谩metros y haciendo clic en "Enviar". Con Scaffold-Eth, no 
necesitamos usar solo comandos CLI y tenemos una forma m谩s intuitiva de crear prototipos.

    馃摌

    Si quieres almacenar y ver una variable en el Debug Contractstab, 隆aseg煤rate de establecer la variable como p煤blica!

隆Impresionante!

Ahora que estamos familiarizados con Scaffold-Eth, podemos sumergirnos m谩s profundamente en el c贸digo.

Mirando nuestro archivo Staker.sol, vemos que tenemos un archivo Solidity bastante vac铆o con un mont贸n de 
comentarios que dictan lo que hay que rellenar.

Dado que el tutorial Road to Web3 (R2W3) se desv铆a del Desaf铆o n潞 1 de Scaffold-Eth, podemos seguir 
adelante e ignorar los comentarios y empezar con el siguiente c贸digo.

```
pragma solidity >=0.6.0 <0.7.0;

import "hardhat/console.sol";
import "./ExampleExternalContract.sol";

contract Staker {
  ExampleExternalContract public exampleExternalContract;
  
  constructor(address exampleExternalContractAddress) public {
      exampleExternalContract = ExampleExternalContract(exampleExternalContractAddress);
  }
  
}
```

Par谩metros del proyecto

Antes de escribir el c贸digo de nuestro contrato inteligente, vamos a repasar c贸mo esperamos que funcione 
nuestra dApp de apuestas.

    Para simplificar, s贸lo esperamos que un 煤nico usuario interact煤e con nuestra dApp de apuestas.
    Tenemos que ser capaces de depositar y retirar del contrato Staker
        Apostar es una acci贸n de un solo uso, lo que significa que una vez que apostamos no podemos volver 
        a apostar de nuevo.
        Las retiradas del contrato eliminan todo el saldo principal y cualquier inter茅s acumulado
    El contrato Staker tiene una tasa de pago de intereses de 0,1 ETH por cada segundo que el ETH 
    depositado es elegible para el devengo de intereses
    Al desplegar el contrato, el contrato Staker debe comenzar con 2 contadores de tiempo. El primer plazo 
    debe establecerse en 2 minutos y el segundo en 4 minutos
        El plazo de 2 minutos dicta el periodo en el que el usuario de staker puede depositar fondos. 
        (Entre t=0 minutos y t=2 minutos, el usuario puede depositar)
        Todos los bloqueos que tengan lugar entre el dep贸sito de fondos y el plazo de 2 minutos son 
        v谩lidos para la acumulaci贸n de intereses.
        Una vez transcurrido el plazo de 2 minutos para retirar fondos, el usuario puede retirar la 
        totalidad del saldo principal y los intereses devengados hasta que se cumpla el plazo de 4 minutos.
        Una vez transcurrido el plazo adicional de 2 minutos para retiradas, el usuario no podr谩 retirar 
        sus fondos desde que se agot贸 el plazo.
    Si a un usuario le quedan fondos, tenemos una 煤ltima funci贸n a la que podemos llamar para "bloquear" 
    los fondos en un contrato externo que ya est谩 preinstalado en nuestro entorno Scaffold-Eth, 
    ExampleExternalContract.sol


    馃摌

    Aunque los par谩metros de staking enumerados anteriormente pueden parecer un poco enrevesados, muchas 
    dApps de staking de la vida real presentan primitivas similares en las que los usuarios tienen un 
    periodo limitado para realizar dep贸sitos y retiradas.

    Adem谩s, muchas dApps desincentivan el capital "improductivo" que simplemente se queda ah铆 despu茅s de 
    que los periodos de espera hayan terminado.

    A veces, el protocolo DeFi puede incluso absorber los dep贸sitos pendientes despu茅s de que finalice un 
    periodo de espera similar al 煤ltimo par谩metro que indicamos en nuestro tutorial.


Mapas de Solidity

En nuestro contrato inteligente, necesitaremos dos mapeos que nos ayuden a almacenar algunos datos.

En particular, necesitamos algo para hacer un seguimiento de

    cu谩nto ETH se deposita en el contrato
    la hora a la que se produjo el dep贸sito

Podemos lograr esto con:

```
mapping(address => uint256) public balances; 
mapping(address => uint256) public depositTimestamps;
```

Variables p煤blicas

De acuerdo con las directrices descritas anteriormente, tambi茅n necesitaremos un pu帽ado de variables 
diferentes.

```
uint256 public constant rewardRatePerSecond = 0.1 ether; 
uint256 public withdrawalDeadline = block.timestamp + 120 seconds; 
uint256 public claimDeadline = block.timestamp + 240 seconds; 
uint256 public currentBlock = 0;
```

La tasa de recompensa establece la tasa de inter茅s para el desembolso de ETH sobre la cantidad principal 
apostada.

Los plazos de retirada y reclamaci贸n nos ayudan a establecer los plazos para que comience/finalice la 
mec谩nica de apuesta.

Y, por 煤ltimo, tenemos una variable que utilizamos para guardar el bloque actual.

    馃摌

    Usamos block.timestamp + XXX segundos para crear plazos exactamente XXX segundos despu茅s de que se 
    inicie nuestro contrato. Definitivamente es un poco "ingenuo" como mecanismo de temporizaci贸n; 驴se te 
    ocurre alguna forma mejor de implementarlo para que sea m谩s generalizable, por ejemplo?

Eventos

Aunque no enviemos eventos a nuestro frontend, deber铆amos asegurarnos de emitirlos en partes clave de 
nuestro contrato para garantizar que mantenemos las mejores pr谩cticas de programaci贸n.

```
event Stake(address indexed sender, uint256 amount); 
event Received(address, uint); 
event Execute(address indexed sender, uint256 amount);
```

Ahora que ya tenemos los par谩metros/variables clave, podemos pasar a crear las funciones espec铆ficas que 
utilizaremos en nuestro tutorial.

Funciones de tiempo READ ONLY

Como se indica en los par谩metros del proyecto, muchas de las diferentes funcionalidades de la dApp de 
estaca est谩n sujetas a "bloqueos de tiempo" que permiten/proh铆ben ciertas acciones en determinados 
momentos.

Aqu铆 tenemos dos funciones diferentes que rigen el inicio y el final de la ventana de retirada.

```
  function withdrawalTimeLeft() public view returns (uint256 withdrawalTimeLeft) {
    if( block.timestamp >= withdrawalDeadline) {
      return (0);
    } else {
      return (withdrawalDeadline - block.timestamp);
    }
  }

  function claimPeriodLeft() public view returns (uint256 claimPeriodLeft) {
    if( block.timestamp >= claimDeadline) {
      return (0);
    } else {
      return (claimDeadline - block.timestamp);
    }
  }
```

Ambas funciones tienen un dise帽o muy familiar.

Ambas presentan una sentencia if -> else est谩ndar.

La condicional simplemente comprueba si la hora actual es mayor o menor que los plazos dictados en la 
secci贸n de variables p煤blicas.

Si el tiempo actual es mayor que los plazos preestablecidos, sabemos que el plazo ha pasado y devolvemos 
0 para significar que se ha producido un "cambio de estado".

En caso contrario, simplemente devolvemos el tiempo restante antes de que se alcance la fecha l铆mite.

Modificadores

Para ver un ejemplo m谩s detallado de un modificador, consulte [Solidity By Example](https://solidity-by-example.org/function-modifier).

A grandes rasgos, los modificadores Solidity son piezas de c贸digo que pueden ejecutarse antes y/o despu茅s 
de una llamada a una funci贸n.

Aunque tienen muchos prop贸sitos diferentes, uno de los casos de uso m谩s comunes y b谩sicos es restringir 
el acceso a ciertas funciones si se dan determinadas condiciones. 
acceso a determinadas funciones si no se cumplen todas las condiciones.

En este tutorial, utilizaremos precisamente modificadores para ayudar a bloquear funciones clave que 
dictan nuestra participaci贸n, retirada y repatriaci贸n.

Aqu铆 est谩n los tres modificadores que utilizamos:

```
  modifier withdrawalDeadlineReached( bool requireReached ) {
    uint256 timeRemaining = withdrawalTimeLeft();
    if( requireReached ) {
      require(timeRemaining == 0, "Withdrawal period is not reached yet");
    } else {
      require(timeRemaining > 0, "Withdrawal period has been reached");
    }
    _;
  }

  modifier claimDeadlineReached( bool requireReached ) {
    uint256 timeRemaining = claimPeriodLeft();
    if( requireReached ) {
      require(timeRemaining == 0, "Claim deadline is not reached yet");
    } else {
      require(timeRemaining > 0, "Claim deadline has been reached");
    }
    _;
  }

  modifier notCompleted() {
    bool completed = exampleExternalContract.completed();
    require(!completed, "Stake already completed!");
    _;
  }
```

Los modificadores withdrawalDeadlineReached(bool requireReached) y claimDeadlineReached(bool 
requireReached) aceptan un par谩metro booleano y comprueban si sus respectivos plazos son verdaderos o 
falsos.

El modificador notCompleted() funciona de forma similar, pero en realidad es un poco m谩s complejo aunque 
contiene menos l铆neas de c贸digo.

En realidad llama a una funci贸n completed() de un contrato externo fuera de Staker y comprueba si 
devuelve true o false para confirmar si se ha cambiado esa bandera.

Ahora, vamos a implementar los modificadores que acabamos de crear en el siguiente par de funciones para 
el acceso a la puerta.

Funci贸n de dep贸sito/apuesta

En nuestra funci贸n de apuesta, usamos los modificadores creados anteriormente estableciendo los 
par谩metros dentro de withdrawalDeadlineReached() para que sean false y claimDeadlineReached() para que 
sean false ya que no queremos que ninguna de las fechas l铆mite haya pasado todav铆a.

```
  // Stake function for a user to stake ETH in our contract
  
  function stake() public payable withdrawalDeadlineReached(false) claimDeadlineReached(false) {
    balances[msg.sender] = balances[msg.sender] + msg.value;
    depositTimestamps[msg.sender] = block.timestamp;
    emit Stake(msg.sender, msg.value);
  }
```

El resto de la funci贸n es bastante est谩ndar en un escenario t铆pico de "dep贸sito" donde nuestro mapeo de 
saldo se actualiza para incluir el dinero enviado.

Tambi茅n establecemos nuestra marca de tiempo de dep贸sito con la hora actual del dep贸sito para que podamos 
acceder a ese valor almacenado para el c谩lculo de intereses m谩s tarde.

Funci贸n de retirada

En nuestra funci贸n de retirada, volvemos a utilizar los modificadores creados anteriormente, pero esta 
vez queremos que withdrawalDeadlineReached() sea verdadero y claimDeadlineReached() sea falso.

Este conjunto de modificadores/par谩metros significa que estamos en el punto 贸ptimo para la ventana de 
retirada, ya que es el momento para que la retirada tenga lugar sin ninguna penalizaci贸n y adem谩s 
obtenemos intereses.

```
  /*
  Withdraw function for a user to remove their staked ETH inclusive
  of both the principle balance and any accrued interest
  */
  
  function withdraw() public withdrawalDeadlineReached(true) claimDeadlineReached(false) notCompleted{
    require(balances[msg.sender] > 0, "You have no balance to withdraw!");
    uint256 individualBalance = balances[msg.sender];
    uint256 indBalanceRewards = individualBalance + ((block.timestamp-depositTimestamps[msg.sender])*rewardRatePerSecond);
    balances[msg.sender] = 0;

    // Transfer all ETH via call! (not transfer) cc: https://solidity-by-example.org/sending-ether
    (bool sent, bytes memory data) = msg.sender.call{value: indBalanceRewards}("");
    require(sent, "RIP; withdrawal failed :( ");
  }
```

El resto de la funci贸n realiza algunos pasos importantes.

    Comprueba que la persona que intenta retirar ETH realmente tiene una apuesta distinta de cero.
    Calcula la cantidad de ETH adeudada en concepto de intereses tomando el n煤mero de segundos 
    transcurridos desde el dep贸sito hasta la retirada y multiplic谩ndolo por nuestra constante de inter茅s.
    Establece el saldo de ETH apostado por el usuario en 0 para que no se produzca una doble 
    contabilizaci贸n.
    Transfiere el ETH del contrato inteligente de vuelta a la cartera del usuario.

Ejecutar la funci贸n de repatriaci贸n

Aqu铆, queremos que claimDeadlineReached() sea true ya que la repatriaci贸n de fondos improductivos s贸lo 
puede ocurrir despu茅s de la marca de 4 minutos.

Del mismo modo, queremos que notCompleted sea true ya que esta dApp est谩 dise帽ada para un solo uso.

```
  /*
  Allows any user to repatriate "unproductive" funds that are left in the staking contract
  past the defined withdrawal period
  */
  
  function execute() public claimDeadlineReached(true) notCompleted {
    uint256 contractBalance = address(this).balance;
    exampleExternalContract.complete{value: address(this).balance}();
  }
```

El resto de las funciones:

    Toma el saldo actual de ETH en el contrato Staker
    Env铆a el ETH al exampleExternalContract del repo

Si has seguido el Solidity hasta ahora, tu Staker.sol deber铆a tener el siguiente aspecto:

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.4;

import "hardhat/console.sol";
import "./ExampleExternalContract.sol";

contract Staker {

  ExampleExternalContract public exampleExternalContract;

  mapping(address => uint256) public balances;
  mapping(address => uint256) public depositTimestamps;

  uint256 public constant rewardRatePerSecond = 0.1 ether;
  uint256 public withdrawalDeadline = block.timestamp + 120 seconds;
  uint256 public claimDeadline = block.timestamp + 240 seconds;
  uint256 public currentBlock = 0;

  // Events
  event Stake(address indexed sender, uint256 amount);
  event Received(address, uint);
  event Execute(address indexed sender, uint256 amount);

  // Modifiers
  /*
  Checks if the withdrawal period has been reached or not
  */
  modifier withdrawalDeadlineReached( bool requireReached ) {
    uint256 timeRemaining = withdrawalTimeLeft();
    if( requireReached ) {
      require(timeRemaining == 0, "Withdrawal period is not reached yet");
    } else {
      require(timeRemaining > 0, "Withdrawal period has been reached");
    }
    _;
  }

  /*
  Checks if the claim period has ended or not
  */
  modifier claimDeadlineReached( bool requireReached ) {
    uint256 timeRemaining = claimPeriodLeft();
    if( requireReached ) {
      require(timeRemaining == 0, "Claim deadline is not reached yet");
    } else {
      require(timeRemaining > 0, "Claim deadline has been reached");
    }
    _;
  }

  /*
  Requires that the contract only be completed once!
  */
  modifier notCompleted() {
    bool completed = exampleExternalContract.completed();
    require(!completed, "Stake already completed!");
    _;
  }

  constructor(address exampleExternalContractAddress){
      exampleExternalContract = ExampleExternalContract(exampleExternalContractAddress);
  }

  // Stake function for a user to stake ETH in our contract
  function stake() public payable withdrawalDeadlineReached(false) claimDeadlineReached(false){
    balances[msg.sender] = balances[msg.sender] + msg.value;
    depositTimestamps[msg.sender] = block.timestamp;
    emit Stake(msg.sender, msg.value);
  }

  /*
  Withdraw function for a user to remove their staked ETH inclusive
  of both principal and any accrued interest
  */
  function withdraw() public withdrawalDeadlineReached(true) claimDeadlineReached(false) notCompleted{
    require(balances[msg.sender] > 0, "You have no balance to withdraw!");
    uint256 individualBalance = balances[msg.sender];
    uint256 indBalanceRewards = individualBalance + ((block.timestamp-depositTimestamps[msg.sender])*rewardRatePerSecond);
    balances[msg.sender] = 0;

    // Transfer all ETH via call! (not transfer) cc: https://solidity-by-example.org/sending-ether
    (bool sent, bytes memory data) = msg.sender.call{value: indBalanceRewards}("");
    require(sent, "RIP; withdrawal failed :( ");
  }

  /*
  Allows any user to repatriate "unproductive" funds that are left in the staking contract
  past the defined withdrawal period
  */
  function execute() public claimDeadlineReached(true) notCompleted {
    uint256 contractBalance = address(this).balance;
    exampleExternalContract.complete{value: address(this).balance}();
  }

  /*
  READ-ONLY function to calculate the time remaining before the minimum staking period has passed
  */
  function withdrawalTimeLeft() public view returns (uint256 withdrawalTimeLeft) {
    if( block.timestamp >= withdrawalDeadline) {
      return (0);
    } else {
      return (withdrawalDeadline - block.timestamp);
    }
  }

  /*
  READ-ONLY function to calculate the time remaining before the minimum staking period has passed
  */
  function claimPeriodLeft() public view returns (uint256 claimPeriodLeft) {
    if( block.timestamp >= claimDeadline) {
      return (0);
    } else {
      return (claimDeadline - block.timestamp);
    }
  }

  /*
  Time to "kill-time" on our local testnet
  */
  function killTime() public {
    currentBlock = block.timestamp;
  }

  /*
  \Function for our smart contract to receive ETH
  cc: https://docs.soliditylang.org/en/latest/contracts.html#receive-ether-function
  */
  receive() external payable {
      emit Received(msg.sender, msg.value);
  }

}
```

_Incursi贸n en el Frontend_

隆Impresionante! Acabamos de pasar por un mont贸n de Solidity. Cuando se trata de pantallas frontend, 
Scaffold-Eth trata de mantener las cosas agradables y simples. 隆Contiene un mont贸n de diferentes 
componentes react que dan a los usuarios soluciones de bajo c贸digo para UIs impresionantes! Te animo a 
jugar con los diferentes componentes disponibles para ti, pero, mientras tanto, vamos a aprender m谩s en 
el lado espartano.

Mirando nuestro archivo App.jsx, espec铆ficamente en el bloque de c贸digo alrededor del enlace 573, vemos 
un bloque de c贸digo utilizado para capturar eventos emitidos desde nuestros contratos Solidity y 
mostrarlos como una lista.

Efectivamente, nos permite registrar las diferentes acciones disparadas desde nuestros contratos 
inteligentes, analizar la informaci贸n almacenada, y luego permitir visualmente a los usuarios de la dApp 
ver su historial en la cadena.

Si bien vamos a practicar las buenas normas de programaci贸n mediante la emisi贸n de eventos en nuestro 
contrato Solidity, esta vez, vamos a ignorar los eventos en nuestro frontend para simplificar, as铆 que 
vamos a eliminar ese bloque de c贸digo por completo.

Si miras en tu pesta帽a Staker UI, notar谩s que la caja de eventos ha sido borrada.

Modificaciones del frontend

En su mayor parte, gran parte del c贸digo de la interfaz seguir谩 siendo el mismo que el predeterminado. En 
el paso anterior, ya hemos eliminado el componente react de eventos.

Nuestro objetivo final ser谩 tener una interfaz de usuario agradable y sencilla que se parezca a la 
siguiente:


![Alt text](https://www.github.com/assets.digitalocean.com/articles/alligator/boo.svg "un t铆tulo")

Tenga en cuenta que al frontend por defecto le faltan algunos de los elementos visuales del repo por 
defecto.

Tasa de recompensa por segundo

Para construir esta parte, insertamos el siguiente trozo de c贸digo justo debajo del bloque del contrato 
Staker:

```
  <div style={{ padding: 8, marginTop: 16 }}>
    <div>Reward Rate Per Second:</div>
    <Balance balance={rewardRatePerSecond} fontSize={64} /> ETH
  </div>
```

Aqu铆 aprovechamos el componente Balance React de Scaffold-Eth para ayudarnos con el formato.
Elementos de la interfaz de usuario de Deadline

Para construir el componente visual de la fecha l铆mite, utilizamos los siguientes 2 fragmentos de c贸digo:

```
  ** keep track of a variable from the contract in the local React state:
  const claimPeriodLeft = useContractReader(readContracts, "Staker", "claimPeriodLeft");
  console.log("鈴? Claim Period Left:", claimPeriodLeft);

  const withdrawalTimeLeft = useContractReader(readContracts, "Staker", "withdrawalTimeLeft");
  console.log("鈴? Withdrawal Time Left:", withdrawalTimeLeft);
```

```
 <div style={{ padding: 8, marginTop: 16, fontWeight: "bold" }}>
  <div>Claim Period Left:</div>
  {claimPeriodLeft && humanizeDuration(claimPeriodLeft.toNumber() * 1000)}
 </div>

 <div style={{ padding: 8, marginTop: 16, fontWeight: "bold"}}>
  <div>Withdrawal Period Left:</div>
  {withdrawalTimeLeft && humanizeDuration(withdrawalTimeLeft.toNumber() * 1000)}
 </div>
```

Mientras que el segundo fragmento de c贸digo es familiar y se parece a lo que ya hemos visto, el primer 
bloque de c贸digo es un poco 煤nico. No es que llamemos a claimPeriodLeft y withdrawalTimeLeft para acceder 
a los valores de las variables almacenadas en el frontend.

Sin embargo, primero debemos leer los valores desde el contrato inteligente. El primer fragmento de 
c贸digo se encarga de esta l贸gica.

Otros Modificaciones en otros componentes de la interfaz de usuario

Ahora que has visto 2 ejemplos diferentes de componentes de interfaz de usuario de frontend, 驴puedes 
averiguar c贸mo hacer los cambios restantes en el frontend para que se vea como el ejemplo proporcionado 
anteriormente?

S贸lo tendr谩s que ajustar unos pocos par谩metros, as铆 que no pienses demasiado.


Uso libre de la interfaz de usuario

Mientras que Scaffold-Eth tiene un mont贸n de componentes por defecto que permiten a los usuarios 
simplemente aprovechar las soluciones de "bajo c贸digo", tambi茅n da a los usuarios acceso a las 
bibliotecas frontend m谩s grandes.

Por defecto, tiene un hook en Ant Design react components (https://ant.design/components/overview/) que 
permite a cualquiera extraer componentes de all铆.

En nuestro frontend de ejemplo, vemos "l铆neas" que dividen cada gran bloque de componentes visuales.

Recrea estas l铆neas explorando las diferentes opciones disponibles en Ant Design

    馃摌

    Si quieres una pista, 隆echa un vistazo a Ant Dividers!

    隆No olvides que necesitamos importar el componente que planeamos usar en la parte superior de nuestro archivo App.jsx!


    import { Alert, Button, Col, Menu, Row, List, Divider } from "antd";


Si has seguido el c贸digo del frontend, tu App.jsx deber铆a tener el siguiente aspecto:

```
import WalletConnectProvider from "@walletconnect/web3-provider";
//import Torus from "@toruslabs/torus-embed"
import WalletLink from "walletlink";
import { Alert, Button, Col, Menu, Row, List, Divider } from "antd";
import "antd/dist/antd.css";
import React, { useCallback, useEffect, useState } from "react";
import { BrowserRouter, Link, Route, Switch } from "react-router-dom";
import Web3Modal from "web3modal";
import "./App.css";
import { Account, Address, Balance, Contract, Faucet, GasGauge, Header, Ramp, ThemeSwitch } from "./components";
import { INFURA_ID, NETWORK, NETWORKS } from "./constants";
import { Transactor } from "./helpers";
import {
  useBalance,
  useContractLoader,
  useContractReader,
  useGasPrice,
  useOnBlock,
  useUserProviderAndSigner,
} from "eth-hooks";
import { useEventListener } from "eth-hooks/events/useEventListener";
import { useExchangeEthPrice } from "eth-hooks/dapps/dex";
// import Hints from "./Hints";
import { ExampleUI, Hints, Subgraph } from "./views";

import { useContractConfig } from "./hooks";
import Portis from "@portis/web3";
import Fortmatic from "fortmatic";
import Authereum from "authereum";
import humanizeDuration from "humanize-duration";

const { ethers } = require("ethers");
/*
    Welcome to 馃彈 scaffold-eth !

    Code:
    https://github.com/austintgriffith/scaffold-eth

    Support:
    https://t.me/joinchat/KByvmRe5wkR-8F_zz6AjpA
    or DM @austingriffith on Twitter or Telegram

    You should get your own Infura.io ID and put it in `constants.js`
    (this is your connection to the main Ethereum network for ENS etc.)


    馃審 EXTERNAL CONTRACTS:
    You can also bring in contract artifacts in `constants.js`
    (and then use the `useExternalContractLoader()` hook!)
*/

/// 馃摗 What chain are your contracts deployed to?
const targetNetwork = NETWORKS.localhost; // <------- select your target frontend network (localhost, rinkeby, xdai, mainnet)

// 馃槵 Sorry for all the console logging
const DEBUG = true;
const NETWORKCHECK = true;

// 馃洶 providers
if (DEBUG) console.log("馃摗 Connecting to Mainnet Ethereum");
// const mainnetProvider = getDefaultProvider("mainnet", { infura: INFURA_ID, etherscan: ETHERSCAN_KEY, quorum: 1 });
// const mainnetProvider = new InfuraProvider("mainnet",INFURA_ID);
//
// attempt to connect to our own scaffold eth rpc and if that fails fall back to infura...
// Using StaticJsonRpcProvider as the chainId won't change see https://github.com/ethers-io/ethers.js/issues/901
const scaffoldEthProvider = navigator.onLine
  ? new ethers.providers.StaticJsonRpcProvider("https://rpc.scaffoldeth.io:48544")
  : null;
const poktMainnetProvider = navigator.onLine
  ? new ethers.providers.StaticJsonRpcProvider(
      "https://eth-mainnet.gateway.pokt.network/v1/lb/611156b4a585a20035148406",
    )
  : null;
const mainnetInfura = navigator.onLine
  ? new ethers.providers.StaticJsonRpcProvider("https://mainnet.infura.io/v3/" + INFURA_ID)
  : null;
// ( 鈿狅笍 Getting "failed to meet quorum" errors? Check your INFURA_ID

// 馃彔 Your local provider is usually pointed at your local blockchain
const localProviderUrl = targetNetwork.rpcUrl;
// as you deploy to other networks you can set REACT_APP_PROVIDER=https://dai.poa.network in packages/react-app/.env
const localProviderUrlFromEnv = process.env.REACT_APP_PROVIDER ? process.env.REACT_APP_PROVIDER : localProviderUrl;
if (DEBUG) console.log("馃彔 Connecting to provider:", localProviderUrlFromEnv);
const localProvider = new ethers.providers.StaticJsonRpcProvider(localProviderUrlFromEnv);

// 馃敪 block explorer URL
const blockExplorer = targetNetwork.blockExplorer;

// Coinbase walletLink init
const walletLink = new WalletLink({
  appName: "coinbase",
});

// WalletLink provider
const walletLinkProvider = walletLink.makeWeb3Provider(`https://mainnet.infura.io/v3/${INFURA_ID}`, 1);

// Portis ID: 6255fb2b-58c8-433b-a2c9-62098c05ddc9
/*
  Web3 modal helps us "connect" external wallets:
*/
const web3Modal = new Web3Modal({
  network: "mainnet", // Optional. If using WalletConnect on xDai, change network to "xdai" and add RPC info below for xDai chain.
  cacheProvider: true, // optional
  theme: "light", // optional. Change to "dark" for a dark theme.
  providerOptions: {
    walletconnect: {
      package: WalletConnectProvider, // required
      options: {
        bridge: "https://polygon.bridge.walletconnect.org",
        infuraId: INFURA_ID,
        rpc: {
          1: `https://mainnet.infura.io/v3/${INFURA_ID}`, // mainnet // For more WalletConnect providers: https://docs.walletconnect.org/quick-start/dapps/web3-provider#required
          42: `https://kovan.infura.io/v3/${INFURA_ID}`,
          100: "https://dai.poa.network", // xDai
        },
      },
    },
    portis: {
      display: {
        logo: "https://user-images.githubusercontent.com/9419140/128913641-d025bc0c-e059-42de-a57b-422f196867ce.png",
        name: "Portis",
        description: "Connect to Portis App",
      },
      package: Portis,
      options: {
        id: "6255fb2b-58c8-433b-a2c9-62098c05ddc9",
      },
    },
    fortmatic: {
      package: Fortmatic, // required
      options: {
        key: "pk_live_5A7C91B2FC585A17", // required
      },
    },
    // torus: {
    //   package: Torus,
    //   options: {
    //     networkParams: {
    //       host: "https://localhost:8545", // optional
    //       chainId: 1337, // optional
    //       networkId: 1337 // optional
    //     },
    //     config: {
    //       buildEnv: "development" // optional
    //     },
    //   },
    // },
    "custom-walletlink": {
      display: {
        logo: "https://play-lh.googleusercontent.com/PjoJoG27miSglVBXoXrxBSLveV6e3EeBPpNY55aiUUBM9Q1RCETKCOqdOkX2ZydqVf0",
        name: "Coinbase",
        description: "Connect to Coinbase Wallet (not Coinbase App)",
      },
      package: walletLinkProvider,
      connector: async (provider, _options) => {
        await provider.enable();
        return provider;
      },
    },
    authereum: {
      package: Authereum, // required
    },
  },
});

function App(props) {
  const mainnetProvider =
    poktMainnetProvider && poktMainnetProvider._isProvider
      ? poktMainnetProvider
      : scaffoldEthProvider && scaffoldEthProvider._network
      ? scaffoldEthProvider
      : mainnetInfura;

  const [injectedProvider, setInjectedProvider] = useState();
  const [address, setAddress] = useState();

  const logoutOfWeb3Modal = async () => {
    await web3Modal.clearCachedProvider();
    if (injectedProvider && injectedProvider.provider && typeof injectedProvider.provider.disconnect == "function") {
      await injectedProvider.provider.disconnect();
    }
    setTimeout(() => {
      window.location.reload();
    }, 1);
  };

  /* 馃挼 This hook will get the price of ETH from 馃 Uniswap: */
  const price = useExchangeEthPrice(targetNetwork, mainnetProvider);

  /* 馃敟 This hook will get the price of Gas from 鉀斤笍 EtherGasStation */
  const gasPrice = useGasPrice(targetNetwork, "fast");
  // Use your injected provider from 馃 Metamask or if you don't have it then instantly generate a 馃敟 burner wallet.
  const userProviderAndSigner = useUserProviderAndSigner(injectedProvider, localProvider);
  const userSigner = userProviderAndSigner.signer;

  useEffect(() => {
    async function getAddress() {
      if (userSigner) {
        const newAddress = await userSigner.getAddress();
        setAddress(newAddress);
      }
    }
    getAddress();
  }, [userSigner]);

  // You can warn the user if you would like them to be on a specific network
  const localChainId = localProvider && localProvider._network && localProvider._network.chainId;
  const selectedChainId =
    userSigner && userSigner.provider && userSigner.provider._network && userSigner.provider._network.chainId;

  // For more hooks, check out 馃敆eth-hooks at: https://www.npmjs.com/package/eth-hooks

  // The transactor wraps transactions and provides notificiations
  const tx = Transactor(userSigner, gasPrice);

  // Faucet Tx can be used to send funds from the faucet
  const faucetTx = Transactor(localProvider, gasPrice);

  // 馃彈 scaffold-eth is full of handy hooks like this one to get your balance:
  const yourLocalBalance = useBalance(localProvider, address);

  // Just plug in different 馃洶 providers to get your balance on different chains:
  const yourMainnetBalance = useBalance(mainnetProvider, address);

  const contractConfig = useContractConfig();

  // Load in your local 馃摑 contract and read a value from it:
  const readContracts = useContractLoader(localProvider, contractConfig);

  // If you want to make 馃攼 write transactions to your contracts, use the userSigner:
  const writeContracts = useContractLoader(userSigner, contractConfig, localChainId);

  // EXTERNAL CONTRACT EXAMPLE:
  //
  // If you want to bring in the mainnet DAI contract it would look like:
  const mainnetContracts = useContractLoader(mainnetProvider, contractConfig);

  // If you want to call a function on a new block
  useOnBlock(mainnetProvider, () => {
    console.log(`鉀? A new mainnet block is here: ${mainnetProvider._lastBlockNumber}`);
  });

  // Then read your DAI balance like:
  const myMainnetDAIBalance = useContractReader(mainnetContracts, "DAI", "balanceOf", [
    "0x34aA3F359A9D614239015126635CE7732c18fDF3",
  ]);

  //keep track of contract balance to know how much has been staked total:
  const stakerContractBalance = useBalance(
    localProvider,
    readContracts && readContracts.Staker ? readContracts.Staker.address : null,
  );
  if (DEBUG) console.log("馃挼 stakerContractBalance", stakerContractBalance);

  const rewardRatePerSecond = useContractReader(readContracts, "Staker", "rewardRatePerSecond");
  console.log("馃挼 Reward Rate:", rewardRatePerSecond);

  // ** keep track of a variable from the contract in the local React state:
  const balanceStaked = useContractReader(readContracts, "Staker", "balances", [address]);
  console.log("馃捀 balanceStaked:", balanceStaked);

  // ** 馃摕 Listen for broadcast events
  const stakeEvents = useEventListener(readContracts, "Staker", "Stake", localProvider, 1);
  console.log("馃摕 stake events:", stakeEvents);

  const receiveEvents = useEventListener(readContracts, "Staker", "Received", localProvider, 1);
  console.log("馃摕 receive events:", receiveEvents);

  // ** keep track of a variable from the contract in the local React state:
  const claimPeriodLeft = useContractReader(readContracts, "Staker", "claimPeriodLeft");
  console.log("鈴? Claim Period Left:", claimPeriodLeft);

  const withdrawalTimeLeft = useContractReader(readContracts, "Staker", "withdrawalTimeLeft");
  console.log("鈴? Withdrawal Time Left:", withdrawalTimeLeft);


  // ** Listen for when the contract has been 'completed'
  const complete = useContractReader(readContracts, "ExampleExternalContract", "completed");
  console.log("鉁? complete:", complete);

  const exampleExternalContractBalance = useBalance(
    localProvider,
    readContracts && readContracts.ExampleExternalContract ? readContracts.ExampleExternalContract.address : null,
  );
  if (DEBUG) console.log("馃挼 exampleExternalContractBalance", exampleExternalContractBalance);

  let completeDisplay = "";
  if (complete) {
    completeDisplay = (
      <div style={{padding: 64, backgroundColor: "#eeffef", fontWeight: "bold", color: "rgba(0, 0, 0, 0.85)" }} >
        -- 馃拃 Staking App Fund Repatriation Executed 馃 --
        <Balance balance={exampleExternalContractBalance} fontSize={32} /> ETH locked!
      </div>
    );
  }

  /*
  const addressFromENS = useResolveName(mainnetProvider, "austingriffith.eth");
  console.log("馃彿 Resolved austingriffith.eth as:", addressFromENS)
  */

  //
  // 馃Й DEBUG 馃懆馃徎鈥嶐煍?
  //
  useEffect(() => {
    if (
      DEBUG &&
      mainnetProvider &&
      address &&
      selectedChainId &&
      yourLocalBalance &&
      yourMainnetBalance &&
      readContracts &&
      writeContracts &&
      mainnetContracts
    ) {
      console.log("_____________________________________ 馃彈 scaffold-eth _____________________________________");
      console.log("馃寧 mainnetProvider", mainnetProvider);
      console.log("馃彔 localChainId", localChainId);
      console.log("馃懇鈥嶐煉? selected address:", address);
      console.log("馃暤馃徎鈥嶁檪锔? selectedChainId:", selectedChainId);
      console.log("馃挼 yourLocalBalance", yourLocalBalance ? ethers.utils.formatEther(yourLocalBalance) : "...");
      console.log("馃挼 yourMainnetBalance", yourMainnetBalance ? ethers.utils.formatEther(yourMainnetBalance) : "...");
      console.log("馃摑 readContracts", readContracts);
      console.log("馃實 DAI contract on mainnet:", mainnetContracts);
      console.log("馃挼 yourMainnetDAIBalance", myMainnetDAIBalance);
      console.log("馃攼 writeContracts", writeContracts);
    }
  }, [
    mainnetProvider,
    address,
    selectedChainId,
    yourLocalBalance,
    yourMainnetBalance,
    readContracts,
    writeContracts,
    mainnetContracts,
  ]);

  let networkDisplay = "";
  if (NETWORKCHECK && localChainId && selectedChainId && localChainId !== selectedChainId) {
    const networkSelected = NETWORK(selectedChainId);
    const networkLocal = NETWORK(localChainId);
    if (selectedChainId === 1337 && localChainId === 31337) {
      networkDisplay = (
        <div style={{ zIndex: 2, position: "absolute", right: 0, top: 60, padding: 16 }}>
          <Alert
            message="鈿狅笍 Wrong Network ID"
            description={
              <div>
                You have <b>chain id 1337</b> for localhost and you need to change it to <b>31337</b> to work with
                HardHat.
                <div>(MetaMask -&gt; Settings -&gt; Networks -&gt; Chain ID -&gt; 31337)</div>
              </div>
            }
            type="error"
            closable={false}
          />
        </div>
      );
    } else {
      networkDisplay = (
        <div style={{ zIndex: 2, position: "absolute", right: 0, top: 60, padding: 16 }}>
          <Alert
            message="鈿狅笍 Wrong Network"
            description={
              <div>
                You have <b>{networkSelected && networkSelected.name}</b> selected and you need to be on{" "}
                <Button
                  onClick={async () => {
                    const ethereum = window.ethereum;
                    const data = [
                      {
                        chainId: "0x" + targetNetwork.chainId.toString(16),
                        chainName: targetNetwork.name,
                        nativeCurrency: targetNetwork.nativeCurrency,
                        rpcUrls: [targetNetwork.rpcUrl],
                        blockExplorerUrls: [targetNetwork.blockExplorer],
                      },
                    ];
                    console.log("data", data);

                    let switchTx;
                    // https://docs.metamask.io/guide/rpc-api.html#other-rpc-methods
                    try {
                      switchTx = await ethereum.request({
                        method: "wallet_switchEthereumChain",
                        params: [{ chainId: data[0].chainId }],
                      });
                    } catch (switchError) {
                      // not checking specific error code, because maybe we're not using MetaMask
                      try {
                        switchTx = await ethereum.request({
                          method: "wallet_addEthereumChain",
                          params: data,
                        });
                      } catch (addError) {
                        // handle "add" error
                      }
                    }

                    if (switchTx) {
                      console.log(switchTx);
                    }
                  }}
                >
                  <b>{networkLocal && networkLocal.name}</b>
                </Button>
              </div>
            }
            type="error"
            closable={false}
          />
        </div>
      );
    }
  } else {
    networkDisplay = (
      <div style={{ zIndex: -1, position: "absolute", right: 154, top: 28, padding: 16, color: targetNetwork.color }}>
        {targetNetwork.name}
      </div>
    );
  }

  const loadWeb3Modal = useCallback(async () => {
    const provider = await web3Modal.connect();
    setInjectedProvider(new ethers.providers.Web3Provider(provider));

    provider.on("chainChanged", chainId => {
      console.log(`chain changed to ${chainId}! updating providers`);
      setInjectedProvider(new ethers.providers.Web3Provider(provider));
    });

    provider.on("accountsChanged", () => {
      console.log(`account changed!`);
      setInjectedProvider(new ethers.providers.Web3Provider(provider));
    });

    // Subscribe to session disconnection
    provider.on("disconnect", (code, reason) => {
      console.log(code, reason);
      logoutOfWeb3Modal();
    });
  }, [setInjectedProvider]);

  useEffect(() => {
    if (web3Modal.cachedProvider) {
      loadWeb3Modal();
    }
  }, [loadWeb3Modal]);

  const [route, setRoute] = useState();
  useEffect(() => {
    setRoute(window.location.pathname);
  }, [setRoute]);

  let faucetHint = "";
  const faucetAvailable = localProvider && localProvider.connection && targetNetwork.name.indexOf("local") !== -1;

  const [faucetClicked, setFaucetClicked] = useState(false);
  if (
    !faucetClicked &&
    localProvider &&
    localProvider._network &&
    localProvider._network.chainId === 31337 &&
    yourLocalBalance &&
    ethers.utils.formatEther(yourLocalBalance) <= 0
  ) {
    faucetHint = (
      <div style={{ padding: 16 }}>
        <Button
          type="primary"
          onClick={() => {
            faucetTx({
              to: address,
              value: ethers.utils.parseEther("0.01"),
            });
            setFaucetClicked(true);
          }}
        >
          馃挵 Grab funds from the faucet 鉀斤笍
        </Button>
      </div>
    );
  }

  return (
    <div className="App">
      {/* 鉁忥笍 Edit the header and change the title to your project name */}
      <Header />
      {networkDisplay}
      <BrowserRouter>
        <Menu style={{ textAlign: "center" }} selectedKeys={[route]} mode="horizontal">
          <Menu.Item key="/">
            <Link
              onClick={() => {
                setRoute("/");
              }}
              to="/"
            >
              Staker UI
            </Link>
          </Menu.Item>
          <Menu.Item key="/contracts">
            <Link
              onClick={() => {
                setRoute("/contracts");
              }}
              to="/contracts"
            >
              Debug Contracts
            </Link>
          </Menu.Item>
        </Menu>

        <Switch>
          <Route exact path="/">
            {completeDisplay}

            <div style={{ padding: 8, marginTop: 16 }}>
              <div>Staker Contract:</div>
              <Address value={readContracts && readContracts.Staker && readContracts.Staker.address} />
            </div>

            <Divider />

            <div style={{ padding: 8, marginTop: 16 }}>
              <div>Reward Rate Per Second:</div>
              <Balance balance={rewardRatePerSecond} fontSize={64} /> ETH
            </div>

            <Divider />

            <div style={{ padding: 8, marginTop: 16, fontWeight: "bold" }}>
              <div>Claim Period Left:</div>
              {claimPeriodLeft && humanizeDuration(claimPeriodLeft.toNumber() * 1000)}
            </div>

            <div style={{ padding: 8, marginTop: 16, fontWeight: "bold"}}>
              <div>Withdrawal Period Left:</div>
              {withdrawalTimeLeft && humanizeDuration(withdrawalTimeLeft.toNumber() * 1000)}
            </div>

            <Divider />

            <div style={{ padding: 8, fontWeight: "bold"}}>
              <div>Total Available ETH in Contract:</div>
              <Balance balance={stakerContractBalance} fontSize={64} />
            </div>

            <Divider />

            <div style={{ padding: 8,fontWeight: "bold" }}>
              <div>ETH Locked 馃敀 in Staker Contract:</div>
              <Balance balance={balanceStaked} fontSize={64} />
            </div>

            <div style={{ padding: 8 }}>
              <Button
                type={"default"}
                onClick={() => {
                  tx(writeContracts.Staker.execute());
                }}
              >
                馃摗 Execute!
              </Button>
            </div>

            <div style={{ padding: 8 }}>
              <Button
                type={"default"}
                onClick={() => {
                  tx(writeContracts.Staker.withdraw());
                }}
              >
                馃彠 Withdraw
              </Button>
            </div>

            <div style={{ padding: 8 }}>
              <Button
                type={balanceStaked ? "success" : "primary"}
                onClick={() => {
                  tx(writeContracts.Staker.stake({ value: ethers.utils.parseEther("0.5") }));
                }}
              >
                馃ォ Stake 0.5 ether!
              </Button>
            </div>

            {/*
                馃帥 this scaffolding is full of commonly used components
                this <Contract/> component will automatically parse your ABI
                and give you a form to interact with it locally
            */}

            {/* uncomment for a second contract:
            <Contract
              name="SecondContract"
              signer={userProvider.getSigner()}
              provider={localProvider}
              address={address}
              blockExplorer={blockExplorer}
              contractConfig={contractConfig}
            />
            */}
          </Route>
          <Route path="/contracts">
            <Contract
              name="Staker"
              signer={userSigner}
              provider={localProvider}
              address={address}
              blockExplorer={blockExplorer}
              contractConfig={contractConfig}
            />
            <Contract
              name="ExampleExternalContract"
              signer={userSigner}
              provider={localProvider}
              address={address}
              blockExplorer={blockExplorer}
              contractConfig={contractConfig}
            />
          </Route>
        </Switch>
      </BrowserRouter>

      <ThemeSwitch />

      {/* 馃懆鈥嶐煉? Your account is in the top right with a wallet at connect options */}
      <div style={{ position: "fixed", textAlign: "right", right: 0, top: 0, padding: 10 }}>
        <Account
          address={address}
          localProvider={localProvider}
          userSigner={userSigner}
          mainnetProvider={mainnetProvider}
          price={price}
          web3Modal={web3Modal}
          loadWeb3Modal={loadWeb3Modal}
          logoutOfWeb3Modal={logoutOfWeb3Modal}
          blockExplorer={blockExplorer}
        />
        {faucetHint}
      </div>

      <div style={{ marginTop: 32, opacity: 0.5 }}>
        {/* Add your address here */}
        Created by <Address value={"Your...address"} ensProvider={mainnetProvider} fontSize={16} />
      </div>

      <div style={{ marginTop: 32, opacity: 0.5 }}>
        <a target="_blank" style={{ padding: 32, color: "#000" }} href="https://github.com/scaffold-eth/scaffold-eth">
          馃嵈 Fork me!
        </a>
      </div>

      {/* 馃椇 Extra UI like gas price, eth price, faucet, and support: */}
      <div style={{ position: "fixed", textAlign: "left", left: 0, bottom: 20, padding: 10 }}>
        <Row align="middle" gutter={[4, 4]}>
          <Col span={8}>
            <Ramp price={price} address={address} networks={NETWORKS} />
          </Col>

          <Col span={8} style={{ textAlign: "center", opacity: 0.8 }}>
            <GasGauge gasPrice={gasPrice} />
          </Col>
          <Col span={8} style={{ textAlign: "center", opacity: 1 }}>
            <Button
              onClick={() => {
                window.open("https://t.me/joinchat/KByvmRe5wkR-8F_zz6AjpA");
              }}
              size="large"
              shape="round"
            >
              <span style={{ marginRight: 8 }} role="img" aria-label="support">
                馃挰
              </span>
              Support
            </Button>
          </Col>
        </Row>

        <Row align="middle" gutter={[4, 4]}>
          <Col span={24}>
            {
              /*  if the local provider has a signer, let's show the faucet:  */
              faucetAvailable ? (
                <Faucet localProvider={localProvider} price={price} ensProvider={mainnetProvider} />
              ) : (
                ""
              )
            }
          </Col>
        </Row>
      </div>
    </div>
  );
}

export default App;
```

Impresionante.

Hemos trabajado juntos en un mont贸n de nuevos componentes, tanto en t茅rminos de entornos de desarrollo, 
Solidity y c贸digo frontend.

Comprueba que tu dApp funciona como esperabas.

    驴Funciona la dApp con staking de un solo uso?
    驴Se respetan las condiciones de retirada/repatriaci贸n de fondos? Pulse yarn deploy --reset varias 
    veces para comprobar cada ventana de tiempo.


## Desaf铆o 鈿欙笍

Bien, ahora es el momento de la mejor parte. Voy a dejarte con unos cuantos retos de extensi贸n para que 
los pruebes por tu cuenta, 隆para ver si entiendes completamente lo que has aprendido aqu铆!

    1. Actualizar el mecanismo de inter茅s en el contrato Staker.sol para que reciba una cantidad "no 
    lineal" de ETH basada en los bloques entre el dep贸sito y la retirada

    馃摌

    隆Sugiero implementar una funci贸n exponencial b谩sica!

2. Permitir a los usuarios depositar cualquier cantidad arbitraria de ETH en el contrato inteligente, no 
s贸lo 0,5 ETH.

3. En lugar de utilizar el contrato vainilla ExampleExternalContract, implementar una funci贸n en Staker.
sol que te permita recuperar el ETH bloqueado en ExampleExternalContract y volver a depositarlo en el 
contrato de contrato Staker.

    Aseg煤rate de poner en la "lista blanca" s贸lo una direcci贸n para llamar a esta nueva funci贸n y 
    controlar su uso.
    Aseg煤rese de crear l贸gica/eliminar c贸digo existente para garantizar que los usuarios puedan 
    interactuar con el contrato Staker una y otra vez. Queremos ser capaces de hacer ping-pong desde 
    Staker -> ExampleExternalContract repetidamente.

Una vez que hayas terminado con el desaf铆o, tuitea sobre 茅l etiquetando a @AlchemyPlatform en Twitter y 
al autor 隆@crypt0zeke usando el hashtag #roadtoweb3!


## Construido con 馃洜锔?

_Herramientas que se utilizaron para la creaci贸n del proyecto/Desafio_

- [Visual Studio Code](https://code.visualstudio.com/) - El IDE
- [Replit](https://replit.com) - IDE en l铆nea para Front End
- [Alchemy](https://dashboard.alchemy.com) - Interfaz/API para la Red Goerli Tesnet
- [Xubuntu](https://xubuntu.org/) - Sistema operativo basado en la distribuci贸n Ubuntu
- [Opensea](https://testnets.opensea.io) - Sistema web utilizado para verificar/visualizar nuestro NFT
- [Goerli Testnet](https://goerli.etherscan.io) - Sistema web utilizado para verificar transacciones,
  verificar contratos, desplegar contratos, verificar y publicar c贸digo fuente de contratos, etc.
- [Goerli Faucet](https://goerlifaucet.com/) - Faucet utilizado para obtener ETH utilizado en las
  pruebas para desplegar las SmartContrat as铆 como para interactuar con ellas.
- [Solidity](https://soliditylang.org) Lenguaje de programaci贸n orientado a objetos para implementar
  contratos inteligentes en varias plataformas de cadenas de bloques.
- [Hardhat](https://hardhat.org) - Entorno utilizado por los desarrolladores para probar, compilar,
  desplegar y depurar dApps basadas en la cadena de bloques Ethereum.
- [GitHub](https://github.com/) - Servicio de alojamiento en Internet para el desarrollo de software y
  el control de versiones mediante Git.
- [MetaMask](https://metamask.io) - MetaMask es una cartera de criptodivisas de software utilizada
  para interactuar con la blockchain de Ethereum.

## Contribuir 馃枃锔?

Por favor, lee [CONTRIBUTING.md](https://gist.github.com/llabori-venehsoftw/xxxxxx) para m谩s detalles 
sobre nuestro c贸digo de conducta, y el proceso para enviarnos pull requests.

## Wiki 馃摉

N/A

## Versionado 馃搶

Utilizamos [GitHub] para versionar todos los archivos utilizados (https://github.com/tu/proyecto/tags).

## Autores 鉁掞笍

_Personas que colaboraron con el desarrollo del reto_.

- **VeneHsoftw** - _Trabajo Inicial_ - [venehsoftw](https://github.com/venehsoftw)
- **Luis Labori** - _Trabajo inicial_, _Documentaci贸nn_ - [llabori-venehsoftw](https://github.com/
  llabori-venehsoftw)

## Licencia 馃搫

Este proyecto est谩 licenciado bajo la Licencia (Su Licencia) - ver el archivo [LICENSE.md](LICENSE.md)
para m谩s detalles.

## Gratitud 馃巵

- Si la informaci贸n reflejada en este Repo te ha parecido de gran importancia, por favor, ampl铆a tu
  colaboraci贸n pulsando el bot贸n de la estrella en el margen superior derecho. 馃摙
- Si est谩 dentro de sus posibilidades, puede ampliar su donaci贸n a trav茅s de la siguiente direcci贸n:
  `0xAeC4F555DbdE299ee034Ca6F42B83Baf8eFA6f0D`

---

鈱笍 con 鉂わ笍 por [Venehsoftw](https://github.com/llabori-venehsoftw) 馃槉
