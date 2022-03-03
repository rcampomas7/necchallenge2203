# NEC Challenge. Raul Campomas

Este proyecto es una implementacion del requerimiento del NEC Challenge,
como prueba tecnica.

## Instalacion

Para su instalación, solo es necesario descargar el codigo fuente con
las facilidades que ofrece GitHub a efecto. Pulsando sobre el boton 
"Descargar codigo", y recibir el archivo ZIP que contiene los archivos
correspondientes.
Una vez descargado el archivo, extraer en la carpeta por defecto, o
cualquier otra de preferencia.

Se observan 2 carpetas luego de la extraccion del archivo,
BE para Back-end, y FE para Front-end.
Se precisa de Visual Studio 2019 o superior con el SDK para NET Core 3.1,
para poder cargar el archivo NEC.sln, que contiene toda la informacion
para generar el API con el simplifica la api de CoinMarketCap.

Para ejecutar el Front-End de aplicacion, que hace uso del Servicio API
de la carpeta BE, es necesario utilizar Visual Studio Code.

## Back End

El Back End, de la aplicación consiste en Web API que simplifica el acceso
al API de CoinMarketCap para consultar criptomonedas, operaciones, precios, etc.
CoinMarketCap es un API muy completa a estos efectos y esta pensada como una
herramienta para uso financiero profesional. NEC Challenge hace un uso 
restringido y simplificado de esta API, para crear una pequeña aplicacion
WEB que devuelve el equivalente a un monto arbitario dado de las siguientes
5 criptomonedas:

1. Bitcoin.
2. Tether
3. Ethereum
4. BNB
5. Cardano

Los EndPoints que conforman esta API son:

1. https://localhost:5001/api/NECApi
	
	Devuelve la cantidad de registros de datos Criptomonedas registradas en
la cache del servicio. Normalmente devuelve 0, en la primera carga, puesto 
a que se debe proporcionar una llave de API (o API KEY) que es un Identificador
Unico Global (GUID) que permitirá usar la funcionalidad de la API.

2. https://localhost:5001/api/NECApi/setkey/ (parametro key (GUID))
	
	Fija el API KEY para activar la funcionalidad de la API. Si no se proporciona
este valor, el cual es facilitado para cada usuario por los servicios de CoinMarketMap,
esta API no funcionara en ningun EndPoint.

3. https://localhost:5001/api/NECApi/resetmap

	Inicia internamente una tabla con los identificadores de las criptomonedas
para las operaciones que han de realizarse con ellas. Esta operacion debe
realizarse justo luego de /NECAPI/setkey y antes de invocar cualquier otro Endpoint.

4. https://localhost:5001/api/NECApi/getmap

	Devuelve la coleccion de todos los registros de monedas que registro /NECAPI/resetmap.
A los efectos de esta aplicación, son solamente las 5 mencionadas al principio de esta sección.

5. https://localhost:5001/api/NECApi/getPriceOfmap (parametros symbol)

	Devuelve el precio unitario de cada moneda reconocida por CoinMarketCap. A efectos de esta aplicacion se han utilizado las 5 monedas mencionadas, mas no está restringido a esas
monedas. Su uso en esta aplicación ha sido para calculos de montos dados los precios de las monedas en cada momento de consulta.
	
6. https://localhost:5001/api/NECApi/convertprice (parametros: amount, symbol y currencies)

	Devuelve la equivalencia de precios entre criptomonedas dado un monto.
El parametro amount especifica el monto de la moneda que se desea convertir.
Symbol representa la moneda a partir de la cual se haran las conversiones deseadas,
y currencies es una lista de las monedas a las que se desea realizar la conversion.

ejemplo:

	https://localhost:5001/api/NECApi/convertprice?amount=10&symbol=BTC&currencies=USD
	
	Es una consulta que devuelve la cantidad en dolares de estadounidense (currencies=USD) que equivalen a 10 (amount=10) Bitcoins (symbol=BTC).
	
Dependiendo del API KEY que se haya registrado con /setKey/, se podrá consultar la
conversion a varias monedas o una única por cada llamada.
El API KEY proporcionado indica a CoinMarketCap, el API subyacente, indica de la posibilidad de consultar una o varias monedas en cada llamado.

7. https://localhost:5001/api/NECApi/currentkey

	Devuelve el API KEY que se ha registrado en este componente WEB API, para usar los servicios de CoinMarketCAP. Este API KEY puede sustituirse en cualquiermomento llamando a
endpoint /setKey.
	
8. https://localhost:5001/api/NECApi/isactive.

	Determina si la WEB API esta lista para realizar operaciones. Devuelve un valor Booleano (true o false) para indicar si esta activa o no a tal fin.
Para activar la WEB API, debe proporcionarse por /setkey, un API KEY valida para poder realizar las operaciones. Si la actual API KEY, no es valida,
no podran invocarse los endpoints con resultados consistentes.

## Front-End

	El Front-End es una pequeña y simple aplicación implementada con Angular 13. Su diseño es simple y directo, y mediante el uso del componete HTTPClient proporcionado
por angular-cli, se invoca a la API descrita en la seccion Back-End.
	La operacion de este Front-End es simple. Se propociona una API-KEY valida en el cuadro de texto "Ingrese API Key", y se pulsa el boton "Establecer", para
registrar en el API del Back-end, la API KEY registrada. Si esa API KEY, no es valida, la aplicacion no funcionara, ni respondera a ningun comando.
	El cuadro de texto "Ingrese un monto en dolares (USD) a convertir:", espera un monto en dolares americanos, para ser convertidos a sus equivalentes segun las criptomonedas
listadas al principio de ese documento. Al mismo momento de ingresar el monto, se convierte automaticamente el valor en las monedas listadas. Los precios unitarios de las criptomonedas, tanto como su equivalente en USD, se actualizan cada 10 segundos.
