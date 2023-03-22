# StarkNet-Es Nadai
## Desplegando tu primer contrato Cairo 1 en Starknet
Esta es una guía de la traducción oficial de la repo [Deploy Cairo 1.0]() desde el equipo de StarkNet-Es para la comunidad hispana, en ella también dejaremos alguna explicación extra así como imágenes para una mejor orientación.


Con el despliegue de la [versión 0.11](https://starkware.medium.com/starknet-alpha-v0-11-0-the-transition-to-cairo-1-0-begins-30442d494515) en Starknet, ¡ya puedes desplegar tus primeros contratos Cairo 1 en Starknet! ¡Fantástico!

... ¿Pero cómo ser?

He aquí una guía muy aproximada, antes de poner en orden nuestra documentación

## El flujo
Usted necesitará
- Clonar el repositorio de Cairo (la herramienta para compilar tu contrato Cairo 1) a una altura específica
- Instalar la última versión de Cairo-lang (la herramienta para interactuar con Starknet)
- Compila tu contrato de Cairo a Sierra, usando cairo 
- Declara tu contrato Sierra, usando cairo-lang
- Despliega tu contrato, usando cairo-lang
- Presume ante tus amigos, usando Twitter

## Clone esta repo
```bash
git clone https://github.com/starknet-edu/deploy-cairo1-demo
cd deploy-cairo1-demo
```

##  Instalar el compilador Cairo one
Si estás instalando Cairo por primera vez:
```bash
git clone https://github.com/starkware-libs/cairo/
cd cairo
git checkout 9c190561ce1e8323665857f1a77082925c817b4c
cargo build --all --release
```

Si ya tienes instalado Cairo:
```bash
cd cairo
git fetch && git pull
git checkout 9c190561ce1e8323665857f1a77082925c817b4c
cargo build --all --release
```

En este punto ya tienes instalado Cairo en este repositorio. 

## Instalando Cairo-lang
Vuelve a la carpeta raíz del repositorio

```bash
cd ..
```
Configura un entorno virtual python

```bash
python3.9 -m venv ~/cairo_venv_v11
source ~/cairo_venv_v11/bin/activate
```

Si tenías cairo-lang instalado previamente, desinstálalo
```bash
pip3 uninstall cairo-lang
```

Descarga la última versión de Cairo-lang [aquí](https://github.com/starkware-libs/cairo-lang/releases/tag/v0.11.0.1) la carpeta raíz de este repositorio. 

Instálala
```bash
pip3 install cairo-lang-0.11.0.1.zip
```
![Graph](/im%C3%A1genes/cairolang.png)

Comprueba que lo has instalado correctamente
```bash
starknet --version
```

![Graph](/im%C3%A1genes/starknetversion.png)

## Compila tu contrato usando Cairo
Puedes usar [este contrato demo](hello_starknet.cairo) para completar esta prueba. Es un registrador de eventos muy básico.

Desde tu terminal, en la carpeta en la que instalaste Cairo
```bash
cd cairo
cargo run --bin starknet-compile -- ../hello_starknet.cairo ../hello_starknet.sierra --replace-ids	
```

![Graph](/im%C3%A1genes/compile.png)

Enhorabuena, ¡has compilado tus contratos de Cairo a Sierra!

## Declara tu contrato usando Cairo-lang
### Configurar variables de entorno

```bash
export STARKNET_NETWORK=alpha-goerli
export STARKNET_WALLET=starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount
```
![Graph](/im%C3%A1genes/export.png)

### Configurar una nueva cuenta para el tutorial

Usted necesita asegurarse de que su CLI starknet está configurado con un contrato de cuenta adecuada, y los fondos. Por seguridad, voy a crear una nueva sólo para este tutorial.

```bash
starknet new_account --account version_11_test
```

![Graph](/im%C3%A1genes/wallettest.png)

- Deberías obtener tu dirección de contrato esperada
- Envíale 0.1 ETH

![Graph](/im%C3%A1genes/send.png) ![Graph](/im%C3%A1genes/send2.png)

- Monitoriza la transacción de transferencia. Una vez que ha pasado "pendiente", proceder

```bash
starknet deploy_account --account version_11_test
```
![Grpah](/im%C3%A1genes/deploya.png)

- Monitoriza la transacción de despliegue. Una vez que ha pasado "pending", proceder

![Graph](/im%C3%A1genes/deploya2.png) ![Graph](/im%C3%A1genes/deploya3.png)

### Declarando el contrato
Vuelve a la carpeta raíz del tutorial e intenta declarar, también puede ser que ya haya sido declarado esa contrato.

```bash
cd ..
starknet declare --contract hello_starknet.sierra --account version_11_test
```

¡Deberías recibir el hash de tu nueva clase declarada!

### Troubleshooting
Si no ha tenido error omita este paso pero si te ha dado error y se ha devuelto el uso de declarar:
```bash
Error: OSError: [Errno 8] Exec format error: '~/cairo_venv_11/lib/python3.9/site-packages/starkware/starknet/compiler/v1/bin/starknet-sierra-compile'
```
La razón es que cairo-lang necesita compilar tu código sierra, y utiliza un binario rust importado 'starknet-sierra-compile' para ello. Pero en este caso, el binario importado no fue construido para mi arquitectura :-(.

Así que, sencillo: Tomé el 'starknet-sierra-compile' del repo de cairo, que construí localmente, y lo reemplacé en el paquete python. Aquí hay un comando que debería funcionar:

```bash
cp cairo/target/release/starknet-sierra-compile ~/cairo_venv_v11/lib/python3.9/site-packages/starkware/starknet/compiler/v1/bin/starknet-sierra-compile
```

A continuación, inténtalo de nuevo declarando. Debería funcionar.

## Despliega tu contrato usando Cairo-lang
En este punto estás de vuelta en el flujo al que estás acostumbrado. Usando el hash de la clase que recibiste antes:
```bash
starknet deploy --class_hash 0x3ae6bb224f12a8818c3ad59cc248968e8ee3a3081ff805610141695381a28f7 --account version_11_test
```

![Graph](/im%C3%A1genes/deploysmart.png)

Monitoriza tu transacción. Si falla debido a la estimación de tasas, reinténtalo. 

![Grpah](/im%C3%A1genes/cairo1.png) ![Grpah](/im%C3%A1genes/cairo1.1.png)

Una vez que su transacción es `accepted_on_l2`.... ¡Enhorabuena! ¡Has desplegado tu primer contrato Cairo 1!

## Presumir en Twitter
¡No olvides presumir! Publica tu contrato desplegado [aquí](https://twitter.com/henrlihenrli/status/1638468939939282945)

Y no olvides añadir funcionalidades interesantes a tu contrato.
El equipo de Starknet edu lanzará pronto la versión Cairo 1 de [Cairo-101](https://github.com/starknet-edu/starknet-cairo-101). Mientras tanto, puedes obtener ideas de qué hacer con Cairo 1 con [Starklings-cairo1](https://github.com/shramee/starklings-cairo1)

## ¿Quiere saber más?
Si estás interesado en 
- Aprender a desarrollar cosas interesantes en Starknet
- Cómo funciona Starknet
- Qué es Cairo
- Cómo funcionan las pruebas Stark
Deberías unirte a Basecamp, nuestro programa de formación gratuito de 6 semanas para aspirantes a desarrolladores de Starknet. Las dos próximas cohortes comienzan en abril y puedes inscribirte[aquí](https://forms.reform.app/starkware/starknet-basecamp-registration-starting-jan-4th-copy-2/itvk4e) for a cohort in English, and [aqui](https://forms.reform.app/starkware/starknet-basecamp-registration-starting-apr-11th-copy/itvk4e) para una cohort en Español.