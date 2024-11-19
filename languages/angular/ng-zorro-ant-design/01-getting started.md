# Getting started

* [Installation](#installation)
* [Create a New Project](#create-a-new-project)
* [Install ng-zorro-antd](#install-ng-zorro-antd)
* [Development & Debugging](#development-&-debugging)
* [Customized Work Flow](#customized-work-flow)
* [Customized Work Flow](#customized-work-flow)
    * [Install ng-zorro-antd](#install-customized-ng-zorro-antd)
    * [Import Styles](#import-styles)
        * [Use all component styles](#use-all-component-styles)
        * [Use Only Certain Component Style](#use-only-certain-component-style)
        * [Import Component Module](#import-component-module)

## Installation

> We strongly recommended to develop Angular with @angular/cli, you can install it with the following commands.
> Read the documentation of Angular to explore more features.

``$ npm install -g @angular/cli``

## Create a New Project

> A new project can be created using Angular CLI tools.

``$ ng new PROJECT-NAME``

> @angular/cli will run npm install after a project is created. If it fails, you can run npm install by yourself.

## Install ng-zorro-antd

ng-zorro-antd support init configuration with schematics, you can get more info in the schematics part.

```
$ cd PROJECT-NAME
$ ng add ng-zorro-antd
```

![new ng project](img/new%20ng%20project.svg)

## Development & Debugging

Run your project now, you can see the img below now.

```
$ ng serve --port 0 --open
```

![ng zorro complete](img/ng%20zorro%20compete.png)

## Customized Work Flow

If you want to customize your work flow, you can use any scaffold available in the Angular ecosystem. If you encounter problems, you can use our config and modify it.

### Install customized ng-zorro-antd

```
$ npm install ng-zorro-antd --save
```

### Import Styles

#### Use all component styles

This configuration will include all the styles of the component library. If you want to use only certain components, please see Use Only Certain Component Style configuration.

Import the pre-build styles in angular.json

```json
{
  "styles": [
    "node_modules/ng-zorro-antd/ng-zorro-antd.min.css"
  ]
}
```

Import the pre-build styles in style.css

```css
@import "~ng-zorro-antd/ng-zorro-antd.min.css";
```

Import the less styles in style.less

```css
@import "~ng-zorro-antd/ng-zorro-antd.less";
```

#### Use Only Certain Component Style

Importing CSS files of several components may result in code redundancy because style files of components have dependency relationships like TypeScript files.

Need import the styles of a base(styles common to all components) before using certain components.

Import the pre-build styles in style.css

```css
@import "~ng-zorro-antd/style/index.min.css"; /* Import basic styles */
@import "~ng-zorro-antd/button/style/index.min.css";  /* Import styles of the component */
```

Import the less styles in style.less

```css
@import "~ng-zorro-antd/style/entry.less"; /* Import basic styles */
@import "~ng-zorro-antd/button/style/entry.less";  /* Import styles of the component */
```

#### Import Component Module

Finally, you need to import the component modules you want to use into your app.module.ts file and feature modules.

Take the following Button Module module as an example, first introduce the component module:

```javascript
import { NgModule } from '@angular/core';
import { NzButtonModule } from 'ng-zorro-antd/button';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    NzButtonModule
  ]
})
export class AppModule { }
Then use in the template:

<button nz-button nzType="primary">Primary</button>
```