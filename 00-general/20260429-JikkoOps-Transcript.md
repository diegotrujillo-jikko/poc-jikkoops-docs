

jikkoops - April 29

VIEW RECORDING - 122 mins (No highlights)


@0:07 - Jaime Darío Guevara Viteri (Jikkosoft)
Mientras les voy a mostrar creo que la presentación. Bueno, pues mientras llega yo creo que
podemos ir charlando de algo.
Bueno, ¿se acuerdan de la reunión con Juan David sobre SIGIA, bueno, y todo este tema?
Jaime, ¿nos puedes compartir la presentación, por favor?
Ahorita que Juan David me diga, que no sé si le va a hacer cambios, entonces apenas me diga
ya.
Entonces, pues digamos que ya hay una iniciativa. Y para iniciar con este proceso del nuevo
ecosistema modular de gobierno inteligente, todo basado en un nuevo ADN técnico, no un
monolito gigante en un ecosistema de productos modulares diseñados para crecer, vamos a
trabajar en estos cuatro primeros componentes.
Y lo inicial que vamos a hacer y por lo que estamos en esta reunión es por Jikkosoft y Jikkosoft
es ese back office comercial y operativo, nuestro back office donde vamos a tener los clientes,
los usuarios, toda la parte administrativa que nos permita a nosotros vender, activar, medir,
facturar.
Desactivar las Feature Flags, bueno, todo lo que necesitemos ahí. Ya Juan David entra, pero
quería darles como esa introducción, porque la idea es que en esta reunión hablemos
netamente de Jikkop, según lo que entiendo, y logremos hacer algo como un Spring Cons,
## ¿cierto?
Para no ir como a ciegas con todo, sino que pudiéramos, tener la visión general y
dependencias de esto que vamos a hacer de Jikkops, un backlog modular por el proyecto,
briefs canónicos, esto es para los agentes que vamos a implementar desde el producto,
## ¿cierto?
Los ínculos autos definidos, y bueno, que en esta conversación... Todos hagamos preguntas
claves o, bueno, digamos que tratemos como de hacer todas las preguntas, todos los
cuestionamientos, todo lo que puede pasar, los riesgos que podríamos en una charla, pues,

una charla normal, para ver qué problema resuelven, si Jikkosoft, qué usuarios lo van a usar, si
se lo van a nosotros, qué entra primero, qué queda para después, qué datos produce esto, sea,
cuáles serían como estos outputs, cuáles serían los inputs, a qué sistema inicial va integrado, si
va integrado a todos los que siguen de ahí en adelante, cuáles son los subflujos críticos,
## ¿cierto?
Para que eso nos genere los MD, los .md, o el brief que necesitemos nosotros para esta nueva
dinámica de desarrollo tipo bytecoding que nos propone, pues, Juan David y el mundo en
general.
Bueno, ahí les dejo la introducción, si tienen preguntas, cuéntenme, igual ya entra Juan David
para ampliar. Hola muchachos, ¿cómo están?

## @9:45 - Juan David Lopez
## Hola Juan David.

## @9:46 - Sergio Raul Ospina Tello
## Hola Juan.

## @9:52 - Diana Plata
Muy bien.

## @10:05 - Juan David Lopez
Bueno, listo. ¿Quiénes estamos? Bueno, ¿cuál es la idea, muchachos? En este momento ya
tienen un poquito el concepto de lo que vamos a hacer con todo el tema, ¿cierto?
¿Alguna pregunta hasta ahora? ¿No? Listo. Baja, baja, baja, baja, baja, En este momento hay
unos, en este momento, esto probablemente está en revisión, esto no va ser, porque la idea es
hacerlo en paralelo.
Vamos a tener que hacer, y es lo que quiero que quedemos claros al principio, vamos a tener
que empezar a hacer, vamos a entregarle un proyecto completo a uno.
Dale, Laura. Yo sí tengo dudas.


## @10:56 - Laura Steffany Angulo Vergara
En la parte de lo que he revisado, más o menos. Tenemos pensado que los usuarios van a ser
no solo los clientes, me imagino, sino que también vamos a meter información sobre los
impuestos, sea, quiero saber cómo el panorama general, no solo los clientes, sino qué más
debería tener en esta parte de Jikkops.
Exactamente, para allá vamos.

## @11:19 - Juan David Lopez
Listo, qué buena pregunta, Laura. Es para allá vamos. Entonces, para él es como un consejo.
Dale, dale, Diego. Qué pena, antes de continuar.

## @11:27 - Diego Trujillo
Es que estaba analizando información antes de preguntar. Con respecto al diagrama de
arquitectura que me compartiste de agentes, ¿esa parte es solamente un pedacito de todo esto
que vamos a hablar o...?
Es un pedacito, es un pedacito, sí, ese pedacito de ceiling.

## @11:46 - Juan David Lopez
Bueno, realmente es un pedacito, es un módulo completo de liquidación, es todo el módulo de
liquidación. Claro, porque tú le dijiste macro módulo, ¿no?
Recuerdo. Sí, porque en ceiling vamos a tener, entonces es lo que yo les quiero explicar ahora.
Vamos a, vamos a, vamos a entrar a mí.
Si quieren yo comparto. Ok, listo. Me vas corrigiendo ahí, ¿qué es este de prototipo? ¿Ves?
¿Qué hace esto? Eh, eh,
¿Cuál es más? ¿Cuál es más hay? Dosia, Cilin, el otro es Socia.

@14:23 - Jaime Darío Guevara Viteri (Jikkosoft)
Revisando. ¿Cuál es más hay?


## @14:27 - Juan David Lopez
Yo tengo acá.

@14:30 - Jaime Darío Guevara Viteri (Jikkosoft)
Del núcleo inicial, Dosia, Cilin, Socia. De la otra fase, Despacho, Confía, Actia, Subsi, Vanes y
## Sigia. Listo.

## @14:54 - Juan David Lopez
¿Qué es que hace esta cosa?

@15:11 - Jaime Darío Guevara Viteri (Jikkosoft)
Vamos a hacer un Maimapso, PECO, PECO, PECO.

## @16:02 - Juan David Lopez
Entonces, dentro de esto, para que lo tengan como en cuenta, es más o menos así. Aunque
esto es, aquí serían tres módulos principales, que es liquidación, liquidación, aquí serían
ingresos, aquí serían expedientes, listo, nuevamente integraciones, listo.
Son como los, son como los principales, bueno, reportes, expedientes, pues está fiscalización,
está cobro coactivo. Gracias. Gracias. Gracias. Gracias.
Aquí en ingresos está facturación y recaudo, recaudo, cartera, ajustes. En liquidación está
parametrización y duración normativa para liquidación y liquidación.
¿No hay allí también causación, Juan? Sí, la liquidación es cuando, bueno, la facturación es
cuando causa. Es que es eso como módulos, como módulos.
La causación va por debajo. La causación es un proceso que cuando yo ya liquido, yo ya, eso
es prácticamente automático.
Si yo ya liquide y paso una fecha, automáticamente. Lógicamente esa información se pasa a la
causación, se pasa a la base de datos.

Lo más importante es el que requiere como atención humana, el resto es un proceso interno
automático. Falta acuerdos, acuerdos pagos.
Ese es algo que vamos a manejarlo. Listo, acuerdos de pago. Bueno, esos son los de CILIM.
Lo de dosia, de gestión documental, asignación de TRDs, más que todo consecutivos, caso de
firma digital.
Bueno, eso será como el proceso de dosia, que son temas de documentales, ¿no? Socia,
exporta a la ciudadano, Echadía, gestión de quejas, bueno, todo lo demás.
¿Listo? Bueno, lo que llaman buzón de correspondencia, ¿eso iría en doce?

## @19:13 - Diana Plata
Ese iría en socia, sí.

## @19:15 - Juan David Lopez
Ya sería socia el que lo tendría. Pensaría yo que sí, porque ahí donde va a estar, que la idea
más adelante es crear una empresa de correspondencia para no pararnos por las empresas de
correspondencia actuales.
Para nosotros es tener una persona exclusivamente para entregar temas del Estado. Págarle a
alguien para que vaya y físicamente le entregue, pero eso hay que hacer, eso es, en socia
serían las entregas.

## @19:52 - Laura Steffany Angulo Vergara
Notificaciones. Eso, notificaciones.

## @19:58 - Juan David Lopez
Quisiera notificaciones manuales. O digitales. Esta es una firma digital interna. Estas son
notificaciones digitales. Son dos cosas diferentes. Listo.
¿Entendido hasta ahí? ¿Qué es lo que va a manejar cada uno? Juan, y el tema de lo que
llaman administración de usuarios.


## @20:27 - Diana Plata
Ya voy para allá. Entonces, hay uno que es el papá de todos, que se llama el CIGIA.

## @20:38 - Juan David Lopez
Entonces, el CIGIA ya tenemos CILIN, o sea, ya adentro es CIGIA. CILIN, DOS, SOCIA adentro
es CIGIA, y DOCIA adentro es CIGIA.
¿Listo? Y acá es donde vamos a tener lo que llamamos IAM. Este es el que va a tener toda la
administración de usuario de todo.
¿Otra pregunta más? Y aquí también va a haber, es el tema más para Laura y Jaime, el del
alcalde, el dashboard del alcalde.
Este es el portal de alcalde, o portal de, no sé, que no solamente es alcalde, sino gobernador,
alcalde, alcalde, gobernador o presidente.
Entonces, este lo va a manejar la STIC y es el que le va a dar información a cada uno de ellos.
A cada uno de ellos. O sea, ese va a ser el G-Cops.

## @21:52 - Laura Steffany Angulo Vergara
No. G-Cops va a manejar, este, el IAM, es un sistema, es una administración de usuarios.

## @22:00 - Juan David Lopez
A nivel de software. Entonces, ¿qué va a pasar? ¿Cómo la interacción? Ahí te viene otro que
se llama G-Cops, que es el que estamos ahorita, vamos a hablar, que es G-Cops.
G-Cops es el que va a manejar los contratos de SIGIA. G-Cops va a tener a SIGIA y más
adelante, por ejemplo, puede tener a SUBSI donadores, por ejemplo.
SIGIA puede tener SUBSI identidad, porque SUBSI tiene dos componentes. Uno que es donde
vamos a traer donaciones del exterior y otro es donde vamos a identificar al donante.
¿Cómo identificamos al donante? Al donado. ¿Cómo se llama eso? Bueno, al beneficiario.
Entonces, ¿cómo identificamos al beneficiario? Con toda la información del Estado.
Ya sabemos cuánto impuesto gasta. Ya sabemos que es la gente pobre, ya sabemos que es la
gente necesitada, tenemos, al nosotros tener toda la información del Estado vamos a tener una

relación mucho más cercana al expediente como tal del ciudadano y determinar si ya está
recibiendo otros subsidios, por ejemplo, entonces para no darle más subsidios a esa persona.
Entonces, pero esa información se le va a dar al donante, a los donadores, el donador va a
decir, ah, ok, no, sí, es un software muy, o yo dono porque yo sé para dónde va el dinero y no le
están dando dinero a otras personas que no deben ser, esa es una de las dudas más grandes.
Entonces, Jikkosoft va a manejar toda la información de Exigía junto con la información de
Subsid, y si más adelante sacamos aplicación adicional, AppX, entonces ahí va.
Entonces, Jikkosoft va a manejar todo eso. Sija va a tener su portal interno en Jikkos,
Subsidonadores va a tener su portal, AppX.
X va a tener su portal. ¿Preguntas? ¿Dudas? Por ahora no. Entonces, como Jikkos va a tener
estos diferentes, va a tener unos módulos que son los módulos core de él, ¿sí?
Esos módulos core son los que vamos a ver ahora.

## @24:42 - Laura Steffany Angulo Vergara
¿Listo? Lo que yo estaba tratando de hacer acá.

## @24:47 - Juan David Lopez
Vamos a generar como una ruta de lo que va a pasar. Vamos a generar una ruta de lo que va a
pasar.
La ruta de lo que va a pasar ahora. Entonces, por ejemplo, la persona entra, digamos, esos son
los usuarios de, aquí van a estar los usuarios de Jikkosoft, aquí van a estar los usuarios de, por
ejemplo, aquí el core va a tener una plataforma que es de usuarios.
Estos usuarios son, estos usuarios son los usuarios de Jikkosoft, no son usuarios de la alcaldía
y nada. Jikkosoft le habilita, entonces Jikkosoft, ¿qué va a tener?
Va a tener un sistema CRM, bueno, primero un tema, un tema de, por acá.

## @25:44 - Diego Trujillo
Estos serían diferentes a los de IAM, ¿cierto? Sí, por eso, los de IAM, estos de IAM.


## @25:52 - Juan David Lopez
Son de los productos y core de Jikkosoft. Exactamente. IAM es directamente de SIGIA, o sea,
el departamento TIC. De la alcaldía, gobernación, de la nación, van a tener acceso al IAM y
este es el le va a asignar usuarios o permisos de los usuarios asilinados, o sea, una, por
ejemplo, secretaria de Hacienda, entonces, necesitamos un usuario de secretaria de Hacienda,
ah, ok, pase el documento o algo, etc.
Entonces, levantan, crean el usuario de la persona y ese usuario le asignan que puede entrar a
SILIN, que puede entrar a DOCIA y que puede entrar a SOCIA, por ejemplo, o solamente SILIN
y SOCIA, o SILIN y DOCIA nomás.
Entonces, con un solo usuario yo puedo entrar a varias alcaldías, o ver, a varias apps de la, de
## SIGIA.
Ese es el IAM, IAM de SIGIA, y aquí es solamente una administración de usuarios simple para
nosotros, es con roles y permisos y demás.
¿Por qué no? Realmente no es como tan. Aunque lo podemos crecer más adelante en caso de
que ya tengamos más aplicaciones, ¿no?
Entonces, este usuario va a tener acceso a la aplicación X para hacer esto, para hacer lo otro.
¿Qué se puede hacer?
Entonces, ahí tenemos que empezar a mirar qué podemos hacer en el tema de GCOPS. ¿Cuál
es la idea? ¿Cuál es el tema de GCOPS?
Vamos a tener, GCOPS. GCOPS es el revenue, es el revenue contract orchestrator, o es un
orquestador de contratos para el CIGIA System.
Conecta comercial, contractual, operaciones, financiero y la capa analítica de sus contratos y
de sus entidades. Podemos tener las entidades, no solamente son alcaldías y gobernaciones,
pueden ser DIAN, pueden ser gobernaciones, pueden ser alcaldías, pueden ser cualquier cosa.
Todo en un sistema. El código del sistema debe responder, bueno. ¿Qué clientes tengo yo y su
estatus? ¿Qué valor se le estoy generando a ellos?
¿El impacto? ¿Qué estoy usando? ¿Qué están usando ellos? ¿Qué es el consumo, digamos,
consumo? ¿Y dónde está la plata?
¿Sí? Ya les vamos a explicar cómo pueden ir mejorando ese aspecto de los límites que tienen,
digamos, los usuarios y los planes como tal.
Entonces, ¿qué va a tener? Un dashboard comercial, contratos, operación, facturación, inside,
documentos y configuración. Eso es algo que yo estaba empezando a hacer, pero ya lo quiero

hacer junto con todos para que empecemos a tener como la mentalidad de lo que tenemos que
hacer primero y que entre todos colaboremos.
¿Hubo una pregunta hasta ahora, muchachos? Sí, yo.

## @28:54 - Laura Steffany Angulo Vergara
En la parte de la configuración también vamos a incluir todo lo que tiene que ver con... Pues no
solo con los impuestos, con la configuración del impuesto, sino todo lo que pueda entrar,
multados, también va en ese módulo.

## @29:12 - Juan David Lopez
Sí, ese módulo, ¿cómo lo nuevamente era? ¿Configuración? Ahí tenemos que empezar a
mirar, porque sí, nosotros vamos a tener, ver, lo primero que vamos a hacer, y es lo que
estábamos aquí haciendo, que tengo el computador lento, tenemos plan, producto, la idea es
crear el producto, entonces ya lo vamos a explicar.
Tenemos un producto, aquí yo voy a poder ver los productos.

## @29:42 - Laura Steffany Angulo Vergara
Pues el producto no solo se abre con un producto digital, sino también con la configuración,
pues con la variable de la configuración, ya sea el impuesto, multa y demás, o sea, cómo
configurado los dos, ¿no?
Así es, entonces, ¿qué va a poder?

## @30:00 - Juan David Lopez
¿Qué va a poder ver aquí la persona? Aquí lo vamos a colocar acá. ¿Qué va a poder ver la
persona?
Va a poder ver los protected resources. Eso es un concepto nuevo, pero muy importante,
muchachos. ¿Saben qué son los protected resources?
## No.


## @30:25 - Laura Steffany Angulo Vergara
## No.

## @30:26 - Juan David Lopez
Bien. Entonces, resulta que cuando yo coloco un botoncito y coloco un botoncito acá, vamos a
ver si por acá te di...
No, pero... Bueno, si yo coloco un botoncito... que... Digamos que yo coloco un botoncito acá.
Listo. Coloqué un botón.
Entonces este es el botón de liquidar. Lo voy explicar de manera gráfica. ¿Sí o no? Este botón,
Laura, se vuelve una acción.
Esto es una acción.

## @31:17 - Laura Steffany Angulo Vergara
¿Sí o no, Diego?

## @31:21 - Juan David Lopez
Vale. Igual como, por ejemplo, un, no sé, el de una, bueno, va a haber acciones, me van a decir
el ejemplo de otra.
Otra que puede ser es un endpoint. ¿Sí? Yo creo que voy cerrando esto porque si no, no me va
a dejar.
Entonces, aquí yo estaba poniendo, me voy a de computador, me compré el MacBook Air, voy
a comprar el MacBook Pro ahora, tantas cosas que estoy haciendo.
Aquí está. Listo. Entonces, por ejemplo, vamos a ponerlo acá. Listo. Entonces, ¿qué sucede?
Aquí vamos a tener una, cada vez que se cree, cada vez que se cree un recurso, entiéndase
por un endpoint, entiéndase por una acción, una acción bottom, entiéndase por una vista, aún
es la otra, una vista, todo esto, cada vez que se vaya a hacer, que son cosas importantes, se
van a inventariar.
O sea, vamos a hacer, vamos a mirar, vamos a ver como botones, acciones, endpoints, que
son importantes, vamos a empezar a inventariarles y darles un código.

¿Dónde quedarían guardados? Ahí está, van a quedar guardados, todo eso queda guardado,
que le preguntémosle acá.

## @35:01 - Diego Trujillo
Esa es una pregunta que tengo y la otra es, bueno, me queda claro lo del endpoint vista, pero
¿cómo inventarías una acción?

## @35:09 - Juan David Lopez
La inventaría es de la siguiente manera. La seguridad está hablando por acá. Fathomico, listo,
acá está, acá directamente, como voy a inventar qué carga que está.
Cada acción va a tener una aquí.

## @37:51 - Diego Trujillo
Entonces allí me quedaría la duda dónde guardaría ese formato, ese código de permiso.

## @38:08 - Juan David Lopez
Por objeto de negocio, si quieres se puede hacer una matriz completa de protección para
silencio. Una matriz, ok. Si lo tenía ya, uno habla con Chachipiti y empieza a hablar, le tengo
que empezar a guardar todo eso, porque uno empieza una cosa a la otra, a otra y se va, y eso
yo ya lo había hablado con él.

## @38:26 - Diego Trujillo
Entonces ahí la pregunta sería, ¿la matriz qué es lo que recomienda? ¿Dónde la guardamos
para acceder a ella? O sea, te lo pregunto es porque sí se me hace familiar el concepto, pero
con relación a microservicios, en los que uno como inventaría los micros para, pues, más que
todo control y garantizar la compatibilidad hacia atrás, etcétera, pero no sé si estamos hablando
de lo mismo aquí, o similar.
Esto es más que todo, ¿qué va a pasar?


## @38:58 - Juan David Lopez
Todos esos, todos esos recursos. Recursos, aquí ya vamos para acá. Todos esos recursos van
a ser listados acá. O yo voy a entrar a producto y se me van a listar.
Listar todos los protected resources, ¿sí? Yo esos protected resources, ¿qué hago? Los agrupo
por funcionalidad. Y esa funcionalidad las agrupo por planes.

@39:47 - Jaime Darío Guevara Viteri (Jikkosoft)
Perdón, yo no estoy viendo, ¿será que se me pausó a mí o...? ¿Ustedes ven?

## @39:52 - Juan David Lopez
Sí, yo veo.

## @39:55 - Diana Plata
Voy a actualizar.

## @40:03 - Diego Trujillo
Juan, entiendo el producto de los Protected, pero ¿cuál sería el propósito?

## @40:10 - Juan David Lopez
El propósito es para qué, para que yo por ejemplo le pueda dar al caso de uso, yo le digo voy a
crear un producto que va a ser un plan gratuito, las funcionalidades van a ser la vista de la ST,
va a tener la vista de esto, va a crear esto, va a tener esto.
Para poder yo agruparlos, tengo que saber cuáles son los recursos que por aquí agrupan esa
funcionalidad. Entonces yo creo que se llama gratuito.
¿Sí? Claro. ¿Cogieron? Sí, sí, ya entendí. Jaime, Jaime, ¿sí? Sí, sí, yo los definí como
canónicos, pero sí, pues creo que es lo mismo.


@40:56 - Jaime Darío Guevara Viteri (Jikkosoft)
Ah, ok. ¿Qué son canónicos? Como el MD o el documento o la información como canónica que
es como segura que él puede ir a consultar y que eso es lo que ya se definió, que está definido,
como decir, el plan de, el plan premium tiene estas, estas condiciones, entonces cada vez.
Ese es informativo, el que tú tienes ahí. Sí, sí, sí.

## @41:24 - Juan David Lopez
Este ya es, esto ya es en G-Cops, o sea, a mí me van a aparecer, y es más, aquí pueden
aparecer tanto, tanto, me pueden aparecer recursos huérfanos.

@41:42 - Jaime Darío Guevara Viteri (Jikkosoft)
¿Qué significa?

## @41:43 - Juan David Lopez
De que cada vez que se va haciendo código, y eso se debe hacer ahí en, creo que cuando se
va a hacer despliegue y demás, la idea es que todos los recursos queden inventariados.
O bueno, por lo menos los importantes que hay en inventariados. Cuando esos importantes
crean nuevos, pues. Todos van a quedar, los nuevos van a quedar en huérfanos porque no
están en ningún, no están agrupados a ninguno.
Entonces aquí va a aparecer todos los listados protegidos con su funcionalidad agrupada. Este
es de esta funcionalidad, este es de esta funcionalidad, este es de esta funcionalidad.
Y estos huérfanos son los nuevos, las nuevas funcionalidades o los nuevos resources que no
están en ninguna funcionalidad. Crearon un botón, pero todavía ese botón quedó creado, pero
nadie lo va a poder ver hasta que le agreguemos la funcionalidad.
Inclusive podemos hacer varios de ellos y crear una funcionalidad nueva y venderlo como un
plan nuevo. ¿Sí me van a comprender ustedes muchachos?
Laura. Juan, ahí no sería primero agrupar por plan y luego por funcionalidad.

## @42:57 - Diana Plata

No, porque un plan son muchas funcionalidades.

## @43:00 - Juan David Lopez
Funcionalidades. Una funcionalidad son muchos recursos. Una funcionalidad podría estar en
varios planes.

## @43:11 - Diana Plata
Una funcionalidad puede estar en varios.

## @43:14 - Juan David Lopez
Sí, ¿no? Sí. Yo puedo crear un plan con tres, otro plan así, otro plan así, coloque el básico,
coloque este.
El plan al final es, al final yo puedo crear un plan y el plan va a tener precio. Por ejemplo, precio
y alcance.

## @43:32 - Diego Trujillo
Y me imagino que ahí entran a ser relevantes las acciones, porque de acuerdo al plan, pues
puede o no ejecutar una acción.
es.

## @43:41 - Juan David Lopez
Entonces, mire que ya me queda parametrizable todo, no me toca ahí. Vea, que desactivarle.
Entonces, inclusive aquí yo puedo hacerlo, mire, yo puedo hacerlo por, por ejemplo, le puedo
activar un plan.
Vamos a crear esas funcionalidades, pero esas funcionalidades... Eh... Por ejemplo, vamos a
crear un plan de expedientes. Vamos a tener expedientes, esto y lo otro, y ese plan usted lo va
a adquirir, y este agrupar planes, lo que yo puedo crear es una propuesta.
Entonces le puedo decir, mire, alumbrado, inclusive, previo alcance, aquí le puedo decir, lo
puedo agrupar por planes, mire, coja solamente el impuesto de alumbrado público, el módulo

de expedientes, pero no lo coja, el precio puede ser por expediente o puede ser por porcentaje
de recaudo, eso es un plan.
O yo puedo decir, no, yo quiero el de, yo quiero el de contratación. Por cantidad de usuarios,
porque ahí no hay recaudo, no porcentaje de recaudo.
Yo lo quiero por cantidad de usuarios a 50 usuarios y listo. Y quiero esta y ese es un plan
básico y tiene estas funcionalidades.
Plan medio tiene estas funcionalidades, plan avanzado tiene estas funcionalidades.

## @45:21 - Laura Steffany Angulo Vergara
¿Cómo entra la parte de la cantidad de, por ejemplo, de expedientes que pueden procesar?
¿Cuál sería esa parte? ¿La cantidad de expedientes que pueden procesar?

## @45:33 - Juan David Lopez
Sí. Claro, usted le puede colocar cantidad de expedientes.

## @45:37 - Laura Steffany Angulo Vergara
Pero eso sería otra variable. Otra variable, ajá.

## @45:41 - Juan David Lopez
Yo lo puedo hacer por cantidad de expedientes o por, o por, o híbrido. Cantidad de expedientes
hasta mil, gratuito, y después de ahí se empieza a cobrar el 30, vamos a decir, 30, el 20, el
10% del recaudo.
## Listo.

## @45:59 - Laura Steffany Angulo Vergara
## Gracias.


## @46:00 - Juan David Lopez
Caute primero, pague después. Entonces le entregamos mil para que lo haga y una vez que se
haga eso, entonces se le puede activar el de 30% y para adelante.
Por eso es importantísimo este Protected Resources, recursos protegidos, porque esto es lo
que nos va a dar la modularización de la...
Entonces, ¿qué va a pasar? Que esos recursos protegidos van a ser parte también del
aplicativo. Este aplicativo tiene estos recursos, este otro aplicativo tiene estos otros recursos y
todos son esos recursos.
Este puede liquidar, este puede hacer esto, este puede hacer lo otro, ta, ta, ta, ta. Y eso, todo
esto de aquí son una funcionalidad.
Todo esto de aquí son otra funcionalidad. Inclusive ya después de eso, ¿qué podemos hacer?
Que ya... ¿Cuándo le activemos lo que ellos compraron?
Esto es lo que ellos compran. No, yo compré la liquidación. No está diciendo qué usuario va a
utilizarlo. Solamente que compró todo el recurso de liquidación.
O sea, para esa entidad, el botón de liquidación va a estar activo. ¿Listo? Ahora, ¿qué va a
pasar después?
¿Quién me dice qué pasa después? Yo, la persona, yo le activé el plan, es un contrato, le
activé el plan a la alcaldía.
Sealing, quedó activado. Sealing, quedó activado. Obviamente, por ejemplo, hay un módulo,
hay módulos que son, exigía, debe estar, exigía como aplicación, debe estar instalada también.
O a se le va a dar a ellos, porque ahí dentro de sí.

## @47:47 - Laura Steffany Angulo Vergara
no, pregunta, ¿no debería estar primero ya ingresado el cliente dentro del contrato para poder
activar el plan? Claro, claro.

## @47:56 - Juan David Lopez
Esos son planes, ¿sí? Inclusive el... La propuesta no va directamente, esto va después en una,
esto va después en el proceso comercial, porque es lo que yo le voy a decir al cliente, ¿sí?

Yo llego hasta acá, hasta agrupar planes, entonces el producto dice, bueno, vamos a vender
esto así, así, así, así, más adelante pueden cambiar y decir, no, ahorita lo hacemos así, así,
así, los planes cambiaron, así, así, así, y los estudios, eso, así, así, ¿sí?
Ok, ok. Ya la propuesta es otra cosa diferente, eso ya al CRM, ese es, ese es el tema
netamente de producto.
¿Iba a decir algo, Jaime? No, no, no, ya no respondiste.

@48:55 - Jaime Darío Guevara Viteri (Jikkosoft)
Entonces, aquí, ¿qué vamos a hacer?

## @48:56 - Juan David Lopez
Solamente, entonces, cuando la persona, cuando hay un producto. Entonces, ese producto se
puede ir a testear con un CRM, que es para conseguir clientes, ¿sí?
Ahí ya vamos al proceso de visitas, visitas comerciales, ¿listo? Que ya sería un CRM interno de
Jikkos. ¿Qué va a tener ese CRM?
Oportunidades de negocio, va a tener la información, va a tener los leads, ¿sí? Aquí es donde
entraría el CRM, por acá.
Entonces, yo lo que puedo hacer primero es crear un producto, entrar a productos para mirar
qué productos tengo yo.
¿Qué productos tengo yo? Van a aparecer todas las aplicaciones, el CIGIA, CILIN, por ejemplo,
el... El de usuarios, pues el de usuarios, digamos, ese tema de recursos también lo podemos
hacer por usuarios, pero yo metería todos los recursos de usuarios siempre dentro de los
planes.

## @50:13 - Laura Steffany Angulo Vergara
¿Cómo un ítem más? Ese tiene que ir transversal.

## @50:17 - Juan David Lopez

El aplicativo SIGIA tiene que ir transversal y se le da un usuario para SIGIA, que le va a
aparecer la aplicación de SIGIA, la de DOSIA y la de las demás.
Entonces, él ya una vez que tenga eso y lo que adquirió el software, le va a aparecer resaltado.
Ah, mire, yo puedo, por ejemplo, vamos a hacer un plan gratuito.
No, alcalde, le colocamos el software. Listo. ¿Cuánto cuesta? Nada. Empecemos así.
Entonces, ¿qué le vamos a adquirir? Se le va a activar la administración de usuarios para las
## TIC.
Entonces, de las TIC va a habilitar el usuario para el de la, para el de, para el de qué?
Para el de la alcaldía puede ser el liquidado, el sugerente de liquidaciones y ahí empezamos a
hacer toda la parametrización con los agentes.
Entonces el de las TIC dice solamente puedo asignarle usuarios al de pre-liquidación porque
ese es el único que tenemos nosotros actualmente.
No tenemos liquidación, no tenemos facturación, no tenemos recaudo, no tenemos
expedientes, pero le pueden aparecer como para comprarlos. Entonces él ¿qué va a hacer?
Va a crear un usuario, el subdirector y dice no, mire, yo quiero el alumbrado público gratuito y lo
va a manejar Pepito Pérez.
Entonces créeme a mí y crea Pepito Pérez y dele acceso a Pepito Pérez. Entonces Pepito
Pérez va a hacer la parametrización y Juanito va a hacer la revisión de la pre-liquidación.
dele acceso, repito, dele acceso para crear el AST. Y al otro del acceso a la pre-liquidación.
¿Le puede dar acceso a la liquidación?
No, porque no fue comprado, no fue adquirido. Jikkos es el que maneja, que le habilita, Jikkos
le habilita por medio de sincronización todo lo que está activo de esa alcaldía, todos los
recursos que están activos.
Ah, activame el recurso X, yo te lo puedo habilitar desde Jikkos y automáticamente se te
habilita ya y ella se lo puede asignar a un usuario.
¿Entendido? ¿Preguntas? No, por ahora no.

## @52:45 - Laura Steffany Angulo Vergara
Entonces, cuando ya se hace la propuesta, ¿entra en el CRM? No, el CRM es que hace la
propuesta.


## @52:52 - Juan David Lopez
Ah, ok, entonces va después.

## @52:54 - Laura Steffany Angulo Vergara
Exacto, entonces aquí, una vez que ya tengo el producto, ¿qué puedo hacer después de que
tenga el producto?

## @52:59 - Juan David Lopez
¿Vendé? ¿Sí o no? Ya tenemos el problema. Ya tenemos los planes, los alcancen, los precios.
Obviamente, ¿esto qué va a hacer?
Esto toca una persona que solo tenga acceso acá, que más adelante puede ser una integrante
inicial que nos pueda decir, venga, usted está perdiendo plata, debería hacer esto y esto y esto
y esto y esto, agrupar esto de esta manera.
Le dunde OK y lo vuelve a agrupar. Por ahora, pues, como es un tema controlado, pues, la idea
es hacerlo un poco más controlado.
Además, no es tan grande en este momento como para meter un agente. Podemos hacerlo
manualmente e ir revisando. Una vez que se hace eso, pues, la parte comercial, ¿qué va a
traer?
Va a ser un pipeline comercial. Ese pipeline comercial, ¿qué va a traer? Pues, una propuesta. Y
una vez aprobado...
Y una vez aprobado, pasamos a la siguiente que es ya los contratos, que ese es otro módulo,
contrato firmado.
Entonces aquí en CRM yo voy a ver cómo va la contratación, quiénes hemos visitado, quiénes
hemos hablado, cuál es esta negociación, cuáles están próximos a darse, como todo ese
aspecto.
Se le genera la propuesta, las propuestas son diferentes. Ah, no, me gusta esa propuesta, sí,
cámbimela, sí, cámbimela, sí, no, no tengo esa plata, ¿por qué no hacemos?
Entregamos esta propuesta y ahí uno empieza a hacer una versión de propuesta y a ver cuál le
funcionó. Esa propuesta, una vez aceptada y firmada la propuesta, se pasa en contratos.
¿Qué pasa ahí cuando se pasan contratos? Ahí ya sí se crea el usuario con todas las
disponibilidades, ¿no?


## @55:22 - Laura Steffany Angulo Vergara
Ahí se crea un usuario.

## @55:25 - Juan David Lopez
Y como ya tenemos, mire lo importante que es esto, ya tenemos la funcionalidad de listadas,
los agrupada por funcionalidad y tenemos los planes que le vamos a entregar al cliente.
Cuando yo acepte la propuesta, ¿qué va a pasar? Se va a habilitar automáticamente todo de
una vez.

## @55:43 - Laura Steffany Angulo Vergara
¿Cómo se activaría, por ejemplo, los impuestos que seleccionen? ¿Es después del contrato o
desde el plan? Antes. Este es antes.

## @56:04 - Diego Trujillo
Es lo que queda en el contrato, ¿no? Firmado. El acuerdo.

## @56:14 - Juan David Lopez
Sí, lo que queda en el contrato firmado.

## @56:17 - Laura Steffany Angulo Vergara
Entonces, si solo, por ejemplo, si solo tomaron alumbrado público, todos los planes son con el
alumbrado público. ¿Qué pasa si quiere otro?
¿Retomamos otro contrato? Otro contrato. repite? Sí. Se llama un otro sí.

## @56:37 - Juan David Lopez

Ese es otro contrato. Ese es un otro sí. Tiene que tener la posibilidad de volver otra vez.

## @56:45 - Laura Steffany Angulo Vergara
Que como que se acumulen. Ok. Sí, sí, yo puedo ya contratado.

## @56:50 - Juan David Lopez
Yo lo que puedo hacer es una adición al contrato. Ah, yo quiero ahorita o hacer un contrato
nuevo, como sea.
Entonces, arma el contrato. Ese contrato tiene un número. Tiene una vía. Vigencia y ese
contrato queda activo por esa vigencia.
Aquí yo ya tengo control directo de lo que le vamos a entregar de manera automática. No es,
ay, vea, es que este contrato, lea las habilidades, párselo a la persona para que venga y haga
clic, clic, clic, clic, clic, ay, no, se me olvidó uno, no, me activo, ¿sí?
Esto es automático. Una vez que yo tenga la propuesta y la acepten la propuesta como yo la
estoy dando y el contrato quede firmado con esos planes, se le entregan el contrato los planes
y lo que estamos haciendo, automáticamente se habilita esa funcionalidad por ese periodo de
tiempo.
Se vence el contrato, las funcionalidades se van. O podemos dejarla solo lecturas también. Sí,
eso te iba a decir como apagado, más bien.
Sí, claro, como solo lectura, el contrato contiene también la vigencia. En día de la vigencia del
contrato, tipo impuesto, funcionalidades, funcionalidades, usuarios, cantidad de expedientes,
porcentaje, precio, etc.
Al aceptarlo como tal, si no existe un plan, pues tocaría volver a la mesa de trabajo para
crearlo. No, es que el alcalde quiere este, este, pero este no.
Ah, no tenemos ese plan. Creemos el plan, le pasamos la propuesta, y a una de las propuestas
se la pasamos automáticamente, como que haría la propuesta.
Él dice que sí, firma el contrato con las propuestas que le está, con las cantidades que le
estamos dando, y una vez que lo firme, pues se habilitan automáticamente y la vigencia del
contrato se habilita automáticamente.

## @59:12 - Laura Steffany Angulo Vergara

Pues eso sería un estilo como de plan enterprise que pues se configura y como personalizado.
¿Cómo así, cómo así?
Que ese estilo como que si no están los planes, se puede crear una última variante que sea
como estilo enterprise, donde uno va, personaliza como el plan, y cuando la acepta se vuelve
contrato.
Que no están los planes fijos, sino uno adicional. Sí, ahí yo puedo crear cualquier tipo de
contrato.

## @59:50 - Juan David Lopez
Esa es la ventaja que yo tengo haciéndole Protective Resources, por eso es demasiado
importante eso. Eso fue respecto a la investigación que yo hice, eso fue lo que descubrí.
Ese es nuestro pilar central de todo, porque esto es lo que nos da posibilidad de hacerlo por
impuesto, por impuesto, inclusive por expediente.
Yo puedo decir, solamente quiero fiscalización de ICA, cantidad de expedientes 5.000 y
después de ahí un 30% de recuperación de cartera.
Lo tengo por acá, aquí está, tengo por acá, lo voy a buscarlo. ¿Entendido hasta ahí? Es
importante ese Protector Resources, porque ese nombre está donde tengo esa categorización,
aquí.

## @1:02:05 - Diego Trujillo
Juan, con respecto a estos recursos protegidos, es que no he terminado de entender bien cuál
sería la diferencia con los permisos jerárquicos que uno conoce, como granulares, globales, si
me profundizas más ahí, por favor.

## @1:02:23 - Juan David Lopez
Otra vez, otra vez.

## @1:02:24 - Diego Trujillo
¿Cuál sería la diferencia de estos protected resources versus lo que uno conoce ampliamente
como permisos jerárquicos, granulares, o globales?


## @1:02:38 - Juan David Lopez
Aquí lo que estamos haciendo es, hazte de cuenta que le estamos haciendo un TAG a cada
acción que pueda hacer el software y a cada vista.

## @1:02:51 - Diego Trujillo
## Ya. Sí.

## @1:02:53 - Juan David Lopez
Entonces, una vista, con esos puntos sabemos que esta acción es de una vista. Sí, entonces
yo lo podría, yo podría agrupar, o sea, ya es naturalmente aplicado, agrupado por Vista, ¿sí me
entiendes ahí?
Entonces, al yo agruparlo todo, a mí me va parecer todo lo que puede hacer el software, o una
lista de las 500 acciones, de las 1000 acciones que hace el software.
Ok, ok.

## @1:03:23 - Diego Trujillo
Entonces, 1000 acciones, yo le puedo colocar permisos a las personas.

## @1:03:27 - Juan David Lopez
Mire, usted puede hacer 353 acciones de estas. Entiendo. Sí, o ya lo veo mucho más granular,
mucho más específico por cada acción que pueda, que pueda generar.
Ok. A ver si por acá. Acá lo tengo. Van haciendo preguntas, muchachos, mientras voy
buscando esto. ¿Tienen alguna otra pregunta?

## @1:04:35 - Laura Steffany Angulo Vergara
Yo, otra vez. Al final, cuando ya se tiene firmado el contrato y se activa todo, pues todo lo del
plan, se puede tener la posibilidad de pausar el plan, pues pausarlo, agregar otro.

¿Qué más podría ser? Sí, esos planes.

## @1:05:00 - Juan David Lopez
Esos planes se pueden guardar y se pueden también depreciar, o sea, que ya no, que ya no,
ya no, sepan.
Estoy buscando donde yo hablé de eso.

## @1:05:15 - Diana Plata
¿Es decir que los alcaldes van a tener, o los funcionarios van a tener la posibilidad tanto de ver
los planes que existen como de armar uno personalizado?
No, solamente lo hacemos nosotros.

## @1:05:29 - Juan David Lopez
Ah, ok.

## @1:05:30 - Diana Plata
Pero eso es Jikkop, Jikkop es solamente nosotros.

## @1:05:33 - Juan David Lopez
Es interno. Es interno, o sea, nosotros vamos a generar los planes. Nosotros somos los que
vamos a generar los planes.
Nosotros generamos los planes y le hicimos, ah, mire, tenemos estos y estos y estos planes.

## @1:05:45 - Diana Plata
Y él va a decir, ay, no, venga, yo quiero, me gusta este plan, pero me interesa la funcionalidad
de este otro plan.


## @1:05:52 - Laura Steffany Angulo Vergara
## Exacto.

## @1:05:53 - Juan David Lopez
Sí, o agrégueme esta funcionalidad y se le crea un plan nuevo al usuario. Ah, Un plan. Un plan
específico para X cantidad.
bueno. Y si nos gusta ese plan, lo ponemos ya como un plan general.

## @1:06:07 - Laura Steffany Angulo Vergara
Sí, se repite. Porque puede que se repita. Sí.

## @1:06:10 - Juan David Lopez
Ah, mire, es que yo le hice un plan a la alcaldía de X. ¿Le gustaría ese plan? Ah, sí, está
chévere.
Yo puedo hacer un plan de multas. Solamente cójame multas. Multas más impuesto predial.
Multas más impuesto. O yo lo puedo hacer por, o sea, yo, a ver, ¿qué pasa?
Una propuesta puede ser varios planes. No, los planes son más que todos nuestros. Somos los
que nosotros creamos. Y la propuesta es, es, ¿qué plan coge?
Ah, yo quiero un plan alumbrado público por porcentaje de recaudo o un plan alumbrado
público por expedientes. O un plan alumbrado público fiscalización por expedientes.
Eso se le agrega en la propuesta. En la propuesta va a decir, el plan de expedientes contiene
todo esto.
El plan de tal cosa contiene todo esto. Y abajo la, y abajo la propuesta, el valor de este. plan es
el 30%, este plan es así, este plan es así.

@1:07:20 - Jaime Darío Guevara Viteri (Jikkosoft)

Una pregunta, y en Jikkos, ¿quiénes entrarían? entraríamos... Nosotros. Pero también
tendríamos como algunos roles o eso, pues, lo digo, dejarlo abierto normal para cualquier
persona de Jikkos?
No, no, tiene roles y permisos, y usuarios. O que, ahí me pregunto, por ejemplo, ¿tributaria
tendría algo? O sea, cada como área tendría algo.
¿Cómo, cómo, otra vez?

## @1:07:51 - Juan David Lopez
Sí, no, el tributario no tenía nada que ver ahí, porque eso es un tema más de, es un tema más
comercial, más de comercial de producto.
Sí, porque por ejemplo, creemos un producto, entonces se sienta comercial y producto, es
decir, bueno, creemos un plan, lancemos, yo estoy viendo que la gente que le gusta más este
tipo de planes así, y un contrato puede contener varios planes, yo quiero el plan alumbrado
público, quiero el plan alumbrado público básico, quiero el plan industria y comercio avanzado,
avanzado significa fiscalización, significa ICA, significa retenciones, significa todo.
Yo puedo hacer planes diferentes, entre un contrato puedo hacer 5, 10, 20 planes. Ahora tengo
una duda. Sí.

## @1:08:36 - Laura Steffany Angulo Vergara
¿Qué pasa si uno de los impuestos yo no lo tengo configurado? ¿Quién es usted?

## @1:08:45 - Juan David Lopez
Jiko. ¿Jiko no lo tiene configurado? No existe.

## @1:08:49 - Laura Steffany Angulo Vergara
No existe, no existe, pero estamos hoy, ¿cómo lo vamos a hacer? Listo. ¿Cómo lo
configuramos? ¿Lo podemos configurar desde acá?
No, primero hay que hacerlo.


## @1:09:01 - Juan David Lopez
Entonces le decimos, Jaime, Jaime, la gente está pidiendo el impuesto de industria y comercio.
Ah, bueno, eso es así.
No, pues, hagámoslo, metámoslo como priorización. Entonces, Jaime tiene que hacerlo y una
semana queda terminado. Ya con esto, tres días ya queda terminado.
O terminamos. Y ahí, va a pasar? Ahí se van a crear recursos nuevos. Botones, acciones. Eso
es lo que estaba pensando.

## @1:09:27 - Laura Steffany Angulo Vergara
es nuevo. Exactamente.

## @1:09:30 - Juan David Lopez
Van a caer, van a caer al producto y van a quedar como funcionalidades nuevas. Entonces, yo
las puedo agrupar.
Van a crear recursos nuevos y yo las puedo agrupar en funcionalidades completas. ¿Y qué
pasa si lo agregamos desde acá?

## @1:09:44 - Laura Steffany Angulo Vergara
Que también tenga la posibilidad de yo, o sea, no yo más. Alguien que está viendo como todo
el bench y dice, no, este es el impuesto que más top de todo.
Lo vamos a agregar. No lo podría yo agregar de manera manual acá para que... Que una vez
me quede configurado con lo de los planes.
O sea, que también puedo hacer esa configuración. No, porque no existe.

## @1:10:07 - Juan David Lopez
Por eso, pero la voy a agregar nueva.

## @1:10:09 - Laura Steffany Angulo Vergara

Sí, pero primero hay que crearlas. Sí, pero que el sistema me da la oportunidad también de
crearlo desde acá.
O que puedo hacer todo.

## @1:10:23 - Juan David Lopez
Pues si usted la crea, es como si usted la crea y se desarrolla en un segundo.

## @1:10:29 - Laura Steffany Angulo Vergara
Pues con unos parámetros de pronto, pues básicos, podría empezar.

## @1:10:37 - Juan David Lopez
No, porque no es solamente, es que si dejamos que arriben funcionalidades, van a inventar
funcionalidades. Ah, yo quiero que me lea la cara y me diga si estoy enojado.
Y lo haga. Sí. Bueno, estoy botando a globos. Estamos dejando, no, aquí la idea es que
digamos hay funcionalidades que pueden estar pidiendo y eso entraría por el CRM.
Miren, también esta funcionalidad, esta funcionalidad. Eso le va a caer a Producto, pero tú
tienes que evaluar si esa funcionalidad va o ya casi va a salir o no va a salir, etcétera.
Y esa funcionalidad entonces saldría directamente a producción. Cuando ya está en
producción, se actualiza aquí los resources y entonces ya dice listo, ya está en producción.
Significa que yo puedo mandar a producción sin necesidad de habilitarle todavía eso a los
usuarios. Ok.

## @1:11:26 - Laura Steffany Angulo Vergara
Entonces lo que podríamos hacer más bien que sea que desde acá llegue la solicitud de, no
solicitudes, sino como vemos que no se está usando tanto esto, es mejor cambiarlo con esto,
de acuerdo al uso.

## @1:11:40 - Juan David Lopez

Si llegó un recurso, si llegó un recurso nuevo, que no, ese recurso puede estar dentro de una
liquida, ahí sí puede, yo lo puedo habilitar también.
Eso se llama Future Flags. Yo los puedo habilitar, no los puedo habilitar, yo puedo nacerlos sin
habilitarlos todavía, por ejemplo.
Entonces yo puedo montarlo en producción, pero no está habilitado. Yo puedo entrar a habilitar
el feature flag si yo quiero.
O sea, el feature lo puedo habilitar o lo puedo quitar. Pues ese el nivel de taller, pero chévere.
O sea, ese nivel de taller lo logramos si somos organizados inventariando resources, protected
resources.
Si nosotros no inventariamos eso desde el principio, todo eso no se puede hacer, no se puede
lograr. Por eso esa es la parte importante de la planificación.
No le puedo dar aquí a... Si yo hablé tanto con Chayipiti. Vamos a mirar a ver si está acá.
Otra pregunta, muchachos.

## @1:13:16 - Diego Trujillo
No, sigue siendo esa. ¿Dónde guardamos aquella matriz del inventario de los Protect
Resources? ¿O qué manejo le damos?

## @1:13:31 - Juan David Lopez
A ver si la puedo coger acá. Me fui muy atrás. Estoy haciendo las métricas de los ASTs, cómo
se van a quedar guardados, unas buenas métricas, miren aquí están, aquí están, entonces
tenemos, si yo quiero más adelante, tengo otro aplicativo del ecosistema de finanzas o de
contratación estatal que ya no tiene que ver con impuestos, solo lo podría manejar sin tener
dos bases de datos para cada aplicativo, entonces si he hecho PDC, ah bueno, aquí me está
diciendo PDC,
Sí, libre de finanzas, aquí empecé a hablar de esto, importante, no utilice todo el ecosistema,
scopes, aquí está hablando de scope, finanzas, es una capa transversal, access core, identity
platform, aplicaciones nuevas, módulos, roles, permisos, roles, permisos, y use respuesta clara
y corta, así puedes, tributario, finanzas, aquí más adelante está, no como.
No creo que me ha perdido esa conversación, pero bueno, yo me acuerdo de qué se trata, qué
es lo importante.

Listo, muchachos. No, aquí no está. Vamos a manejarlo en Jikkosoft. Aquí está sacando
solamente, netamente el... Bueno, aquí está.
Bueno, vamos a preguntarle acá, no sé dónde tenía toda la información que voy a organizar
mejor con esto. Creo que está acá.
Entonces, contratado. Mira, aquí está. Es este. Módulos, contratos, lo que el cliente compró.
Pantalla. Entonces, clientes, contratos, catálogo de productos, planes comerciales, features,
capacidades, lo contratado, límites de uso, revenue share, oportunidades comerciales,
facturación y monetización.
Y acá arriba, feature code, user seed, app code, CGL. Entonces, por ejemplo, aquí puedo tener
por tipo de usuario avanzado.
Métricas de usuario, que también pasa acá, temas de métricas de usuario, por acá hasta arriba
lo que hemos hablado de, por ejemplo, licencia por capacidad sin costo inicial, pero un
porcentaje de recaudo o híbrido también se puede hacer.
Este es, estaba viendo acá arriba, arriba, arriba, eso, este es. Ah, mira, aquí está, nivel de base
de datos y recomendación, Jikkofa, ya puede generar liquidación oficial predial, feature block,
liquidación cruyere, oficial, quema auditoría, quema integración, quema expedientes, quema
liquidación.
Ya, catálogo de maestro de features. Hay un feature catalog. ¿Qué tiene el feature catalog? ID.
El código de la aplicación, el código del módulo, el código de la feature, el nombre, la
descripción, el scope type, que es el impuesto, el status y quien fue creado, la funcionalidad, el
catálogo.
Acá arriba, salida con producir en expedientes, coro coactivo, planes, comercial, feature code,
input code, require action, feature API bandings, general liquidación, post liquidación en
general, allow.
Feature Catalog, Guardar Liquidación, Preliquidar, Catálogo Maestro, Contrato de
Comercialización, Cine Contrato de Feature, Sija Sincronización de Capacidades Activas, y
Sealing Consulta Copia Local Rápida para Mostrar, Ocultar, Bloquear.
Ese es el sistema del flujo para la copia de Runtam de Sealing. Esa es una de las preguntas
que tenías también, ¿cierto, Diego?
¿Cómo se hacía? ¿Dónde se guardaba ese Feature Catalog?

## @1:23:33 - Diego Trujillo

Entiendo que es el que agrupa y lo tiene, pero ¿dónde está almacenado ese Feature Catalog?
Eso creo que está armanece a una base de datos.
Pues a mí en el sentido común me dice que sí, y le metemos caché a él. ¿Qué pasa?

## @1:23:52 - Juan David Lopez
Aquí lo que dice, Sealing Rule Time, Feature Flag, ¿cómo se maneja? Jacobs define el
contrato, sincroniza este contrato. Una vez que se firma el contrato, él tiene que sincronizar con
## Sigia.
Sí, sí, no hay problema. Y después hace una copia local rápida para mostrar, ocultar, bloquear
lo que se puede hacer.
Sí, sí.

## @1:24:16 - Diego Trujillo
Tiene que haber como un sistema de chan, chan, o sea, arriba se genera.

## @1:24:20 - Juan David Lopez
Manda la parada de GKO, GKO, OPS.

## @1:24:22 - Diego Trujillo
GKO, OPS, exactamente.

## @1:24:24 - Juan David Lopez
Sincroniza a las demás y dice, mire, como ya se terminó el contrato, al otro día le manda y le
dice, prorumbájelas todas.
## Ok.

## @1:24:33 - Diego Trujillo

Y después al otro día uno dice, bueno, vamos a activarle, entonces, vamos a activárselos.

## @1:24:39 - Juan David Lopez
Entonces se la activamos y ya quedó activada. Vamos a darle un mes más de activación. Ah,
se bloqueó, Juan David, el pago y ese.
No, no, es que ya estamos sacando la cuenta, ayúdenos 15 días, listo, vamos a darle un mes.
Autorizamos, le damos un mes más.
## Ok.

## @1:24:55 - Diego Trujillo
Gratuito, un mes con, un mes más gratuito.

## @1:25:00 - Juan David Lopez
## Listo.

## @1:25:01 - Diego Trujillo
Entonces, en ese orden de ideas, Juan, ¿ese grupo de features lo definiría el área comercial,
producto o cómo?

## @1:25:09 - Juan David Lopez
Eso se lo define producto, pero tenemos que darles a ellos las, mira, aquí tenemos módulo
code, feature code, name, scope tags, app code.
O sea, esto es el módulo liquidador, el feature es preliquidad, en preliquidad va a haber muchos
protected resources. Ajá.
Sí, porque yo para poder, para poder liquidar, necesito, para yo poder liquidar, necesito, para yo
poder liquidar, necesito hacer el AST, paliar el AST, tener acceso al chat, por ejemplo.
Pero, inclusive, inclusive, yo le pregunté algo importante con eso, y es que cuando tengamos
ya la gente de inteligencia artificial,

Yo como usuario, como Secretaría de Hacienda, le puedo decir, quiero hacer la liquidación, y el
mismo chat le diga, ups, no lo tiene contratado.
Claro, porque él va y verifica qué es lo que hay, y si no lo tiene, el chat GPT, o bueno, el LLM,
no utiliza esa función.
¿Sí me va entender? Sí, sí. Entonces, por eso también es importante saber qué puede hacer y
qué no puede hacer, inclusive el tema de los endpoints.
Mire, por ejemplo, este, ingresos, mi recomendación es nombre de features para liquidar,
simular valores, flujo comercial perfecto, tal, tal, ¿Cómo se usa el frontend?
Cuando el usuario entra al backend. Devuelve su menú, botones permitidos. Ah, mira. Cuando
el usuario entra, el backend le devuelve su menú y botones permitidos.
Modo liquidador, tax predial, feature, preliquidar, enable, true, modo, gratis. Ustedes lo tienen.
General liquidación, enable, false. Modo, requiere pago.
Razón, módulo no contratado. Ejemplo de pago deshabilitado, tenan y de alcaldía, módulo
code, liquidador, feature code, general liquidación, tax code predial, status, disable.
Commercial mode, very required. Curso GCOPS. Lo que tiene activado cada entidad. Tenan,
feature flags. Liquidador, ejemplo, liquidador, preliquidador, liquidador, general liquidación,
general liquidación oficial, liquidador, aprobar.
Liquidación, aprobar liquidación. Aquí es donde tenemos que hacerlo muy bien hecho para que
cuando estemos programando, le digamos, o cuando estemos haciendo el bytecode, porque la
idea es empezar a hacer esto con el tema de bytecode, es empezar a hacerlo, es empezar a
darle esas instrucciones que toda acción, podemos hacer que toda acción, la inventaré.
Yo no la vería, no sé qué tanto, porque él dice que los recursos son protegidos, pero podemos
utilizar todo, capability activada, mire, ten en expediente, coro coactivo, predial, activa
expedientes, coro coactivo, predial, la solución correcta, aplica.
Sealing no debe preguntarle a Jikkops en cada request, esto mata la plataforma, la solución
correcta es, tienes que tener una tabla Sealing o zigiam como, tengan capabilities, Jikkops,
eventos, ZIGIA, Sealing, tengan capabilities, tengan alcaldía, producto, expediente, coro
coactivo y predial, lo que compró, ¿tú lo ven?
Ahí yo pregunto, ¿y esa batería de capacidades contratadas la debemos poner en Jikkops o en
dónde? Buena pregunta, aquí donde muchos sistemas se enredan después, la respuesta corta
es si, la fuente de verdad va vivir en Jikkops, pero no puede depender de Jikkops en tiempo
real, para operar.

te explico cómo hacerlo bien y por qué. Separación correcta de responsabilidades. GCOPS,
back office, comercial, debe manejar contratos, planes, suscripciones, capacidades vendidas,
fecha, inicio, fin, facturación del SAS.
Alcaldía de Palmira compró expedientes, coro coaltivo predial, inició tal fecha y fina tal fecha.
GCOPS es la fuente de verdad contractual.
CIGIA, CILIN, Runtime Operation, debe manejar. ¿Qué puede usar realmente el sistema en
tiempo real? Validación de acceso. Y Feature Flash Activos, que lo podemos desactivar
también, ¿no?
Toda una funcionalidad la podemos desactivar. O podemos montarla desactivada e ir
activándose. Silly no debe preguntar a Jikkops en cada request, eso mata el performance y
acopla todo.
Tienes que tener una tabla de Silly, en Silly, no o SIGIA y AEM, o como TENAN Capabilities.
Aquí está, esta era la pregunta tuya.
Vas a ver cuál es la capacidad de cada uno. Y esa matriz de capacidades la debemos poner
aquí arriba, expediente.
Y queda el espacio de esta conversación. Lo que yo le digo, puede ser liquidador. Ingresos y
expedientes. Dentro de expedientes hay diferentes tipos de expedientes, que son
determinación, discusión, coro coactivo, fiscalización, sancionatorios y cada uno por impuesto.
Me gustaría venderlos por cada tipo de impuesto y por cada tipo de expediente. Ejemplo, una
alcaldía solo compra coro coactivo, impuesto prioritario y solo puede beber eso.
¿Cómo se podría hacer? Voy a analizar, voy a aterrizarlo como diseño comercial, permisos,
datos. Si puedes hacerlo, la clave es manejar CILIN con licenciamiento por capacidades, que
es lo que usted ha estado hablando ahí.
Entidad, módulo, tipo de expediente, impuesto, alcaldía de Palmira, módulo, expediente, tipo de
expediente, coro coactivo, impuesto predial, estado. Entonces, por acá le quería mostrar los
recursos.
Expediente como servicio obligatorio de cartera. Cliente solo compra expediente, sistema
externo a la alcaldía, conector adaptador, servicio de obligación y cartera, módulo de
expediente.
Cliente compra CILIN completo, CILIN facturación. Ah, se le estaba preguntando que qué
pasaba, CILIN completado a otro. Digamos, S3 o otro software.
SILIM, módulo liquidador y capas de integración. No debes partir SILIM entre vasos
obligatorias. Módulo comercial desacopable. Aquí estaba preguntando.

Bueno, aquí comencé con el tema de la modularización. ¿Qué le pasó? Vamos a ver qué me
dice acá. Quiero que me...
No despertó. Vamos a ver si recuerda esta conversación. Entre normales de la HGPT, más
empiezan como a variar. Aquí está.
Arquitectura apuesta de la FreeShip Flags administra desde Jikkos. Jikkos administra contratos,
planes, features, límites y modelos. Y aquí, apps del ecosistema, ceiling, dosia, socia,
contratación, valían localmente features, límites y permisos.
Principio base, una feature flag no debe ser solo un botón, debe representar una capacidad
comercial funcional. Ejemplo, preliquidar, generar liquidaciones, generar coro coactivo,
gestionar.
O sea, el feature es toda la funcionalidad, que el feature contiene muchos recursos, tanto como
vistas, o como botones, o como endpoints.
La feature puede activar menús, vistas, botones, endpoints, procesos automáticos, límites
comerciales y modelos de cobro. Capas de arquitectura, Jikkops, administración comercial,
aquí se configura app vendibles, módulos, features, planes, contratos, límites, revenue share,
usuarios incluidos, expedientes incluidos.
Jacob responde preguntas como ¿qué compró la alcaldía? ¿Por cuánto tiempo? ¿Con qué
límites? ¿Con qué modelo comercial? ¿Qué features están incluidas?
CIGIA Core, Entirements Centrales. CIGIA Core debe ser la fuente central de derecho de uso.
Tenant, App, Módulo, Feature, Impuesto, Tipo de Expediente y Estado.
Alcaldía Palmira, SILIN, Expedientes, Gestionar Cobro, Predial, Cobro Coactivo, Activo. SILIN,
Room Time, Ejecución Rápida. SILIN no debe preguntar a Jikkops en Carrequed.
SILIN debe tener una copia local. Room Time, Tenant, Feature, Flags. Este Tenant puede usar
esta feature. Este impuesto está habilitado.
Este tipo de expedientes está habilitado. Ya superó el límite. El usuario tiene permiso. Pases de
datos recomendadas. Commercial Apps, Commercial Modules, Commercial Features,
Commercial Plans, Commercial Plan Features, Customer Contract, Contract Items, Contra
Limits, Contra Revenue Share Rules y Contra Events.
CIGI Accord debe tener Tenants, Apps, Modules, Taxes, Expedientes Type, Feature Catalog,
Tenant Entailment, Tenant Entailment Limits, Tenant Entailment Revenue Rules, Users, Tenant
User, Tenant User Apps, Roles, Permicios, Roles y Permisos.
E-Silling, Runtime.TenantFutureFlare, Runtime.TenantFutureLimits. Ahí te va, te respondemos a
pregunta, Diego.


## @1:36:26 - Diego Trujillo
Sí, desde el Feature Catalog, empieza a tener más sentido.

## @1:36:34 - Juan David Lopez
Ok, modelo de Feature Catalog, aquí está, Feature Catalog, ID, App Code, Module Code,
Feature Code, Name, Description, Scope Type, Feature Type, Feature Type es los diferentes
tipos de funcionalidades.
Scope Type, Global, App, Module, Tax, Expediente Tipo y Tax, Expediente Type. Feature Type,
Business Capability, Limit, Commercial Model, UI Control, API Control Automation, Ejemplo,
SILIM, Liquidador, Preliquidar, TAX, y Business Capability.
SILIM, Liquidador, General Liquidación, ¿qué tipo de TAX y qué capacidad tiene para eso? Ya
sean expedientes, cantidad de expedientes o valor.
Modelo Tenant Entailment, ID, Tenant ID, App Code, Module Code, Feature Code, Task Code,
Expediente Type Code, Status, Commercial Mode, Commercial Mode, Status, Active, Disabled,
## Expired, Suspended.
Modelos comerciales, Free, Paid, Capacity, Revenue Share, Hybrid, Unlimited. Son los modelos
comerciales. Ejemplos de gratis. Alcaldía de Palmira, App Sealing, Módulo, Liquidador, Feature,
Preliquidar, porque en el módulo hay dos features.
Puede estar la feature de preliquidación. Y está la feature de liquidación. Tax predial, status
active, commercial mode free. Entonces aquí tenemos la alcaldía de Palmira, la aplicación,
módulo de liquidado, la feature es preliquidar y el tax es predial.
El status está activo y está en el modelo comercial gratuito. Ejemplo, pago bloqueado. Tenan,
alcaldía Palmira, ceiling, liquidador, general liquidación, predial, pago requerido.
Commercial pay, pay. Ejemplo, coro coactivo contratado. Tenan, alcaldía Palmira, ceiling,
expediente, gestionar coro coactivo, predial, expediente tipo coro coactivo, status active,
capacidad comercial, commercial capacity.
Límites comerciales. Ahí vienen límites comerciales. Tienen intactos límites. Y de tener intactos
límites y de limit type, limit value, period type, reserve policy.
Tipos de límites. Active user, active case file, porque le dije que podemos tener, podemos
hacerlo y decirle que usted puede tener hasta 30 mil expedientes activos.

O que si hay uno que ya se archiva, que le queda uno para la alcaldía. Entonces puede entrar
otro.
Para eso, en CILIM hay que hacer algo que se llama expedientes propuestos. Que cuando ya
está bloqueado por la cantidad de expedientes, vaya a cambiar una base de datos intermedia,
o una cola, perdón, donde va haciendo todos los expedientes propuestos.
Entonces el día de mañana le dicen, y ahí vienen las oportunidades de negocios en Jikkos.
Entonces le decimos, vea, usted tiene 30 mil expedientes en ese aumento activos, pero usted
tiene 20
Ahí vienen límites comerciales. Tienen intactos límites. Y de tener intactos límites y de limit
type, limit value, period type, reserve policy.
Tipos de límites. Active user, active case file, porque le dije que podemos tener, podemos
hacerlo y decirle que usted puede tener hasta 30 mil expedientes activos.
O que si hay uno que ya se archiva, que le queda uno para la alcaldía. Entonces puede entrar
otro.
Para eso, en CILIM hay que hacer algo que se llama expedientes propuestos. Que cuando ya
está bloqueado por la cantidad de expedientes, vaya a cambiar una base de datos intermedia,
o una cola, perdón, donde va haciendo todos los expedientes propuestos.
Entonces el día de mañana le dicen, y ahí vienen las oportunidades de negocios en Jikkos.
Entonces le decimos, vea, usted tiene 30 mil expedientes en ese aumento activos, pero usted
tiene 20
20,000 que vienen en camino y esos 20,000 son 35,000 millones de pesos. Compre el plan
para poder o espere a que esto se acabe y va generando el proceso o compre todo el plan y
entonces ya los puede ingresar.
Esa es otra opción. Por ejemplo, limit type, active case file, limit value, 5,000, period type
contract, ejemplo usuarios por app, app code, contratación, feature code, user seat, limit type,
active user, limit values, para ceiling ilimitada, app code ceiling, feature code, user seats,
commercial model, unlimited.
En ceiling necesitamos cantidad de usuarios, en ceiling necesitamos cobrar.

@1:41:44 - Jaime Darío Guevara Viteri (Jikkosoft)
O el modelo comercial de ceiling no son por cantidad de usuarios.


## @1:41:47 - Juan David Lopez
Entonces, ilimita la cantidad de usuarios. Nosotros cobramos por expediente o cobramos por el
recaudo que tenemos. Uso y consumo, runtime, feature user counters.
ID, Tenance, ID, App Code, Module Code, Feature Code, Tax Code, Expediente Type Code,
Usage Metric, Current Value, Limit Value, Period Start, Period End, Last Calculated Ad.
Tenance, Alcaldía de Palmira, Feature, Gestión de Coro Coactivo, Tax, Predial, Expediente,
## Tipo, Coro Coactivo, Usage Metrics, Activar Caso, Files.
Por ejemplo, aquí podemos hacer, creo que podíamos llegar a hacer un feature hasta antes de
embargo. Y en embargo pararlo.
Por ejemplo, es un ejemplo. ¿Aquí está Jaime todavía? Sí, señor. Es decir, vamos a hacer la
etapa, vamos a hacer el feature que sea una etapa de determinación, otro de discusión, otro en
la subetapa de auto de medidas previas.
Entonces tenemos una etapa que se llama auto de medidas previas. Ese podría ser uno. Y
después hasta llegar al embargo, ese es otro.
Otro feature type, gestión de embargos. Entonces solamente le entregamos gratuitamente, le
podemos hacer todo el proceso y se lo entregamos hasta autoarchivo.
Ya está el autoarchivo, pero para generación de embargos tiene que comprar el software. Aquí
también lo tenemos en cuenta.
Este, por ejemplo, alternativa preventiva, 80% usado, 90% usado, 100% usado, bloqueo o
upgrade. Revenue share, 1099 revenue share, este es cuando es porcentaje.
En tal de medir, revenue share percentage, billing metrics, applies to status, feature, gestionar
cobro coactivo, tax predial, expediente tipo cobro coactivo, revenue share purchase, 8% de
cobro coactivo, le podemos poner.
Expediente case revenue tracking y en ceiling, expediente punto case revenue. Revenue
Tracking, Case File ID, Payment ID, Amount Recovered, Revenue Shared Percentage, Data
From Free Amount, Created At, UA Bindings, ID, App Code, Model Code, Switch Code, View
Code, Component Code, Feature, General, Preliquidar, View, View Liquidador, Preliquidador,
## Component, Button Calcular, Behavior Enable.
Este es el UI Bindings. Este es para mostrar y qué no mostrar en el UX, imagino.

## @1:44:40 - Diego Trujillo

Deserve with AppShell, AppShell, API Bindings, Room Time Feature, aquí también. O sea, el UI
es también para saber qué se va a mostrar y qué botones se pueden mostrar.
Por ejemplo, hay un botón de reportes. Le vendemos todo, pero vendemos los botones de
reportes. Eso no, eso no.

## @1:45:00 - Juan David Lopez
Aunque el botón esté, por ejemplo, en expedientes, si no compro el módulo de reportes, ese
botón pertenece a reportes y no lo vamos meter dentro del plan.
Ah, usted quiere el plan de reportes, compre el plan de reportes. Pero ya hasta allá podemos
llegar. Flujo de sincronización, Jikkos, crea contrato, modifica plan, contract, event, SIGIA, core,
consume eventos, actualiza tenants, entitlements, emite entitlements change, SIGILIN
sincroniza ronetime.tenant, feature flags, SIGILIN valida localmente.
¿Te hace sentido, Diego? Sí, desde hace, desde el principio empezó a... Ah, sentido. Aquí ya
se expande más, pero con el features catalog me empieza a bastar.
Cada operación debe pasar. Por un servicio, feature access service, can access, tenant ID,
user ID, app code, model code, feature code, task, code expediente type, code, task.
Entonces, JSON, false, limit reach, upgrade plan, has alcanzado el límite de expedientes
activos para coloco activo predial. Regla final de autorización, tenant activo, app activa, feature
activo, impuesto habilitado se aplica, tipo de expediente habilitado se aplica, límite no superado
se aplica, usuario con permiso.
No basta con feature activa, no basta con permiso de usuario, deben cumplirse en ambas
capas, entitlement commercial y permiso operativo.
Esto lo entregamos nosotros y el permiso operativo lo entrega el IAM, la persona de las TIC de
cada alcaldía, de cada gobernación, entrega el permiso operativo y nosotros entregamos el
tema comercial.
Caso preliquidador gratis, ng-coops, plan, preliquidador gratis predial, feature, preliquidar, tax,
predial, commercial mode free. En CIGIA, tenant entitlement, ceiling, liquidador, preliquidar.
¡Gracias! ¡Gracias! Predial Active, ¿quién lo ven? En Sealing, botón preliquidar activo, botón
generar liquidación oficial, bloqueado, ahí desbloqueamos ese botoncito.
Backend, POS, preliquidación es calcular permitido, POS, liquidación es generar bloqueado. Y
eso hay que hacerlo desde el backend, porque si no alguien lo puede, alguien lo puede va a
pasear.

O sea, que el mismo, el mismo, el mismo endpoint no, no deje, no esté activo. Caso, solo coro
coactivo predial, ceiling, expediente, gestionar coro coactivo predial, coro coactivo.
Puede haber expedientes, coro coactivo de predial, fiscalización predial, coro coactivo ICA,
liquidador oficial, ingresos, alertas comerciales, Jikkos debe leer, consumos y oportunidades.
Tipos, límite 80%, límite 90%. Límite Rich, High Block Value, Contract Expired. Este límite y
este límite son de avisos, alertas comerciales.
Alcaldía de Palmira tiene 4.850, 5.000 expedientes activos. Alerta, contactar para ampliación.
Esto nos ayuda para nosotros contactar. También en el caso de la persona, mire, ya a ustedes
se le van a, ya a ustedes antes le quedan 500.
Venga, les ayudamos para que aumenten más. Hay una plata ahí también que se les va, se les
puede perder.
O por ejemplo, detector de oportunidades. Encontramos que en el ICA, ustedes pueden
generar 10.000 expedientes con 80.000 millones de pesos.
Contraten el módulo de ICA. Se detectaron 5.200 en posible evasión de ICA. Fiscalización de
ICA. Opportunity Engine conectado a Feature Flags.
Las oportunidades no deben crear expedientes de una. Primero se registran. Expedientes,
Case Opportunities. Detecte elegible bloqueado feature, bloque limit, ready to create, create.
Si no tiene módulo, Block Feature. Si tiene módulo pero superó el límite, Block Limit. Si no tiene
Revenue, Share Limitado, Ready to Create.
Administración desde Jikkops. Jikkops debe permitir crear features, agrupar features en planes,
definir límites, definir modelo comercial, activar, desactivar por cliente, ver consumo, ver
histórico contratado, ver oportunidades bloqueadas, generar alertas comerciales.
Pantallas mínimas, catálogo de apps, catálogo de, aquí están las pantallas, Laura, catálogo de
apps, catálogo de módulos, catálogo de features, planes comerciales, contratos, entailment por
cliente, límites y consumo, revenue share, oportunidades, alertas comerciales, histórico
contractual, auditoría.
Conclusión, arquitectura debe quedar así.

## @1:49:43 - Diego Trujillo
Jikkops administra y vende. CIGIA Core centraliza derechos.


## @1:49:48 - Juan David Lopez
Ceiling Runtime ejecuta rápido. IAM controla usuarios y permisos. Opportunity Engine detecta
valor comercial. Audit deja trazabilidad. Este diseño te permite vender a una alcaldía solo por el
liquidador.
Gratis, cobro coactivo predial con 5.000 expedientes, fiscalización ICA por porcentaje de
recaudos, SILI limitado, pero con contratación con 10 usuarios, módulo por impuestos,
expedientes, fichos y modelo comercial.
Y todo queda administrarle desde Jikkops sin tocar código ni desplegar de nuevo. ¿Cómo lo
ven, muchachos? Buenísimo. La respuesta a todos nuestros problemas.
Eso nos falta que lo ejecutemos. Listo. Y, ¿qué, muchachos? Vamos a entregarle todo este
proyecto a Diego y a Diana.
Vamos. Empecemos. Yo también los voy a acompañar al principio, pero la idea es que
tengamos esto. ¿Qué vamos a hacer entonces de lo que hemos estado aquí inventando
cosas?
bueno, porque ustedes también tienen que decir, a ver, ustedes como son los de arriba, como
ustedes van a empezar a...
Arriba, con GCOPS, ustedes normalmente tienen que empezar a tener las reglas y decirles,
miren, no me están entregando o no están nombrando bien los botones, o tener como ese
PROM para, o ese punto MD donde especifique, siempre se le ponga al que está desarrollando
ceiling, por ejemplo, yo en esta casa estoy desarrollando ceiling, liquidador, yo me voy encargar
de liquidador, ¿listo?

## @1:51:21 - Diego Trujillo
Ustedes encárguense de este GCOPS y vamos viendo cómo nos va llegando.

## @1:51:25 - Juan David Lopez
Entonces, ustedes me van a decir a mí, hey, Juan, necesito que los usuarios para darles
permisos siempre estén así en esta forma, o los botones estén así nombrados.
¿Sí me hago entender? Entonces, a medida que yo vaya haciendo, tenemos que hacer un
sistema, lo podemos preguntar acá, para que cuando yo cree un botón acá, se actualice un
inventario.

No sé si eso lo hace, yo creo que ahí me estaba diciendo que era en la despliegue, como ¿Con
## Lint?
¿Se sabe qué es Lint? ¿Un código L-I-N-T? Sí. ¿Qué significa? Es el que pedí que analiza
código estático. Eso es lo que yo conozco, ¿no?
Una sección en B-S-C-O-B. Pero no entendería la relación aquí. Porque analiza el código, el
nombre de herramientas de programación utilizada para detectar código sospechoso o confuso.
Yo creo que con pruebas de Lint se hacen eso. Para detectar que hayan acciones que no
tengan nombre. O que tengan un nombre que no deben ser.
Y que estén bien estructuradas para poder ser prendidas desde arriba. Porque si la
funcionalidad abajo no está bien estructurada, el de arriba no lo puede prender ni apagar.
Les voy a pasar.

## @1:52:58 - Laura Steffany Angulo Vergara
Me gustó mucho.

## @1:53:00 - Juan David Lopez
Quiero que pongas esto mismo en un archivo .md, que le voy a pasar ese archivo. ¿Qué quiero
hacer con este archivo, Laura?
Echémosle también mano a IUD Design. O la idea es que le voy a entregar esto para que
jueguen con ustedes.
Ahorita voy a entrar a cada uno. Ustedes miren, así pueden hacer la base. Esto es la idea de
hagamos la base de datos, nos hace la base de datos.
Y ahí podemos empezar nosotros a empezar como a hacer todo el proceso de esto. Pero lo
importante que es mirar el tema de UX, que podamos realmente desde UX poder hacer todo lo
que queremos hacer, ya como un diseño como tal.
Sí, señor. Entonces, vamos a pasar esto. Yo voy a aquí descargar arquitectura de feature flags,
si quieres, ah mira, diseñar diagrama CS4, arquitectura de eventos y secuencia, voy a dejarlo
como es desde el PowerPoint, vamos a ver diseñar diagramas, ya lo bajé, mira, voy a enseñar
los de mermaid para que los pueda pegar directo en el ME, en cloud design, los voy a separar
por visión C4, eventos,

Runtime, datos y luego comercial. Trabajemos con la idea que ellos nos pueden ser más
rápidos y pueden alucinar, pero nos va a ahorrar demasiado trabajo, muchachos.
A veces es mejor equivocarse una hora que trabajar cinco días en algo perfecto. Más bien
trabajar una hora que se equivocó y volverlo a hacer y la otra se equivocó y la otra hora ya lo
hice bien.
Fueron tres horas perdidas, pero avancé un mes. En cambio el otro es, no, no, como una hora
perdida no quiero perder la hora, voy a hacerlo yo solo y así se demoró una semana.
Lo hizo bien, pero muero una semana. Aquí me equivoqué dos horas y lo hice en tres. Aquí le
voy a pasar este documento a ustedes porque esta es la primera parte.
Esta es la parte más importante de Jikkops. Este es el core central, pero yo no entiendo por
qué. Dice, Jikkos, SIGIACOR, ah, bueno, aquí lo trajo todo, alerta, ta, ta, ta, pero al final la
acción es permitido, solo si, permitir vender por módulo, por impuesto, por expediente, por
capacidad y por porcentaje, resultado de arquitectura de Jikkosoft, listo.

@1:56:58 - Jaime Darío Guevara Viteri (Jikkosoft)
Le voy a pasar, te voy a pasar esto. Espero que sea demasiado lento esto. A ver si dejo de
compartir.

## @1:57:07 - Juan David Lopez
¿Ahí me escuchan? Sí. Sí, señor. Les voy a compartir esto a ustedes. Se lo voy a compartir
aquí.

## @1:57:17 - Diego Trujillo
Tengo un grupo aquí en producto para que se los comparta a ellos, porfa. Coja eso y le quiero
poner, déjeme el entio de este computador.
Ahí me lo enviaste a mí, se los mandó en el chat.

## @1:57:42 - Juan David Lopez
Sí, envíaselo en el chat a ellos, sí. Si quiere, haga un chat conjunto, porfa, de este primer. ¿El
de la lanchadora?

El de la lanchadora. Ya tenemos uno de la lanchadora. Ah, pero en WhatsApp o qué? Sí, en
WhatsApp. Ok.

## @1:58:07 - Diego Trujillo
¿Cuál está la estructura de datos?

## @1:58:10 - Juan David Lopez
Esa la estructura de datos. Juan, una pregunta, me queda claro lo que vamos a hacer y cuál es
su propósito, pero la pregunta es, ¿no tenemos en este momento repositorio o alimentos
técnicos para hacerlo?

@1:58:30 - Jaime Darío Guevara Viteri (Jikkosoft)
Empecemos a pensarlo, sí, toca hacerlo. ¿Hay libertad para poderlos empezar a aplicar
conforme a nuestro criterio?

## @1:58:41 - Juan David Lopez
Pues, no, hagamos, hagamos una, hagamos, digamos, sí, sí la idea es que nos nos peguemos
al alineamiento que tenemos en la arquitectura, pero Freddy, Freddy, ¿qué te dijo?
Es que es lo que te iba a preguntar, que no tenemos alineamiento arquitectural. O sea, yo lo
que tengo ahorita solamente ha sido el drama que me mostraste a agentes.
Eso es lo único que he visto en arquitectura. Ajá. Jaime, ¿qué te dijo Freddy? ¿Tú le dijiste?
Ahí lo invité, pero no.
Pues no, como está operado, dijo que iba a estar por allá. Claro, listo. No, no, es que no sabía
si debía, si estaba...
No, él tiene incapacidad. Sí, sí, pero no sabía si ya le había acabado la incapacidad. ¿Hasta
cuándo tiene incapacidad?
Eran cinco días, ¿cierto, Sergio? A partir del lunes, creo. Sí, son cinco días. Entendería que ya
el lunes ya estaría.

Pero usted le mandó la invitación. Ajá. Ok. Listo. Entonces, ¿cuál es la idea, muchachos? Yo
voy a hacer este, este sería para mí...
Empecemos a pasar. Hágase una carpeta conjunta. Para que tengamos toda esta información
también, conjuntamente.

@2:00:11 - Jaime Darío Guevara Viteri (Jikkosoft)
## Listo.

## @2:00:12 - Juan David Lopez
Aquí le voy a pasar a... Y quiero que con esa información que tú tienes, saques... Ahí te tengo
que salir a una reunión, pero entonces vaya sacando...
Vamos a hacer dos cosas, muchachos. Una, pues ya hay hora de almuerzo, pues ustedes van
a almuercer. Y después de la hora de almuerzo, una.

@2:00:33 - Jaime Darío Guevara Viteri (Jikkosoft)
Diego y Diana, miren, ver, para configurar todo con Clauco, con toda la información. Y etcétera,
háganse en una prueba pequeña.

## @2:00:42 - Juan David Lopez
Pongámoslo ahí, echémosle candela a esa prueba pequeña ahí. Iremos a ver cómo se puede
trabajar entre los dos. Si hay necesidad de trabajar entre los dos o trabajar solamente uno.

## @2:00:53 - Diego Trujillo
Piénsenlo, ver, hagan pruebas.

## @2:00:56 - Juan David Lopez
Nos vemos ya al final de la tarde para mirarlo. Yo voy a estar entrando, si quieren me
comparten, el link para yo verlo, y ese es por un lado.

Y por otro lado, quiero crear con Cloud Design, quiero echarle candela a Cloud Design para
empezar a armar la interfaz gráfica, por lo menos a tipo de mockup.
¿Alguien levantó la mano que escuchó por ahí? No, acabé de mandar el mensaje, por favor,
ahí para, como no tengo el número de todos, en ese link pueden registrarse en el grupo, entrar
al grupo de WhatsApp.

## @2:01:46 - Diego Trujillo
¿Alguna pregunta?

## @2:01:51 - Juan David Lopez
No por mi lado, me regalas un minuto al final, Juan. Dale, dale, listo.

## @2:01:58 - Diana Plata
Cree la cita. Entonces pido el celular. Sí, dale. Hasta ahora. Listo, muchachos. No sé si, ¿qué
edad me llama Diego?
Por el celular. O es algo que me a mostrar acá. Aquí, aquí, rapidito. Los dos, nada más. Sí,
pero como la gente hay que esperar que se vaya.
Ya te llamo entonces. Dale, listo. Nos vemos entonces. Chao, Buen día. Chao, chao.

