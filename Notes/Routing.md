### Lecturer 124 Folder For refrence of these notes

- In In our app, we got three sections 
    - Home
    - Server 
        - View and Edit Servers
        - A Service is used to load and update Servers
    - Users
        - View users
- This app will be improved by adding routing but definitely feel free to play around with it, besides routing, everything should be working fine.
- Currently upon loading our app in the browser it looks like the image shown below
     ![This is before we made any changes to the code](Routing_Image1.png )
- Now we are going to talk about the code
- In the app component we are loading all of our three components that is app-home, app-users and app-servers as shown below
    ```ts
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
            <ul class="nav nav-tabs">
                <li role="presentation" class="active"><a href="#">Home</a></li>
                <li role="presentation"><a href="#">Servers</a></li>
                <li role="presentation"><a href="#">Users</a></li>
            </ul>
            </div>
        </div>
        <div class="row">
            <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
            <app-home></app-home>
            </div>
        </div>
        <div class="row">
            <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
            <app-users></app-users>
            </div>
        </div>
        <div class="row">
            <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
            <app-servers></app-servers>
            </div>
        </div>
    </div>
    ```
- For the time being we are going to ignore the sub components such as edit-server and server and we will later nest these components together.
- So in the above shown code we can see that only one of the role listed in the list which can lead us to that particular link after clicking on them and that to dynamically.
- Now these routes are responsible for our whole app that is everywhere in our application if we enter /users meaning that we want to load the users component as our main page lets say. So since this is core part of our app where should we register it, well the app module sounds like a good place because here we configure our app that is we add all our components and so on and it is good place to inform angular about the routes our application has.
- So here above the @NgModule decorator we are adding a constant named appRoutes which should be of type Routes which should be imported from @angular/router. This is the type we are informing angular we are giving the routes some structure.  
    ```ts
    const appRoutes: Routes = [
        {path: 'users'} //localhost:4200/users
    ]
    ```
- The const should hold an array because we are going to have multiple routes. And each route is just a javascript object in this array. But now the question is how the route should configure in this array.
- The structure in which the routes should be return contains a path (*the path is something which gets entered in the url after your domain*) and path should be a string in our case 'users' as shown above.
- Now we should define what should happen when this path is reached that is we haven't attached any action to it. The action typically here is a component, so we inform angular that once here this path is reached a certain component should be loaded and this component will be the page which gets loaded.
    ```ts
    const appRoutes: Routes = [
        {path: 'users', component: UsersComponent} //localhost:4200/users
    ]
- Similarly we can have other pages that is server and many more. But here we want a empty path that is when we just hit the localhost:4200/ and there we want to load our HomeComponent which is the starting page.
    ```ts
    const appRoutes: Routes = [
        {path: '', component: HomComponent},
        {path: 'users', component: UsersComponent},
        {path: 'servers', component: ServersComponent} 
    ]
- But writing all this won't do anything that is the angular won't understand what have we written in this appRoutes because this is just a constant and nothing more. We have to tell angular that we have created an array of routes which it needs to make use of and for that purpose inside the appModule.ts we have the imports array in it we need to add RouterModule which gets imported from @angular/router package.  
- So with this added we are adding the routing functionality to our app but still our routes are not registered that is why this RouterModule has a special method we can call **RouterModule.forRoot()** which allows us to register some routes for our main application.  
- This forRoot will receive the name of the array in which we have stored our routes which in our case is appRoutes as an argument and with that our routes are now registered in our angular app on this router module which gives us this routing functionality.
    ```ts
    //the below import is inside the imports array of the app.module.ts
    RouterModule.forRoot(appRoutes)
    ```
- Now in our app.component.html we have used our selector that app-home to show our page. But as we have used the angular router we doesn't require to use them like that. We need to add a special directive shipping with the angular that is **router-outlet**.  
- This router-outlet looks like a component but still it is a directive. And this now simply marks the place in our document where we want the angular router load the component of the currrently selected route.
    ```ts
    // this is inside the app.component.html
    // this was before we used router 
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <app-home></app-home>
    </div>
    // this is after using the router
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <router-outlet></router-outlet>
      // do it same for servers and users
    </div>
    ```
- And now when we hit the ng serve and the localhost:4200 gets loaded we can see our HomeComponent. And upon changing the url manually that is by writing localhost:4200/users we can go the desired pages.
- As of now our links are not working and we need to enter the url manually to vist the particular page. So now we need to make sure that our links also takes us to the pages as our url does.  
- As we go our app.component.html we can see that we have some list in the form of links as shown below. 
    ```ts
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <ul class="nav nav-tabs">
        <li role="presentation" class="active"><a href="#">Home</a></li>
        <li role="presentation"><a href="#">Servers</a></li>
        <li role="presentation"><a href="#">Users</a></li>
      </ul>
    </div>
    ```
- In the first list we can see that there is anchor tag with # to be precise.we can change these anchor tags to the desired pages we need as shown below.  
    ```ts
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <ul class="nav nav-tabs">
        <li role="presentation" class="active"><a href="/">Home</a></li>
        <li role="presentation"><a href="/servers">Servers</a></li>
        <li role="presentation"><a href="/users">Users</a></li>
      </ul>
    </div>
    ```
- Now here we can go to the pages upon clicking on the links, but the problem here is as soon as we click on one of the links it reloads the total app. And we just don't want our app to be reloaded as it will just waste making use of angular framework. As the angular is use to create the single page applications and here the whole app gets reloaded so this is not a right practice.  
- So we need to use a special directive that angular gives us which routerLink. 
- Now routerLink is just able to parse a string some could pass just a /. Now this will tell angular that element here on routerLink is placed will serve as a link in the end but it will handle the click differently.  
    ```ts
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <ul class="nav nav-tabs">
        <li role="presentation" class="active"><a routerLink="/">Home</a></li>
        <li role="presentation"><a routerLink="/servers">Servers</a></li>
        <li role="presentation"><a routerLink="/users">Users</a></li>
      </ul>
    </div>
    ```
- Now upon clicking the links , it gives us the components but it doesn't refresh the whole application, because routerLink catches the click on the element prevents the default which would be to send a request and instead analyzes what we passed here to the routerLink directive then parses it and checks if it finds a fitting route in our configurations which it ofcorse does because we have defined them in our app.module.ts. And this is how routerLinks works.
- Now if we change our routerLinks that is if we remove the slashes from our routerLink as shown below.  
    ```ts
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <ul class="nav nav-tabs">
        <li role="presentation" class="active"><a routerLink="/">Home</a></li>
        <li role="presentation"><a routerLink="servers">Servers</a></li>
        <li role="presentation"><a routerLink="users">Users</a></li>
      </ul>
    </div>
    ```
- Upon removing the / , the click on the links works fine. But now if we go to our servers.html and add a routerLink as shown below then and here we add the link for the servers page again and upon clicking that Reload page link we will get an error
    ```ts
    <div class="col-xs-12 col-sm-4">
    <a routerLink="servers">Reload Page>
      <app-edit-server></app-edit-server>
      <hr>
      <app-server></app-server>
    </div>
    ```
- This error occurs here because it does not found the route servers/servers. Now this error won't occur if we add / in front of the servers routerLink in the above servers.html code which will convert it to a absolute path from a relative path. And now upon clicking on Reload page no error will occur. So with a relative path it always appends a path you specify in the routerLink to the end of your current path and your current path depends on which component you are currently on.
- The meaning of a realtive path means without slash it tries to apend the path we speccified in the routerLink to the end of your current path. And the current path depends on which component we are currently on.
- That is why we can remove it in our navigation where we also use the relative paths because that is in our root component, this component is not loaded through the router so this root component always sits at localhost:4200/nothing. So this is always at our root level which is why we can use our relative paths in the links whuch are getting loaded via our appRoutes.
- But one layer below that for example when we loaded a route which gets loaded when we are only at /servers andif we try to load a a relative path here then it will try to load servers/servers

#### Styling our tabs

- We just need to add css to ourlinks which use to swutch tabs for this we will remove the class active from the link to the HomeComponent and we will try to set it dynamically And angular provides a specific directive fro it which is **routerLinkActive** directive
- The **routerLinkActive** directive can be addedto the wrapping element like a list item or we can even add it to a link itself as shown below.
    ```ts
    // this is inside app.component.html
    <ul class="nav nav-tabs">
        <li role="presentation"><a routerLink="/" routerLinkActive="active">Home</a></li>
        <li role="presentation"><a routerLink="servers">Servers</a></li>
        <li role="presentation"><a routerLink="users">Users</a></li>
    </ul>
    ```
- Now providing active to link dosen't do anything when using bootstrap so we need to provide it to the list itself as shown below.
    ```ts
    <ul class="nav nav-tabs">
        <li role="presentation" routerLinkActive="active"><a routerLink="/">Home</a></li>
        <li role="presentation" routerLinkActive="active"><a routerLink="servers">Servers</a></li>
        <li role="presentation" routerLinkActive="active"><a routerLink="users">Users</a></li>
    </ul>
    ```
- Here now the routerLinkActive directive analyzes the currently loaded path and then checks which links leads to a route which uses this path and by defaukt it marks the element as active  as it adds this css class if it contains the path you are on. So for the servers and the users case the above explained is the case.
- But for the /nothing link which is forst one the home page, we always have /nothing at the beginning. In simple words the first link is common in other two links which a slash. Still we don't want home being marked as active all the time and for that we need another directive **routerLinkActiveOptions**.
- In routerLinkActiveOptions we need to use the property binding because we have to provide js object to it where we use exact and set it to true. So here exact is kind of reserved property on this object you passed to routerLinkActiveOptions and this will now tell angular only add this css class if everything is just a /.
    ```ts
    <ul class="nav nav-tabs">
        <li role="presentation" routerLinkActive="active" [routerLinkActiveOptions]="{exact: true}"><a routerLink="/">Home</a></li>
        <li role="presentation" routerLinkActive="active"><a routerLink="servers">Servers</a></li>
        <li role="presentation" routerLinkActive="active"><a routerLink="users">Users</a></li>
      </ul>
      ```
#### Navigate

- Lets now create a button in the home component html which will lead us t o the servers page using navigate method provided by the angular router. The navigate allows us to navigate to a new route and here route is defined as an array of the single or diffrent elements of this new path.
    ```ts
    // html of home component
    <button class="btn-btn-primary" (click)="onLoadServers()">Load Servers</button>

    // ts of home component
    constructor(private router: Router){ }// imported from @angular/router 

    onLoadServers(){
        this.router.navigate(['/servers']);// here we have use the absolute path
    }
    ```
- Now what if we want to use the raltive path inside the navigate route array that is if we add another button which will again load servers page but with the relative path that is without the slash as shown below
    ```ts
    // this is inside servers html
        <button class="btn btn-primary" (click)="onReload()">Reaload Servers</button>
    // this is inside servers.ts 
    constructor(private serversService: ServersService, private router: Router) { }

    onReload(){
        this.router.navigate(['servers']); // no slash that means its an realtive path
    }
    ```
- Here upon clicking on the Reload Servers button we won't get any error even though we used a relative bpath. Because unlike the routerLink the navigate method doesn't know on which route we are currently sitting on. The routerLink always knows in which template it sits therefore it always knows what the currently loaded route is. So to tell navigate method where we currently are we need to a pass a second argument to the navigate method which is js object and here we can configure this navigation action and we can add **relativeTo** property.
- The **relativeTo** property defines relative to which route this link should be loaded and so to this relativeTo we have to pass route which we can inject here using route which is of type **ActivatedRoute** from @angular/router
- The ActivatedRoute simply injects the currently active route so for the comopinent you loaded this will be the route for this component as shown below
    ```ts
     onReload(){
        this.router.navigate(['servers'], {relativeTo: this.route}); //route is of type ActivatedRoute and is imported from @angular/router
    }
    ```
- And now upon clicking on the reload page button we will get an error because it will again look for servers/servers. So now lets comment this button and move on .
- Lets now add some more paths to our appRoutes array in the app.module.ts like loading a singke user component and for that we might want to load a user dynamically because if we look at our user.ts we have ar users array with different ids. So it would be nice if we are able to pass the id of the user that we want to load in our appRoutes path. And for this we can add parameters to our routes that is dynamic segments in our paths. And we do this by adding colon and then any name you want to give like in our case we are giveing it *id*.
    ```ts
    const appRoutes: Routes[
        {path: 'users/:id', component: UserComponent}
    ]
    ```
- You will later be able to retrieve the parameter loaded inside the component by the name you specified after the colon as in our case by *id*. The colon simply tells angular that this is a dynamic path of the path. So now if we now manually write http://localhost:4200/users/ssss (*here in place of ssss we can literally write anything because our path in the appRoutes will interprete as the id and it will load the single user components html page*)
- Now we want to use the data which is encoded in the url and to get access to this data in the ts file of the single user component we neeed to inject **ActivatedRoute** from the angular/router and by injecting this we get access tothe currently loaded route. And this currently loaded route is js object with a lot of metadata about the currently loaded route and one of this information is the currently active user
    ```ts
    // app.module.ts
    {path: 'users/:id/:name', component: UserComponent}
    //html 
    <p>User with ID {{user.id}} loaded.</p>
    <p>User name is {{user.name}}</p>
    // inside the single user ts
    export class UserComponent implements OnInit {
    user: {id: number, name: string};

    constructor(private route: ActivatedRoute) { }

    ngOnInit() {
        this.user = {
        id: this.route.snapshot.params['id'],
        name: this.route.snapshot.params['name']
        };
    }
- Here after injecting the Activated route we can call snapshot on which we can use params property which will provide us the parameters of the currently loaded route from the url.
- We load our data in the html by using this snapshot object on the route. And now if we load a new route then angular has a look at our app.module, finds the fitting route here, loads the component, initializes the component and gives the data by accessing the snapshot here and this only happens if haven't been on this component before. But we click this link which is on the user component then the url get changes but we are aklready on the component which should get loaded. So angular cleverly doesn't re-instatiate this component because it will only cost us the performance.
- Its fine to use the snapshot for the first time but to be able to update the subsequent changes we need a different approach as shown below
- If we add a link in the same component which is already loaded and on that link we are loading same component again but with different parameters then that willl not reflected in our html text 
    ```ts
    // html of user component
    <a [routerLink]="['/users', 10, 'case1']">Load Case1 (10)</a>

    // ts of user component
    ngOnInit() {
    this.user = {
      id: this.route.snapshot.params['id'],
      name: this.route.snapshot.params['name']
    };
    this.route.params
      .subscribe(
        (params: Params) => {
          this.user.id = params['id'];
          this.user.name = params['name'];
        }
      )
  }
    ```
- We can call params on the route directly where params is an observable to which we can subscribe and subscribe can take 3 arguments. First will be fired whenever the parameters changes where we can update our user objects refering to the params we paased in the callback or anonymous function. And now upon clicking on the link from the component itself the url also gets passed to the html code.
- This approach is not recommended when you surely know that your component will not get called again from the same component itself. In other case this is must.

#### How to pass query parametrs

- These are the ones seaparated by question marks and you can add multiples by using & sign also the # fragments separated with the # sign.
- Lets now add a route in our appRoutes which allows us to edit a certain server
    ```ts
    {path: 'servers/:id/edit', component: EditServerComponent}
    ```
- So we added a new route we should load the EditServerComponent. Now in the servers html lets add a routerLink which will have parameters as shown below.
    ```ts
    // html of servers
    <div class="col-xs-12 col-sm-4">
    <div class="list-group">
      <a [routerLink]="['/servers', 5, 'edit']" // added this which is similar to that of the appRoutes path
        href="#"
        class="list-group-item"
        *ngFor="let server of servers">
        {{ server.name }}
      </a>
    </div>
  </div>
  ```
- Now lets say we want to add query parameter deciding whether to allow edit server or not and for this we can add a new property of the routerlink directive called as **queryParams**
- And here we have to pass a js object and we can define key value of the parameters we want to add.
    ```ts
    <div class="col-xs-12 col-sm-4">
        <div class="list-group">
        <a [routerLink]="['/servers', 5, 'edit']"
            [queryParams]="{allowEdit: '1'}" // added
            href="#"
            class="list-group-item"
            *ngFor="let server of servers">
            {{ server.name }}
        </a>
        </div>
    </div>
  ```
- Now clicking upon any of the servers we will get a url some thing like this http://localhost:4200/servers/5/edit?allowEdit=1
- Also we can have the fragments as shown below and then the url will be like http://localhost:4200/servers/5/edit?allowEdit=1#loading
    ```ts
    <div class="col-xs-12 col-sm-4">
        <div class="list-group">
        <a [routerLink]="['/servers', 5, 'edit']"
            [queryParams]="{allowEdit: '1'}"
            fragment="loading" // added
            href="#"
            class="list-group-item"
            *ngFor="let server of servers">
            {{ server.name }}
        </a>
        </div>
    </div>
    ```
- Now lets go to our home html and there we have a button to load servers and now lets change it as onLoadServer(1) , we passed 1 as an argument which we now need to pass it on the ts home as shown below
    ```ts
    // html of home component
    <button class="btn-btn-primary" (click)="onLoadServers(1)">Load Servers 1</button>

    // ts of home component
    onLoadServers(id: number){
    this.router.navigate(['/servers', id, 'edit'], {queryParams: {allowEdit: '1'}, fragment: 'loading'});
    }
    ```
- Now on clicking on the load server 1 the all the things gets passed in the url dynamically. Now we are loading the EditServerComponment in the end and that is where we want to retrieve our data and for this lets inject ActivatedRoute and now in the ngOnInit we can use the snapshot on which we can call queryParams and fragments. 
    ```ts
    // ts of edit-server
     ngOnInit() {
    //method 1
    console.log(this.route.snapshot.queryParams);
    console.log(this.route.fragment);
    //method 2
    this.route.queryParams.subscribe();
    this.route.fragment.subscribe();
    this.server = this.serversService.getServer(1);
    this.serverName = this.server.name;
    this.serverStatus = this.server.status;
    }
    ```
- Now in the UsersComponent we can add a routerLink to in the users list using the user arrays user.id and user.name as which wll look into the app module to find the right path upon clicking this link in ther users html we will be redirected to the exact user with id and name as shown below.
    ```ts
    //users html
    <a
      [routerLink]="['/users', user.id, user.name]" // routerlink added
        href="#"
        class="list-group-item"
        *ngFor="let user of users">
        {{ user.name }}
    </a>
    ```
- A similar work can be done in the servers html with providing link to get redirected to the single server but along with it we need to providw a fitting path in the appRoutes of our appModule.ts
    ```ts
    // inside servers html
    <a
        [routerLink]="['/servers', server.id]"
        [queryParams]="{allowEdit: '1'}"
        fragment="loading"
        href="#"
        class="list-group-item"
        *ngFor="let server of servers">
        {{ server.name }}
    </a>
    // inside appmodule
    const appRoutes: Routes = [
    {path: '',component: HomeComponent},
    {path: 'users', component: UsersComponent},
    {path: 'users/:id/:name', component: UserComponent},
    {path: 'servers', component: ServersComponent},
    {path: 'servers/:id', component: ServerComponent}, // added for the router link
    {path: 'servers/:id/edit', component: EditServerComponent}
    ]
    // inside the single server ts
    ngOnInit() {
    const id = this.route.snapshot.params['id'];
    this.server = this.serversService.getServer(id);
    this.route.params
      .subscribe(
        (params: Params) => {
          this.server = this.serversService.getServer(params['id']);
        }
      );
    }
    ```
- If we pass a parameter from the url then it will always be a string because our whole url is simply just text so whatever we are trying to pass to our function from the params id it will be 100% a string. So if we try to search a server by this string id then it will go to the servers service where we have defined our server array but there it will not find the id 1 which is a string because there we have kept id as a number. So we simply need to convert this string id to a number by placing a + sign before it.
    ```ts
    ngOnInit() {
    const id = +this.route.snapshot.params['id']; // added +
    this.server = this.serversService.getServer(id);
    this.route.params
      .subscribe(
        (params: Params) => {
          this.server = this.serversService.getServer(+params['id']); // added +
        }
      );
    }
    ```
- But now upon clicking on the servers and users a new page is getting loaded but we want them to get loaded just beside the list of servers or beside the users menu and for that we need some nested routing or some kind of child routes to have router inside a router
    ```ts
    const appRoutes: Routes = [
    {path: '',component: HomeComponent},
    {path: 'users', component: UsersComponent},
    {path: 'users/:id/:name', component: UserComponent},
    {path: 'servers', component: ServersComponent},
    {path: 'servers/:id', component: ServerComponent},
    {path: 'servers/:id/edit', component: EditServerComponent}
    ]
    ```
- Like shown in the above we have 3 paths starting wth the same name that is servers 2 and users. So to avoid doing like this we can add another property children in the servers path where children takes another array of routes like shown below
    ```ts
    const appRoutes: Routes = [
        {path: '',component: HomeComponent},
        {path: 'users', component: UsersComponent},
        {path: 'users/:id/:name', component: UserComponent},
        {path: 'servers', component: ServersComponent, children: [
            {path: ':id', component: ServerComponent},
            {path: ':id/edit', component: EditServerComponent}
        ]},  
    ]
    ```
- But now upon clicking on any of the server in the list of the server it will throw an error that it *cannot find primary outlet to 'ServerComponent'* because the only router-outlet in our code is in the app.component.html and that is reserved for the routes only on the top level but the child routes of the app-servers need a separate outlet because they can't override the servers component instead they should be loaded nested into this servers component.
- So in the servers.html we need to add router-outlet which now adds a new hook which will be used on all child routes of the routes being loaded of the servers component which ofcourse is our /servers route. So now all the child routes will be loaded in place of router-outlet now as shown below.
    ```ts
    // in the html of servers
    <div class="col-xs-12 col-sm-4">
    /*<app-edit-server></app-edit-server>
    <hr>
    <app-server></app-server>*/
    <router-outlet></router-outlet>
    </div>
    ```
- 



















































### Routing Guards

- While creating routing links in some page, and upon clicking that link to navigate to that particular page, we first load the OnInit() function of that page because we have written some of the condition to be met upon loading of that page. But this could be cumbersome, so to avoid this checking of onInit functionality agian and again we have something called as routing-guards.  
- We can run some code at a point defined by us and this can be done by protecting routes with *canActivate*.  
- *canActivate* is interface which comes with method to be implemented  called as canActivate which takes two arguments as shown in the below snippet. The canActivate returns Observable and Promise so that it can react to both the behaviours that is either the synchronous or the asynchronous.  
- The Snippet Showing the Working of the the guard.  
  ```ts
  import { Injectable } from "@angular/core";
    import { 
        ActivatedRouteSnapshot,
        CanActivate,
        Router,
        RouterStateSnapshot
        } from "@angular/router";
    import { Observable } from "rxjs";
    import { AuthService } from "./auth.service";

    @Injectable()
    export class AuthGuard implements CanActivate{

        constructor(private authService: AuthService,
                    private router: Router){}
        canActivate(
            route: ActivatedRouteSnapshot, 
            state: RouterStateSnapshot): 
            Observable<boolean> | Promise<boolean> | boolean {
                return this.authService.isAuthenticated()
                    .then(
                        (authenticated: boolean) => {
                            if(authenticated){
                                return true;
                            }else{
                                this.router.navigate(['/'])
                            }


                        }
                    )
        }
    }
  ```
- The secondary service to check whether the person is logged in or not. This service is a dummy one.  
  ```ts
   export class AuthService{
        loggedIn = false;

        isAuthenticated(){
            const promise = new Promise(
                (resolve, reject) => {
                    setTimeout(() => {
                        resolve(this.loggedIn)
                    },800);
                }
            );return promise;
        }

        logIn(){
            this.loggedIn =true;
        }


        logOut(){
            this.loggedIn = false;
        }
    }
  ```

- And now we have to apply the canActivate to the paths that we want to restrict with the help of the services that we just created above.  
  ```ts
  {path: 'servers', canActivate:[AuthGuard], component: ServersComponent, children:[
      {path: ':id', component: ServerComponent},
      {path: ':id/edit', component: EditServerComponent}
    ]},
  ```  

### Guarding the child components

- Just now above we have seen that the routing to the component can be guarded by using the *canActivate* interface. But if we want to guard the child component then applying this *canActivate* method to each and every chuild path will be troublesome. So to avoid applying again and again of this *canActivate* we have other interface called as *canActivateChild* as shown in the below two snippets.  
  ```ts
  //canActivate:[AuthGuard],
      canActivateChild: [AuthGuard],
      component: ServersComponent,
      children:[
      {path: ':id', component: ServerComponent},
      {path: ':id/edit', component: EditServerComponent}
    ]},
  ```

  ```ts
   canActivateChild(
        route: ActivatedRouteSnapshot, 
        state: RouterStateSnapshot): 
        Observable<boolean> | Promise<boolean> | boolean {
            return this.canActivate(route, state);
        }
  ```
