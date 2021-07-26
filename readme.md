
**[ARTICULO EXTRAIDO DEL BLOG](https://ivansantander.com/blog/index.php/2021/07/26/rick-morty-api-app-ionic-rick-morty/)**


**[EL SIGUIENTE EJERCICIO ESTA BASADO EL TUTORIAL DE FAZT](https://www.youtube.com/watch?v=hI5PJkXCKJs&t=125s)**


**Herramientas**
----------------

Angular

ionic

[Rick y Morty API REST](https://rickandmortyapi.com/api)

Postman

Visual Studio Code

1.Crear proyecto ionic blank
----------------------------

Después de tener todas las herramientas instaladas ejecutar el siguiente comando para crear una nueva aplicación ionic y seleccionar la opcion blank además tener en cuenta que **SI** se debe instalar capacitor y **NO** es necesario crear una cuenta.

    ionic start 01-app-rick-y-morty

2.Iniciar servidor de ionic
---------------------------

    ionic serve

3.Generar nueva pagina
----------------------

    ionic g page characters

4.Crear botón para ir atrás
---------------------------

    <ion-buttons slot="start">
       <ion-back-button defautlHref="home"></ion-back-button>
    </ion-buttons>

*   ![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-1024x373.png)
    
    _archivo characters.page.html_
    

5.Importar el modulo de HttpClient
----------------------------------

Importar el modulo en src/app/app.module.ts para poder usarlo dentro de las paginas

    import { HttpClientModule } from '@angular/common/http';

Una vez importado agregarlo dentro del @NgModule

      imports: [BrowserModule, IonicModule.forRoot(), AppRoutingModule,HttpClientModule],
    

![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-1-1024x641.png)

_archivo app.module.ts_

6.Importar HttpClient dentro de la pagina
-----------------------------------------

Dentro de character.page.ts Pagina donde se realizara la petición a la API, se debe importar y declarar el HttpClient dentro del constructor

    import { HttpClient } from '@angular/common/http'
    

el constructor quedaria de la siguiente manera para que en el ngOnInit pueda ejecutarse correctamente la peticion a la API

    constructor(
        private http : HttpClient
      ) { }

*   ![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-2-1024x708.png)
    
    _archivo characters.page.ts_
    

7.Realizar petición a la API
----------------------------

Dentro del ngOnInit y por medio de una petición http ya teniendo importado el modulo HttpClient realizar una peticion get a [https://rickandmortyapi.com/api/character](https://rickandmortyapi.com/api/character) y recibir los datos por el momento en la consola.

    ngOnInit(){
      this.http.get('https://rickandmortyapi.com/api/character')
       .subscribe(res=>{
          console.log(res);
       })
    }

*   ![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-3-1024x668.png)
    
    _archivo characters.page.ts_
    

8.Agregar lista con personajes
------------------------------

_Dentro de characters.page.ts_

Primero agregar un _<any>_ a la petición para no declarar cada una de los tipos de datos que se van a recibir puesto que es un ejemplo rápido.

    this.http.get<any>('https://rickandmortyapi.com/api/character')

Crear un array vacío antes del constructor donde se guardaran los datos de la petición

      characters = []
    

Con el array creado en el ngOnInit llenar el array con los resultados de la petición

    this.characters = res.results

![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-4-1024x949.png)

a_rchivo characters.page.ts_

_Dentro de characters.page.html_

Crear una lista en ionic con el avatar, id y el nombre esta lista debe estar controlada por un \*ngFor el cual le proporcionara los datos de manera correcta

    <ion-list>
     <ion-item *ngFor="let character of characters">
       <ion-avatar slot="start">
          <img src="{{character.image}}">
       <ion-avatar>
       <ion-label>
          {{character.id}}-{{character.name}}
       </ion-label>
     </ion-item>
    </ion-list>

![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-5-1024x968.png)

_archivo character.page.html_

9.Generar ruta a perfil de personajes
-------------------------------------

Generar una ruta en ionic es relativamente fácil solo se agrega la propiedad _\[routerLink\]_ dentro del ítem

    <ion-item *ngFor="let character of characters" [routerLink]="['/home']">
    

![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-6-1024x257.png)

_archivo profile.page.html_

Generar una nueva pagina llamada _profile_ en la cual se procesaran los perfiles de todos los personajes.

    ionic g page profile

En la ruta de la app agregar _/:id_ para permitir que la ruta de la pagina sea dinamica

    path: 'profile/:id'
    

![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-7-1024x299.png)

_archivo app-routing.module.ts_

al routerlink del _character.page.html_ concatenarle el id de cada personaje

    <ion-item *ngFor="let character of characters" [routerLink]="['/profile/'+character.id]" >

![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-8-1024x203.png)

_archivo character.page.html_

Generar un boton para ir atras

    <ion-buttons slot="start">
      <ion-back-button defaultHref='characters'></ion-back-button>
    </ion-buttons>

![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-9-1024x316.png)

_archivo profile.page.jtml_

10.Capturar id del personaje
----------------------------

Primero se debe importar la url del navegador importando la clase ActivatedRoute de angular/routes

    import { ActivatedRoute } from '@angular/router'

Para capturar el id primero debo instanciar la clase en una propiedad del constructor

    constructor(private activatedRoute:ActivatedRoute){}

y luego debo extraer el id de la propiedad mediante parámetros con snapshot extraigo una parte de la url y con paramMap busco el parametro establecido en el enrutador

    this.activatedRoute.snapshot.paramMap.get('id')

esto se lo puede mostrar por consola para obtener un resultado del id de cada personaje

*   ![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-10-1022x1024.png)
    
    _archivo profile.page.ts_
    

11.Peticion de datos del personaje a la API
-------------------------------------------

Importar clase _HttpClient_ y realizar la petición _get_ a la api concatenando el id consultado anteriormente el cual debe ser almacenado en una propiedad

    //Head
    import { HttpClient } from '@angular/common/http'
    //Propiedad
    profileId:string;
    //Dentro del ...
    constructor(private http:HttpClient){}
    //Dentro del ngOnInit
    this.http.get('https://rickandmortyapi.com/api/character/'+profileId)
    .subscribe(res=>{console.log(res)})

![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-11-1024x874.png)

_profile.page.ts_

12.Mostrar datos del perfil del personaje
-----------------------------------------

enviar datos al profile.page.html por medio de una propiedad llamada character y crear una tarjeta de ionic para mostrar los datos

    character;
    .subscribe(res=> this.character=res)

![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-12-1024x809.png)

_profile.page.ts_

Mostrar los datos por medio de una tarjeta ionic agregando la condicional _ngIf_ para evitar errores.

    <div *ngIf='character'>
        <ion-card class="ion-text-center">
          <ion-card-header>
            <img src="{{character.image}}" alt="">
            <ion-card-subtitle>{{character.species}}</ion-card-subtitle>
            <ion-card-title>{{character.name}}</ion-card-title>
          </ion-card-header>
        
          <ion-card-content>
            {{character.origin.name}}
          </ion-card-content>
        </ion-card>
      </div>

![](https://ivansantander.com/blog/wp-content/uploads/2021/07/code-13-1024x937.png)

profile.page.html

Con eso tendríamos una aplicación funcional falta agregar un grid para lograr tener mejor diseño en la web...