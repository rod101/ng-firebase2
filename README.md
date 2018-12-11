# NgFirebase2 - Angular with Google's Firebase, Cloud Firestore

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 7.1.2.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).


1. Installation and Setup

0. Prerequisites
AngularFire provides multiple module formats for different types of builds. The guide is based on the Angular CLI. It is possible to do a manual setup with Webpack or a SystemJS build as well.


## Angular 7 (or even 6)? 
This version of firebase work with Angular 6 and up, version 7.1.2 being the latest at the time this project was setup

Create a new project in Angular 7

```
npm install @angular/cli
ng new <project-name>
cd <project-name>
```

Create a new project in Angular 6

```
npm install -g @angular/cli@6
ng new <project-name>
cd <project-name>
```

2. Test your setup

```
ng serve
open http://localhost:4200
```

You should see a message on the page with the Angular logo

3. Install AngularFire and Firebase
Now that you have a new project setup, install AngularFire and Firebase from npm.

```
npm install @angular/fire firebase --save
```

4. Starting a New Cloud Firestore Project
Login to your Google account and start a new tab on your browser. Visit the [Firebase page](https://firebase.google.com) and click the Get Started button. 

Click Add project 

![alt text](https://s3.amazonaws.com/coursetro/posts/content_images/1-1508018079648.png)

Next, give it a project name and hit Create Project 

![alt text](https://s3.amazonaws.com/coursetro/posts/content_images/2-1508018085160.png)

Click on the Add Firebase to your web app button and copy the following properties and values. Save them in a text document for later use: 
This will be added to angular: /src/environments/environment.ts

![alt text](https://s3.amazonaws.com/coursetro/posts/content_images/3-1508018089946.png)

Click on the Database menu to the left, and then choose Try Firestore Beta (Note: It is currently in beta at the time of writing this tutorial, but that may have changed. This process should still remain the same).

![alt text](https://s3.amazonaws.com/coursetro/posts/content_images/4-1508018094970.png)

On the next screen shown below, choose test mode and then click Enable

![alt text](https://s3.amazonaws.com/coursetro/posts/content_images/5-1508018099889.png)

On the next screen, click Add Collection and name the collection "posts"

![alt text](https://s3.amazonaws.com/coursetro/posts/content_images/6-1508018105129.png)

Then, add 2 fields of type string (title, content), and provide them with some initial value. Then, click Save

![alt text](https://s3.amazonaws.com/coursetro/posts/content_images/7-1508018109730.png)

![alt text](https://s3.amazonaws.com/coursetro/posts/content_images/8-1508018113910.png)

That's all there is to the setup of our Firestore project for now.



5. Add Firebase config to environments variable
Open /src/environments/environment.ts and add your Firebase configuration. 
I hope you did copy the code when you were setting up firebase, If not.

You can find your project configuration in the Firebase Console. From the project overview page, click Add Firebase to your web app.

```
export const environment = {
  production: false,
  firebase: {
    apiKey: '<your-key>',
    authDomain: '<your-project-authdomain>',
    databaseURL: '<your-database-URL>',
    projectId: '<your-project-id>',
    storageBucket: '<your-storage-bucket>',
    messagingSenderId: '<your-messaging-sender-id>'
  }
};
```

6. Setup @NgModule for the AngularFireModule
Open /src/app/app.module.ts, inject the Firebase providers, and specify your Firebase configuration.

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';
import { AngularFireModule } from '@angular/fire';
import { environment } from '../environments/environment';

@NgModule({
  imports: [
    BrowserModule,
    AngularFireModule.initializeApp(environment.firebase)
  ],
  declarations: [ AppComponent ],
  bootstrap: [ AppComponent ]
})
export class AppModule {}
```

7. Inject AngularFirestore

Open /src/app/app.component.ts, and make sure to modify/delete any tests to get the sample working (tests are still important, you know):

```
import { Component } from '@angular/core';
import { AngularFirestore } from '@angular/fire/firestore';
import { Observable } from 'rxjs';

@Component({
  selector: 'app-root',
  templateUrl: 'app.component.html',
  styleUrls: ['app.component.css']
})
export class AppComponent {
  posts: Observable<any[]>;
  constructor(db: AngularFirestore) {
    this.posts = db.collection('posts').valueChanges();
  }
}
```

8. Open /src/app/app.component.html:

```
<ul>
  <li class="text" *ngFor="let post of posts | async">
    {{post.title}}
  </li>
</ul>
```

9. Run your app

```
ng serve
```


