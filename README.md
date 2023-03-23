<div align="center">
<img alt="starknet logo" src="https://github.com/Nadai2010/Nadai-Maths-Starks/blob/master/im%C3%A1genes/Starknet.jpg" width="600" >
  <h1 style="font-size: larger;">
    <img src="https://github.com/Nadai2010/Nadai-SHARP-Starknet/blob/master/im%C3%A1genes/Starknet.png" width="40">
    <strong>StarkNet-Es - Guía desplegando Cairo 1.0</strong> 
    <img src="imágenes/Pionero.png" width="40">
  </h1>
</div>

## Desplegando tu primer contrato Cairo 1 en Starknet
Esta es una guía oficial de StarkNet-Es para la comunidad hispana, puede consultar repo oficial [aquí](https://github.com/starknet-edu/deploy-cairo1-demo) o el video de Henri [Desplegando Cairo 1](https://twitter.com/i/broadcasts/1OwGWwoewekGQ), que te ayudará a desplegar tus primeros contratos Cairo 1 en Starknet. Incluimos algunas explicaciones adicionales y capturas de pantalla para ayudarte a entender mejor el proceso.

Antes de comenzar, es importante que tengas una comprensión básica de lo que son Cairo y Starknet. Cairo es un lenguaje de programación de contratos inteligentes y Starknet es una plataforma de contrato inteligente sin permisos basada en la tecnología zk-rollup.

Con el despliegue de la [versión 0.11](https://starkware.medium.com/starknet-alpha-v0-11-0-the-transition-to-cairo-1-0-begins-30442d494515) en Starknet, ahora es posible desplegar tus primeros contratos Cairo 1. ¡Fantástico! Pero, ¿cómo hacerlo? No te preocupes, hemos creado esta guía aproximada para ayudarte en el proceso antes de poner en orden nuestra documentación.

---

## El flujo
Para completar necesitarás hacer lo siguiente:

- Instalar las dependencias necesarias, como Git y Python.
- Clonar el repositorio de demostración "deploy-cairo1-demo", la herramienta para compilar tu contrato Cairo 1.
- Instalar la última versión de Cairo-lang, la herramienta para interactuar con Starknet.
- Compilar tu contrato Cairo 1 en formato Sierra.
- Declarar tu contrato Sierra utilizando Cairo-lang.
- Desplegar tu contrato en Starknet utilizando Cairo-lang.
- Compartir tus logros en Twitter.

---
## Clone Repo
Primero, clona el repositorio de demostración `deploy-cairo1-demo` utilizando Git.

```bash
git clone https://github.com/starknet-edu/deploy-cairo1-demo
cd deploy-cairo1-demo
```

---

##  Instalar el compilador Cairo one
Para instalar el compilador Cairo 1, primero debes descargar el código fuente de Cairo desde el repositorio oficial y luego compilarlo.

Si es la primera vez que instalas Cairo, ejecuta los siguientes comandos:
```bash
git clone https://github.com/starkware-libs/cairo/
cd cairo
git checkout 9c190561ce1e8323665857f1a77082925c817b4c
cargo build --all --release
```

Si ya tienes Cairo instalado, ejecuta los siguientes comandos:
```bash
cd cairo
git fetch && git pull
git checkout 9c190561ce1e8323665857f1a77082925c817b4c
cargo build --all --release
```

En este punto ya tienes instalado Cairo en este repositorio. 

---

## Instalando Cairo-lang
Para instalar Cairo-lang, descarga la última versión desde el sitio oficial y luego instálala, para ello primero vuelva a la carpeta raíz del repositorio.

```bash
cd ..
```

Configura un entorno virtual python:

```bash
python3.9 -m venv ~/cairo_venv_v11
source ~/cairo_venv_v11/bin/activate
```

Si tenías cairo-lang instalado previamente, desinstálalo:
```bash
pip3 uninstall cairo-lang
```

Descarga la última versión de Cairo-lang [aquí](https://github.com/starkware-libs/cairo-lang/releases/tag/v0.11.0.1) la carpeta raíz de este repositorio. 

Instala Cairo-Lang:
```bash
pip3 install cairo-lang-0.11.0.1.zip
```
![Graph](/im%C3%A1genes/cairolang.png)

Para verificar que Cairo-lang se ha instalado correctamente, ejecuta el siguiente comando:
```bash
starknet --version
```

![Graph](/im%C3%A1genes/starknetversion.png)

---

## Compila tu contrato usando Cairo
Puedes usar [este contrato demo](hello_starknet.cairo) para completar esta prueba. Es un registrador de eventos muy básico.

Desde tu terminal, en la carpeta en la que instalaste Cairo
```bash
cd cairo
cargo run --bin starknet-compile -- ../hello_starknet.cairo ../hello_starknet.sierra --replace-ids	
```

![Graph](/im%C3%A1genes/compile.png)

Enhorabuena, ¡has compilado tus contratos de Cairo a Sierra!

---

## Declara tu contrato usando Cairo-lang
### Configurar variables de entorno
Antes de declarar tu contrato, debes configurar algunas variables de entorno en tu terminal. Ejecuta los siguientes comandos:

```bash
export STARKNET_NETWORK=alpha-goerli
export STARKNET_WALLET=starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount
```
![Graph](/im%C3%A1genes/export.png)

---

### Configurar una nueva cuenta para el tutorial

Usted necesita asegurarse de que su CLI starknet está configurado con un contrato de cuenta adecuada, y los fondos. Por seguridad crearemos un nueva cuenta. Para crear una nueva cuenta, ejecuta el siguiente comando:

```bash
starknet new_account --account version_11_test
```

![Graph](/im%C3%A1genes/wallettest.png)

Este comando creará una nueva cuenta de StarkNet y deberías obtener la dirección del contrato esperada. `Envíale 0.1 ETH` a la cuenta creada. Monitoriza la transacción de transferencia hasta el estado `pending`.

![Graph](/im%C3%A1genes/send.png) ![Graph](/im%C3%A1genes/send2.png)

- Una vez que ha pasado al estado `pending`, procede a ejecutar el siguiente comando:

```bash
starknet deploy_account --account version_11_test
```

![Grpah](/im%C3%A1genes/deploya.png)

- Este comando desplegará la cuenta creada. Monitoriza la transacción de despliegue, una vez que ha pasado al estado `pending` podemos seguir.

![Graph](/im%C3%A1genes/deploya2.png) ![Graph](/im%C3%A1genes/deploya3.png)

---

### Declarando el contrato
Una vez que hayas desplegado la cuenta, vuelve a la carpeta raíz del tutorial e intenta declarar el contrato. Si ya ha sido declarado, puedes omitir este paso. De lo contrario, declara el contrato utilizando Cairo-lang con el siguiente comando:


```bash
cd ..
starknet declare --contract hello_starknet.sierra --account version_11_test
```

¡Deberías recibir el hash de tu nueva clase declarada!

---

### Troubleshooting
Si no has tenido ningún error, puedes omitir este paso. Sin embargo, si has recibido un error al declarar el contrato, y se ha devuelto el uso de declarar, sigue los siguientes pasos:

```bash
Error: OSError: [Errno 8] Exec format error: '~/cairo_venv_11/lib/python3.9/site-packages/starkware/starknet/compiler/v1/bin/starknet-sierra-compile'
```
La razón es que Cairo-lang necesita compilar tu código Sierra y utiliza un binario rust importado llamado 'starknet-sierra-compile' para hacerlo. Pero en este caso, el binario importado no fue construido para esta arquitectura :-(.

Para solucionarlo, debes tomar el 'starknet-sierra-compile' del repo de Cairo, construirlo localmente y reemplazar el paquete Python. Aquí hay un comando que debería funcionar:

```bash
cp cairo/target/release/starknet-sierra-compile ~/cairo_venv_v11/lib/python3.9/site-packages/starkware/starknet/compiler/v1/bin/starknet-sierra-compile
```

A continuación, inténtalo de nuevo declarando. Debería funcionar.

---

## Despliega tu contrato usando Cairo-lang
Ahora, estás de vuelta en el flujo al que estás acostumbrado. Usa el hash de la clase que recibiste antes para desplegar tu contrato en Starknet usando Cairo-lang:

```bash
starknet deploy --class_hash 0x3ae6bb224f12a8818c3ad59cc248968e8ee3a3081ff805610141695381a28f7 --account version_11_test
```

![Graph](/im%C3%A1genes/deploysmart.png)

Monitorea tu transacción y, en caso de que falle debido a la estimación de tasas, inténtalo nuevamente.

![Grpah](/im%C3%A1genes/cairo1.png) ![Grpah](/im%C3%A1genes/cairo1.1.png)

Una vez que su transacción es `accepted_on_l2`.... ¡Enhorabuena! ¡Has desplegado tu primer contrato Cairo 1! Puedes verificar que el contrato se ha desplegado correctamente siguiendo este enlace: 

![Graph](/im%C3%A1genes/deploycairo1.png)

- [Contract Cairo 1.0](https://testnet.starkscan.co/contract/0x0479ba9b1aa1e2746214f0669c9057181a72edcfe1961afa33e01dac8ea22c83#read-write-contract-sub-write)

---
## Presumir en Twitter
¡No te olvides de presumir de tu logro en Twitter! Publica tu contrato desplegado [aquí](https://twitter.com/henrlihenrli/status/1638468939939282945)

Además, ¡añade nuevas funcionalidades interesantes a tu contrato! El equipo de Starknet edu lanzará pronto la versión Cairo 1 de [Cairo-101](https://github.com/starknet-edu/starknet-cairo-101). Mientras tanto, puedes obtener ideas de qué hacer con Cairo 1 en [Starklings-cairo1](https://github.com/shramee/starklings-cairo1)

---

## ¿Quiere saber más?
Si estás interesado en:
- Aprender a desarrollar cosas interesantes en Starknet
- Cómo funciona Starknet
- Qué es Cairo
- Cómo funcionan las pruebas Stark
- Únete a nuestra comunidad [StarkNet-Es Telegram](https://t.me/starknet_es)
- Síguenos en nuestro Twitter [StarkNet Español](https://twitter.com/StarkNetEs)
- Deberías unirte a Basecamp, nuestro programa de formación gratuito de 6 semanas para aspirantes a desarrolladores de Starknet. Las dos próximas cohortes comienzan en abril y puedes inscribirte [aquí](https://forms.reform.app/starkware/starknet-basecamp-registration-starting-jan-4th-copy-2/itvk4e) para la cohorte en Inglés, y [aqui](https://forms.reform.app/starkware/starknet-basecamp-registration-starting-apr-11th-copy/itvk4e) para la cohorte en Español.
